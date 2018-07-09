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

关于 jupyter 的使用方法，可点击 jupyter notebook 中的 help 菜单查看或访问[在线文档][ref01]。

[ref01]:http://ipython.org/notebook.html "http://ipython.org/notebook.html"

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
    er = 1
    while er > 0.00001:
        x0 = eqx(x1, x2)
        y1 = eq(x1)
        y0 = eqy(x1, x2)
        if y1*y0 < 0:
            x2 = x0
        else:
            x1 = x0
        er = math.fabs(x2-x1)
    print "(" + str(x0) + "," + str(er) + ")"

输出为：

    (0.682334899902,7.62939453125e-06)

### 迭代法

    #!/usr/bin/env python
    import math
    x0 = 1.3
    def eq(x):
        x1 = 2-math.log(x)
        return x1
    x1 = eq(x0)
    i = 0
    while math.fabs(x0-x1) > 0.00000000001:
        i += 1
        x0 = x1
        x1 = eq(x0)
    print x1
    print i

### 加速迭代法

    #!/usr/bin/env python
    import math
    x0 = 1.3
    def eq(x):
        x1 = 2-math.log(x)
        return x1
    x1 = eq(x0)
    x2 = eq(x1)
    i = 0
    while math.fabs(x2-x1) > 0.00000000001:
        if x2-2*x1+x0 == 0:
            break
        i += 1
        x0 = x2-((x2-x1)**2)/(x2-2*x1+x0)
        x1 = eq(x0)
        x2 = eq(x1)
    print x2
    print i

### 牛顿切线法

    #!/usr/bin/env python
    import math
    x0 = 1.3
    def eq(x):
        x1 = x-(x+math.log(x)-2)/(1+(1/x))
        return x1
    x1 = eq(x0)
    x2 = eq(x1)
    i = 0
    while math.fabs(x2-x1) > 0.00000000001:
        if x2-2*x1+x0 == 0:
            break
        i += 1
        x0 = x2-((x2-x1)**2)/(x2-2*x1+x0)
        x1 = eq(x0)
        x2 = eq(x1)
    print x2
    print i

### 弦位迭代法

    #!/usr/bin/env python
    import math
    x0 = 1.0
    x1 = 2.0
    def eq(x):
        y = x+math.log(x)-2
        return y
    def eqx(x0, x1):
        x2 = x1-eq(x1)*(x1-x0)/(eq(x1)-eq(x0))
        return x2
    x2 = eqx(x0, x1)
    i = 0
    while math.fabs(x2-x1) > 0.00000000001:
        i += 1
        x0 = x1
        x1 = x2
        x2 = eqx(x0, x1)
    print x2
    print i

### 二分法




## 线性方程组的解法(numpy)

    #!/usr/bin/env python
    import numpy as np
    a = np.array([[10,-1,-2],[-1,10,-2],[-1,-1,5]])
    # a = np.mat("10 -1 -2; -1 10 -2; -1 -1 5")
    b = np.array([7.2, 8.3, 4.2])
    x = np.linalg.solve(a, b)
    print x
    r = np.allclose(np.dot(a, x), b)
    print r

## 插值方法

### 拉格朗日插值

    #!/usr/bin/env python
    import math
    import numpy as np
    n = 4
    xk = 0.20
    x = [0.10, 0.15, 0.25, 0.30]
    y = [0.904837, 0.860708, 0.778801, 0.740818]
    l = []
    result = 0
    for i in range(n):
        l.append(1)
        for j in range(n):
            if i != j:
                ll = (xk-x[j])/(x[i]-x[j])
            else:
                ll = 1
            l[i] = l[i]*ll
        result = result + y[i]*l[i]
    print result
