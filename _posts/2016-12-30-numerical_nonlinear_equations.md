---
layout: post
title: 非线性方程数值解法
---

### 1. 使用 jupyter 调试程序

本文及其后的几篇总结介绍 python 在科学计算中的基本应用方法。

建议安装 jupyter，使用 `jupyter notebook` 练习语法和调试程序：

    $ pip3 install --upgrade jupyter  # 安装jupyter
    $ python3 -c "import jupyter"  # 查看是否安装成功
    $ jupyter notebook  # 启动jupyter服务

在浏览器中访问 http://localhost:8888 就可以打开 jupyter notebook。

关于 jupyter 的使用方法，可查看 jupyter notebook 中的 help 菜单或[在线文档][ref01]。

[ref01]:http://ipython.org/notebook.html "http://ipython.org/notebook.html"

说明：这几篇总结中的 python 代码仅仅用于展示算法的实现方法，没有在风格和效率方面进行优化。

### 2. 问题背景

科学研究和工程实践中，求解方程(组)是最常用的数学过程，几百年来，工程师和数学家研究了各种各样的方程解法。

对于方程 \\( f(x) = 0 \\)，当\\( f(x) \\)为线性函数时，称之为线性方程，当\\( f(x) \\)为非线性函数时，称之为非线性方程。

线性方程的求解方法很多，后文将开专题详细介绍。

当\\( f(x) \\)为\\( n \\)次代数多项式时，称上述方程为\\( n \\)次代数方程，当\\( n \geqslant 5 \\)时，没有求根公式可用；当\\( f(x) \\)含有三角函数、指数函数、对数函数等形式时，相应的方程称为超越方程，这类方程的精确求解一般是不可行的，甚至解的存在性、解的个数也难以判断。

工程上，根据对具体施工的精度要求，设计数值解法，可以满足计算要求，解决很多实际问题。

### 3. 迭代方法

### 3.1 二分法

设函数\\( f(x) \\)在区间\\( [a, b] \\)上连续，且\\( f(a) \cdot f(b) < 0 \\)，可知方程\\( f(x) = 0 \\)在\\( [a, b] \\)内一定有解。

记 \\( x ^*\\) 为方程的精确解，对于预先给定的精度\\( \varepsilon > 0 \\)，只要二分次数\\( n \\)满足

>\\( n > \frac{\ln (b-a) - \ln (2 \varepsilon)}{\ln 2} \\)

便有

>\\( \vert x ^* - x _n \vert < \varepsilon \\)

\\( x _n \\)就是满足精度要求的近似解。

- 例3.1 使用二分法求方程\\( x ^3 + x - 1 = 0 \\)在\\( [0,1] \\)内的近似解，误差\\( < 10 ^{-5} \\)。

数值解法：

    #!/usr/bin/env python
    import math
    x1 = 0.0
    x2 = 1.0
    def eq(x):
        y = x ** 3 + x - 1
        return y
    def eqx(x1, x2):
        x0 = (x1+x2)/2
        return x0
    def eqy(x1, x2):
        x0 =eqx(x1, x2)
        y0 = eq(x0)
        return y0
    i = 0
    err = 1
    while err > 0.00001:
        x0 = eqx(x1, x2)
        y1 = eq(x1)
        y0 = eqy(x1, x2)
        if y1*y0 < 0:
            x2 = x0
        else:
            x1 = x0
        err = math.fabs(x2-x1)
        i += 1
    print "迭代"+str(i)+"次：" + str((x0,err))

输出为：

    迭代17次：(0.6823348999023438, 7.62939453125e-06)

### 3.2 迭代法

将方程\\( f(x)=0 \\)经过简单的变形处理，转化为\\( x = \varphi (x) \\)的形式，据此构造迭代格式

\\( x _{n+1} = \varphi (x _n) \\)

对某个选定的初值\\( x_0 \\)进行迭代，得到一个迭代数列\\( \lbrace x_n \rbrace \\)，如果该数列存在极限，即

\\( \lim _{n \rightarrow \infty} x _n = x ^* \\)

\\( x ^* \\)就是原方程的解。

- 例3.2 设计不同迭代算法求解方程\\( x^2 = 2 \\)的正数解。

方法1：\\( x _{n+1} = \frac{2}{x _n} \\)

方法2：\\( x _{n+1} = \frac{1}{2}(x _n + \frac{2}{x _n}) \\)

方法3：\\( x _{n+1} = \sqrt[3] {2x_n - x ^2 _n +2} \\)

方法4：\\( x _{n+1} = 1 + \frac{1}{1+x _n} \\)

选用初值\\( x=1 \\)进行计算：

    #!/usr/bin/env python
    x0 = 1.0

    def eq1(x):
        y = 2.0/x
        return y
    def eq2(x):
        y = 0.5*(x+2.0/x)
        return y
    def eq3(x):
        y = (2.0*x - x**2 +2.0)**(1.0/3.0)
        return y
    def eq4(x):
        y = 1.0+1.0/(1.0+x)
        return y

    n = [1,2,3,4,5,6,7,8,9,10,11]

    for m in n:
        for i in range(m):
            if i==0:
                x_1 = x_2 = x_3 = x_4 = x0
            else:
                x_1 = eq1(x_1)
                x_2 = eq2(x_2)
                x_3 = eq3(x_3)
                x_4 = eq4(x_4)
        print "迭代"+str(m)+"次：" + str((x_1, x_2, x_3, x_4))

输出：

    迭代1次：(1.0, 1.0, 1.0, 1.0)
    迭代2次：(2.0, 1.5, 1.4422495703074083, 1.5)
    迭代3次：(1.0, 1.4166666666666665, 1.4102002159497848, 1.4)
    迭代4次：(2.0, 1.4142156862745097, 1.4147647905038103, 1.4166666666666667)
    迭代5次：(1.0, 1.4142135623746899, 1.4141373989066883, 1.4137931034482758)
    迭代6次：(2.0, 1.414213562373095, 1.4142240773083499, 1.4142857142857144)
    迭代7次：(1.0, 1.414213562373095, 1.4142121105435805, 1.4142011834319526)
    迭代8次：(2.0, 1.414213562373095, 1.4142137628285405, 1.4142156862745099)
    迭代9次：(1.0, 1.414213562373095, 1.4142135346959663, 1.4142131979695431)
    迭代10次：(2.0, 1.414213562373095, 1.414213566194509, 1.4142136248948696)
    迭代11次：(1.0, 1.414213562373095, 1.4142135618454679, 1.4142135516460548)

可以看到方法1不收敛。

- 例3.3 分别设计两种不同迭代算法求解以下方程：

(1) \\( x^3 - x^2 - x - 1 = 0 \\)

方法1：\\( x _{n+1} = \sqrt[3] {x ^2 _n + x _n + 1} \\)

方法2：\\( x _{n+1} = 1 + \frac{1}{x _n} + \frac{1}{x ^2 _n} \\)

(2) \\( 5x - e ^x = 0 \\)

方法1：\\( x _{n+1} = \frac{e ^{x_n}}{5} \\)

方法2：\\( x = \frac{1}{10} (5x_n + e ^{x_n}) \\)

程序：

    #!/usr/bin/env python
    import math
    x0 = 2.0

    def eq1_1(x):
        y = (x**2.0+x+1.0)**(1.0/3.0)
        return y
    def eq1_2(x):
        y = 1.0+1.0/x+1.0/(x**2.0)
        return y
    def eq2_1(x):
        y = math.e ** x/5.0
        return y
    def eq2_2(x):
        y = 0.1*(5.0*x+math.e**x)
        return y
    err = 1
    i = 0
    xn = x0
    while err > 0.000001:
        x = xn
        xn = eq1_1(xn) # xn = eq1_2(xn);xn = eq2_1(xn);xn = eq2_2(xn)
        i += 1
        err = math.fabs(x-xn)
    print "3_3(1)方法1：迭代" + str(i) + "次，" + str((xn, err))

输出：

    3_3(1)方法1：迭代16次，(1.8392874164327768, 7.731202917860003e-07)
    3_3(1)方法2：迭代27次，(1.8392864276650116, 8.584023183377809e-07)
    3_3(2)方法1：迭代14次，(0.25917119358536855, 2.6230974975449683e-07)
    3_3(2)方法2：迭代33次，(0.25917224855675247, 6.746787469258386e-07)

- 例3.4 求解方程\\( x + \ln x = 2 \\)。

迭代法求解：

    #!/usr/bin/env python
    import math
    x0 = 1.3
    def eq(x):
        x1 = 2-math.log(x)
        return x1
    x1 = eq(x0)
    i = 0
    err = math.fabs(x0-x1)
    while err > 0.00000000001:
        x0 = x1
        x1 = eq(x0)
        i += 1
        err = math.fabs(x0-x1)
    print "迭代" + str(i) + "次，" + str((x1, err))

输出：

    迭代56次，(1.5571455990005743, 7.576606009251918e-12)

迭代法有很多加速方法，如：

    #!/usr/bin/env python
    import math
    x0 = 1.3
    def eq(x):
        x1 = 2-math.log(x)
        return x1
    x1 = eq(x0)
    x2 = eq(x1)
    i = 0
    err = math.fabs(x2-x1)
    while err > 0.00000000001:
        if x2-2*x1+x0 == 0:
            break
        x0 = x2-((x2-x1)**2)/(x2-2*x1+x0)
        x1 = eq(x0)
        x2 = eq(x1)
        i += 1
        err = math.fabs(x2-x1)
    print "迭代" + str(i) + "次，" + str((x2, err))

输出：

    迭代3次，(1.5571455989978924, 7.185363415374013e-13)

### 3.4 牛顿法

设函数\\( f(x) \\)在区间\\( [a, b] \\)上有二阶导数，\\( f(a) \cdot f(b) < 0 \\)，且\\( f' (x) \\)、\\( f ^{\prime \prime} (x) \\)在区间\\( [a, b] \\)内不变号，则方程\\( f(x) = 0 \\)在\\( [a, b] \\)内有且只有一个实根。

设\\( x _0 \\)为\\( f(x) \\)上的一点，则过\\( x _0 \\)的切线方程为

>\\( y = f (x_0) + f'(x_0)(x - x_0) \\)

令\\( y = 0 \\)，得到切线与\\( x \\)轴的交点为

>\\( x = x_0 - \frac{f(x_0)}{f'(x_0)} \\)

据此可构造迭代关系式

>\\( x _{n+1} = x_n - \frac{f(x_n)}{f'(x_n)} \\)

这种迭代方法称为牛顿切线法。

- 例3.5 使用牛顿切线法求方程\\( x ^3 + x - 1 = 0 \\)在\\( [0, 1] \\)内的近似解。

该方程的迭代关系式为：

>\\( x _{n+1} = x_n - \frac{x _n ^3 + x _n - 1}{3x ^2 _n + 1} \\)

牛顿法求解：

    #!/usr/bin/env python
    import math
    x0 = 0.5
    def eq(x):
        x1 = x - (x ** 3.0 + x -1.0)/(3.0 * x ** 2 + 1)
        return x1
    i = 0
    err = 1.0
    while err > 0.00001:
        if i == 0:
            x = eq(x0)
        else:
            x0 = x
            x = eq(x)
            err = math.fabs(x0-x)
        i += 1
    print "迭代" + str(i) + "次，" + str((x, err))

输出：

    迭代4次，(0.6823278038283471, 6.194762312006219e-07)

对比例3-1，牛顿法的收敛速度快于二分法。

### 3.5 弦位迭代法

牛顿法的每一步都需要计算导数值，当导数计算困难、导数值非常小或方程有多重根时，牛顿迭代可能会失效，此时可使用割线代替切线，若取

>\\( f' (x_n) = \frac{f(x_n)-f(x_0)}{x_n - x_0} \\)

得到单点弦位迭代式

>\\( x _{n+1} = x_n - \frac{f(x_n)(x_n - x_0)}{f(x_n) - f(x_0)} \\)

若取

>\\( f' (x _n) = \frac{f(x _n)-f(x _{n-1})}{x _n - x _{n-1}} \\)

得到两点弦位迭代式

>\\( x _{n+1} = x_n - \frac{f(x_n)(x_n - x _{n-1})}{f(x _n) - f(x _{n-1})} \\)

- 例3.6 使用两点弦位迭代法求方程\\( x ^3 + x - 1 = 0 \\)在\\( x _0 = 2 \\)附近的近似解。

两点弦位迭代法：

    #!/usr/bin/env python
    import math
    x0 = 2.0
    x1 = 1.9
    def f(x):
        y = (x ** 3.0 - x * 3.0 -1.0)
        return y
    def eq(x0,x1):
        x2 = x1-f(x1)*(x1-x0)/(f(x1)-f(x0))
        return x2
    i = 0
    err = 1.0
    x2 = eq(x0,x1)
    while err > 0.00001:
        x0 = x1
        x1 = x2
        x2 = eq(x0,x1)
        err = math.fabs(x2-x1)
        i += 1
    print "迭代" + str(i) + "次，" + str((x2, err))

输出：

    迭代3次，(1.8793852415724437, 3.2711481523506336e-08)

### 4. 迭代加速

### 4.1 松弛法

在迭代计算过程中，当迭代发散或收敛很慢时，可以使用松弛法进行迭代加速。松弛法是指由已经构造的迭代格式\\( x _{n+1} = \varphi (x _n) \\)，选取松弛因子\\( \omega \\)，进行处理，得到新的迭代格式为

>\\( x _{n+1} = \omega \varphi (x_n) + (1- \omega ) x_n \\)

设

>\\( h(x) = \omega \varphi (x) + (1- \omega ) x \\)

在满足\\( \vert h'(x) \vert < 1 \\)条件下，迭代收敛。令\\( h'(a)=0 \\)，得到

>\\( \omega = \frac{1}{1- \varphi ' (a) } \\)

用\\( x_n \\)代替\\( a \\)，得

>\\( \omega = \frac{1}{1- \varphi ' (x _n) } \\)

由此得到松弛加速迭代格式

>\\( x _{n+1} = \frac{ \varphi (x_n) - x_n \varphi ' (x_n)}{1 - \varphi ' (x_n)} \\)

- 例4.1 采用松弛加速迭代算法求解以下方程：

(1) \\( x^3 - x^2 - x - 1 = 0 \\)

其松弛加速迭代格式为

>\\( x _{n+1} = \frac{ -2 x _n ^3 + x _n ^2 - 1 }{ -3 x _n ^3 + 2x _n + 1 } \\)

程序：

    #!/usr/bin/env python
    import math
    x0 = 3.0

    def eq(x):
        y = (-2.0 * (x**3) + x**2 -1.0)/(-3.0 * (x**2) + x*2.0 + 1.0)
        return y
    err = 1
    i = 0
    while err > 0.000001:
        if i == 0:
            x = x0
        else:
            x0 = x
            x = eq(x)
            err = math.fabs(x-x0)
        i += 1        
    print "迭代" + str(i) + "次，" + str((x, err))

输出：

    迭代7次，(1.8392867552141607, 3.9248069239050665e-09)

对比例3-3(1)，可以看到，得到同样精度的解，迭代次数从16次下降到7次。

(2) \\( 5x - e ^x = 0 \\)

其松弛加速迭代格式为

>\\( x _{n+1} = \frac{ (1 - x _n) e ^{x _n} }{ 5 - e ^{x _n} } \\)

程序：

    #!/usr/bin/env python
    import math
    x0 = 1.0

    def eq(x):
        y = ((1-x)*(math.e**x))/(5-math.e**x)
        return y
    err = 1
    i = 0
    while err > 0.000001:
        if i == 0:
            x = x0
        else:
            x0 = x
            x = eq(x)
            err = math.fabs(x-x0)
        i += 1        
    print "迭代" + str(i) + "次，" + str((x, err))

输出：

    迭代6次，(0.25917110181907377, 3.716360552630249e-11)

### 4.2 Aitken法

松弛加速需要计算\\( \varphi \prime (x _n) \\)，Aitken 法用差商替代导数，简化了计算。

设迭代格式为\\( x _{n+1} = \varphi (x_n) \\)，\\( x ^* \\)为方程\\( x = \varphi (x) \\)的解，即\\( x ^* = \varphi (x ^*) \\)，则

>\\( x _{n+1} - x ^* = \varphi (x _n) - \varphi ( x ^* ) = \varphi \prime ( \xi ) ( x _n - x ^* ) \\)

使用差商\\( \frac{ \varphi (x_n) - \varphi (x _{n-1}) }{ x _n - x _{n-1} } \\)近似替代\\( \varphi \prime ( \xi ) \\)，则

>\\( x _{n+1} - x ^* = \frac{x _{n+1} - x _n}{x _n - x _{n-1}} (x _n - x ^*) \\)

得到

>\\( x ^* = x _{n+1} - \frac{(x _{n+1} - x _n)^2}{x _{n+1} -2x _n + x _{n-1}} \\)

设计 Aitken 迭代公式为

>\\( x _k ^{(1)} = \varphi (x _k) \\)

>\\( x _k ^{(2)} = \varphi (x _k ^{(1)}) \\)

>\\( x _{k+1} = x _k ^{(2)} - \frac{(x _k ^{(2)} - x _k ^{(1)})^2}{x _k ^{(2)} -2x _k ^{(1)} + x _k} \\)

- 例4.2 已知\\(x = 2\\)为方程\\( x ^3 - 5x + 2 = 0 \\)的一个根，设计 Aitken 加速迭代格式进行验证。

由原方程可得\\( x = \frac{x ^3 + 2}{5} \\)，因此\\( x _{n+1} = \frac{x _n ^3 + 2}{5} \\)，与之对应的 Aitken 加速迭代式为

>\\( x _k ^{(1)} = \frac{x _k ^3 + 2}{5} \\)

>\\( x _k ^{(2)} = \frac{ (x _k ^{(1)}) ^3 + 2}{5} \\)

>\\( x _{k+1} = x _k ^{(2)} - \frac{(x _k ^{(2)} - x _k ^{(1)})^2}{x _k ^{(2)} -2x _k ^{(1)} + x _k} \\)

程序：

    #!/usr/bin/env python
    import math
    x0 = 2.5

    def eq(x):
        y = (x**3 + 2.0)/5.0
        return y
    def aitken(x0,x1,x2):
        y = x2 - ((x2-x1)**2)/(x2 - 2.0*x1 + x0)
        return y

    err = 1
    i = 0
    x = x0
    while err > 0.000001:
        x0 = x
        x1 = eq(x)
        x2 = eq(x1)
        x = aitken(x,x1,x2)
        err = math.fabs(x-x0)
        i += 1        
        print "Aitken加速：迭代" + str(i) + "次，" + str((x, err))

    j = 0
    x0 = 2.5
    while j < 6:
        if j == 0:
            x = eq(x0)
        else:
            x = eq(x)
        j += 1        
        print "未加速：迭代" + str(j) + "次，" + str(x)

输出：

    Aitken加速：迭代1次，(2.272101942691977, 0.22789805730802293)
    Aitken加速：迭代2次，(2.1021712171330007, 0.16993072555897637)
    Aitken加速：迭代3次，(2.0180937910816468, 0.08407742605135393)
    Aitken加速：迭代4次，(2.0006517327321096, 0.01744205834953716)
    Aitken加速：迭代5次，(2.000000872732025, 0.0006508600000847409)
    Aitken加速：迭代6次，(2.0000000000015667, 8.727304581235273e-07)
    未加速：迭代1次，3.525
    未加速：迭代2次，9.160065625
    未加速：迭代3次，154.118363007
    未加速：迭代4次，732138.753077
    未加速：迭代5次，7.84892503934e+16
    未加速：迭代6次，9.67075853845e+49

可以看到原迭代格式是不收敛的，而 Aitken 格式则收敛。

### 5. 使用 scipy.optimize.fsolve() 求解非线性方程

用 python 的 scipy.optimize 库中 fsolve 函数可以求解常见的非线性方程(组)，可以看到求解非线性方程(组)的问题实际上是一个优化问题。

- 例5.1 求解方程\\( x ^3 + x - 1 = 0 \\)。

程序：

    #!/usr/bin/env python
    from scipy.optimize import fsolve
    import math
    def f(x):
        return x ** 3.0 + x - 1.0
    result = fsolve(f, 0)
    print result

输出：

    [ 0.6823278]

- 例5.2 求解方程组

\\[ \begin{cases} x ^2 + 2y = 1 \\\\
     3x - 4y = 0 \end{cases} \\]

程序：

    #!/usr/bin/env python
    from scipy.optimize import fsolve
    def f(x):
        x0 = float(x[0])
        x1 = float(x[1])
        return [
            x0 ** 2 + x1 *2 -1,
            x0 *3 - x1 * 4
            ]
    result1 = fsolve(f, [-1.5,1])
    result2 = fsolve(f, [2.0,1])
    print result1
    print result2

输出：

    [-2.  -1.5]
    [ 0.5    0.375]

- 例5.3 求解方程\\( \cos (x) = \sin (x) e ^x \\)。

程序：

    #!/usr/bin/env python
    from scipy.optimize import fsolve
    from math import sin,cos,e
    def f(x):
        x = float(x)
        return [
            cos(x)  - sin(x)*(e**x)
        ]
    result = fsolve(f, 1)
    print result
    print f(result)

输出：

    [ 0.53139086]
    [0.0]
