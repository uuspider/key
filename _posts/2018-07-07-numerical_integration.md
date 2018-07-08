---
layout: post
title: 数值积分实验
---

### 1. 问题的提出

对于定积分

>\\(\int _a ^b f (x) {\rm d} x \\)

如果被积函数\\(f(x)\\)具有初等函数形式的原函数\\(F(x)\\)，即

>\\( F'(x) = f (x) \\)

可以利用Newton-Leibniz公式计算理论解(准确解)：

>\\(\int _a ^b f (x) {\rm d} x = F(x) \vert _a ^b = F(b)-F(a) \\)

但是，如果\\(f(x)\\)不具有初等函数形式的原函数或难以确定其原函数的具体形式，就需要使用数值方法计算定积分。

### 2. 数值积分方法

### 2.1 一元函数积分

设函数\\( y=f(x) \\)在\\([a, b]\\)上有界，在区间\\([a, b]\\)上任取\\(n+1\\)个分点：

>\\(a=x_0<x_1<x_2<\cdots<x_{n-1}<x_n=b\\)

这些点把\\([a, b]\\)分成\\(n\\)个小区间

>\\( \Delta _i = [x _{i-1}, x _{i}], i=1,2,\cdots,n \\)

小区间的长度为\\( \Delta x_{i} = x_{i} - x_{i-1} \\)，取\\( \lambda = \max \limits_{ 1 \leqslant i \leqslant n} \Delta x_i \\)。

在区间\\( \Delta _i \\)上任意取点\\( \xi _i \in [x _{i-1}, x _i] \\)，记

>\\( S = \sum _{i=1} ^n f( \xi_i ) \Delta x_i \\)

当\\( \lambda \to 0 \\)时，若\\( S \\)的极限存在，就称这个极限为函数\\( f(x) \\)在区间\\([a, b]\\)上的定积分：

>\\( \int _a ^b f (x) {\rm d} x = \lim\limits _{ \lambda \to 0} \sum _{i=1} ^n f(\xi _i) \Delta x _i \\)

将\\([a, b]\\)进行等分，有

>\\( \Delta x _i = \frac{1}{n} (b-a) \\)

>\\( x _i = a + \frac{i}{n} (b-a), i=1,2, \cdots, n \\)

则

>\\( \int _a ^b f(x) {\rm d} x = \lim\limits _{n \to \infty} \sum _{i=1} ^n f(\xi_i) \\)

这就是数值计算中常用的机械求积公式。

若取\\( \xi _i = x _{i-1} \\)，得到左矩阵法：

>\\( \int _a ^b f(x) {\rm d} x \approx \frac{1}{n}(b-a) \sum _{i=1} ^n f( x _{i-1} ) \\)

若取\\( \xi _i = x _{i} \\)，得到右矩阵法：

>\\( \int _a ^b f(x) {\rm d} x \approx \frac{1}{n}(b-a) \sum _{i=1} ^n f(x_i) \\)

若取\\( \xi _i = \frac{x _{i-1} + x _i}{2} \\)，得到中矩阵法：

>\\( \int _a ^b f(x) {\rm d} x \approx \frac{1}{n}(b-a) \sum _{i=1} ^n f(\frac{x _{i-1} + x _i}{2}) \\)

若取\\( \xi _i = \frac{f(x _{i-1}) + f(x _i)}{2} \\)，得到梯形法：

>\\( \int _a ^b f(x) {\rm d} x \approx \frac{1}{2n}(b-a) \sum _{i=1} ^n [f(x _{i-1}) + f(x _i)] \\)

若取\\( \xi _i = \frac{f(x _{i-1}) + 4f(\frac{x _{i-1} + x _i}{2})+ f(x _i)}{6} \\)，得到Simpson法：

>\\( \int _a ^b f(x) {\rm d} x \approx \frac{1}{6n}(b-a) \sum _{i=1} ^n [f(x _{i-1}) + 4f(\frac{x _{i-1} + x _i}{2}) + f(x _i)] \\)

- 例1-1 使用矩形法、梯形法、Simpson法计算\\( \int _0 ^1 \frac{1}{1+x^2} {\rm d} x \\)。

理论解：

>\\( \int _0 ^1 \frac{1}{1+x^2} {\rm d} x = \arctan x \vert _0 ^1 = \frac{\pi}{4} \\)

数值解法：

    #!/usr/bin/env python
    import math
    RESULT = math.pi/4.0

    def f(x):
        return 1.0/(1+x**2)
    def x(i,n):
        return i/n
    # 中矩形法
    def rectangle_m(n):
        i = 1.0
        f_sum = 0.0
        while i <= n:
            f_sum += (f((x(i-1,n)+x(i,n))/2))
            i += 1
        result = f_sum / n
        err = abs(result - RESULT)
        err = "%.2e" % err
        result = "%.15f" % result
        return result,err
    # 梯形法
    def trapzoid(n):
        i = 1.0
        f_sum = 0.0
        while i <= n:
            f_sum += (f(x(i-1,n)) + f(x(i,n)))
            i += 1
        result = f_sum / (2*n)
        err = abs(result - RESULT)
        err = "%.2e" % err
        result = "%.15f" % result
        return result,err
    # simpson方法
    def simpson(n):
        i = 1.0
        f_sum = 0.0
        while i <= n:
            f_sum += (f(x(i-1,n)) + 4*f((x(i-1,n)+x(i,n))/2.0) + f(x(i,n)))
            i += 1
        result = f_sum / (6*n)
        err = abs(result - RESULT)
        err = "%.2e" % err
        result = "%.15f" % result
        return result,err

    print "理论解：","%.15f" % RESULT
    print "矩形法：",rectangle_m(50),rectangle_m(100),rectangle_m(200)
    print "梯形法：",trapzoid(50),trapzoid(100),trapzoid(200)
    print "Simpson法：",simpson(50),simpson(100),simpson(200)

输出为

    理论解： 0.785398163397448
    矩形法： ('0.785406496730751', '8.33e-06') ('0.785400246730781', '2.08e-06') ('0.785398684230781', '5.21e-07')
    梯形法： ('0.785381496730814', '1.67e-05') ('0.785393996730782', '4.17e-06') ('0.785397121730782', '1.04e-06')
    Simpson法： ('0.785398163397438', '9.99e-15') ('0.785398163397448', '1.11e-16') ('0.785398163397448', '5.55e-16')

可以看到Simpson法的精度显著优于矩形法和梯形法。

- 例1-2 (1) 使用Simpson法计算\\( \int _1 ^2 \frac{\sin x}{x} {\rm d} x \\)。

数值解法：

    #!/usr/bin/env python
    import math
    def f(x):
        return math.sin(x)/x
    def x(i,n,a,b):
        return a+(b-a)*i/n
    def simpson(n,a,b):
        i = 1.0
        f_sum = 0.0
        while i <= n:
            f_sum += (f(x(i-1,n,a,b)) + 4*f((x(i-1,n,a,b)+x(i,n,a,b))/2.0) + f(x(i,n,a,b)))
            i += 1
        result = f_sum / (6*n)
        result = "%.15f" % result
        return result
    print simpson(200,1,2)

结果为：

    0.659329906435525

- 例1-2 (2) 使用Simpson法计算\\( \frac{1}{\sqrt{2\pi}} \int _0 ^3 e ^{-\frac{x^2}{2}} {\rm d} x \\)。

数值解法：

    #!/usr/bin/env python
    import math

    def f(x):
        return math.exp(-(x**2) / 2) / math.sqrt(2*math.pi)
    def x(i,n,a,b):
        return a+(b-a)*i/n
    def simpson(n,a,b):
        f_sum = 0.0
        for i in range(n):
            f_sum += (f(x(i,n,a,b)) + 4.0*f((x(i,n,a,b)+x(i+1,n,a,b))/2.0) + f(x(i+1,n,a,b)))
        result = f_sum / (6.0*n)
        result = "%.15f" % result
        return result
    print simpson(200,0.0,3.0)

结果为：

    0.166216700655656

### 2.2 二重积分

设函数\\( z = f(x,y) \\)是有界闭域\\( D \\)上的有界函数，将\\( D \\)任意分成\\( n \\)个小闭区域

>\\( \Delta \sigma _1 , \Delta \sigma _2 , \cdots , \Delta \sigma _n \\)

其中\\( \Delta \sigma _i \\)表示第\\( i \\)个闭域的面积，\\( \Vert \Delta \sigma _i \Vert \\)表示\\( \Delta \sigma _i \\)的直径，取\\( \lambda = \max \limits _{ 1 \leqslant i \leqslant n} \Vert \Delta \sigma _i \Vert \\)。

在每个\\(  \Delta \sigma _i \\)上任取一点\\( (\xi _i , \eta _i) \\)，记

>\\( S = \sum _{i=1} ^n f( \xi_i , \eta_i ) \Delta \sigma _i \\)

当\\( \lambda \to 0 \\)时，若\\( S \\)的极限存在，就称这个极限为函数\\( f(x,y) \\)在区间\\( D \\)上的定积分：

>\\( \iint _D f (x,y) {\rm d} \sigma = \lim\limits _{ \lambda \to 0} \sum _{i=1} ^n f(\xi _i , \eta _i) \Delta \sigma _i \\)

二重积分表示以\\( z = f(x,y) \\)为顶，以\\( D \\)为底的曲顶圆柱体的体积。

取\\( \Delta \sigma _i \\) 为等分矩形域\\( \Delta x \Delta y \\)，\\( \xi _i , \eta _i \\)为区域\\( \Delta \sigma _i \\)的中心点\\( P _i \\)，则

>\\( \iint _D f (x,y) {\rm d} \sigma = \lim\limits _{ \lambda \to 0} \sum _{i=1} ^n f(P _i) \Delta x \Delta y \\)

这种方法称为单点柱体法。

- 例2-1 计算\\( \iint _D (xy+y^3) {\rm d} \sigma \\)，其中\\( D: 0 \leqslant x \leqslant 1 , 0 \leqslant y \leqslant 1 \\)。

理论解：

>\\( \iint _D (xy+y^3) {\rm d} \sigma = \int _0 ^1 {\rm d} x \int _0 ^1 (xy+y^3) {\rm d} y \\)

>\\( = \int _0 ^1 x \frac{y^2}{2} + \frac{y^4}{4} \vert _0 ^1 {\rm d} x \\)

>\\( = \int _0 ^1 (\frac{x}{2} + \frac{1}{4}) {\rm d} x \\)

>\\( = \frac{1}{2} \\)

数值解法：

    #!/usr/bin/env python
    RESULT = 0.5

    def f(x,y):
        return x*y+y**3.0
    def x(i,n,a,b):
        return a+(b-a)*i/n
    def y(i,n,a,b):
        return a+(b-a)*i/n
    def int_cylinder(m,n,ax,bx,ay,by):
        ax = float(ax)
        bx = float(bx)
        ay = float(ay)
        by = float(by)
        f_sum = 0.0
        for i in range(m):
            xx = (x(i,m,ax,bx)+x(i+1,m,ax,bx))/2
            for j in range(n):
                yy = (y(j,n,ay,by)+y(j+1,n,ay,by))/2
                f_sum += f(xx,yy)
        result = f_sum * ((bx-ax)/m) * ((by-ay)/n)
        err = abs(result - RESULT)
        err = "%.2e" % err
        result = "%.15f" % result
        return result,err

    print int_cylinder(100,100,0,1,0,1)

输出为：

    ('0.499987499999999', '1.25e-05')

- 例2-2 计算\\( \iint _D xy {\rm d} \sigma \\)，其中\\( D \\)为\\( y=x \\)和\\( y=x^2 \\)围成的区域。

理论解：

>\\( \iint _D (xy+y^3) {\rm d} \sigma = \int _0 ^1 {\rm d} x \int _{x^2} ^x xy {\rm d} y \\)

>\\( = \int _0 ^1 x \frac{y^2}{2} \vert _{x^2} ^x {\rm d} x \\)

>\\( = \int _0 ^1 (\frac{x^3}{2} - \frac{x^5}{2}) {\rm d} x \\)

>\\( = \frac{1}{24} \\)

数值解法：

    #!/usr/bin/env python
    RESULT = 1.0/24.0

    def f(x,y):
        return x*y
    def x(i,n,a,b):
        return a+(b-a)*i/n

    def int_cylinder(n,ax,bx):
        ax = float(ax)
        bx = float(bx)
        f_sum = 0.0
        for i in range(n):
            xx = (x(i,n,ax,bx)+x(i+1,n,ax,bx))/2
            y1 = x(i+1,n,ax,bx) ** 2
            y2 = x(i+1,n,ax,bx)
            for j in range(n):
                yy = (x(j,n,y1,y2)+x(j+1,n,y1,y2))/2
                f_sum = f_sum + f(xx,yy) * ((bx-ax)/n) * ((y2-y1)/n)
        result = f_sum
        err = abs(result - RESULT)
        err = "%.2e" % err
        result = "%.15f" % result
        return result,err

    print int_cylinder(500,0,1)

输出为：

    ('0.041599667000667', '6.70e-05')

- 例2-3 计算\\( \iint _D \sin \frac{3x+y}{5} e ^{ -x ^2 -xy } {\rm d} \sigma \\)，其中\\( D \\)为\\( y=2x \\)和\\( y=x^2 \\)围成的区域。

数值解法：

首先确定积分区域\\( D \\)的坐标区间为\\( 0 \leqslant x \leqslant 2, x ^2 \leqslant y \leqslant 2x \\)，因此

    #!/usr/bin/env python
    import math

    def f(x,y):
        return math.sin((3*x+y)/5) * math.exp(-x**2-x*y)
    def x(i,n,a,b):
        return a+(b-a)*i/n

    def int_cylinder(n,ax,bx):
        ax = float(ax)
        bx = float(bx)
        f_sum = 0.0
        for i in range(n):
            xx = (x(i,n,ax,bx)+x(i+1,n,ax,bx))/2
            y1 = x(i+1,n,ax,bx) ** 2
            y2 = x(i+1,n,ax,bx) * 2
            for j in range(n):
                yy = (x(j,n,y1,y2)+x(j+1,n,y1,y2))/2
                f_sum = f_sum + f(xx,yy) * ((bx-ax)/n) * ((y2-y1)/n)
        result = f_sum
        result = "%.15f" % result
        return result

    print int_cylinder(1000,0,2)

输出为：

    0.121888425678087

### 3. python中的数值积分模块

#### 3.1 一元函数积分

python有很多用于计算积分的[模块和库][ref01]，scipy是科学计算中最常用的一个模块，其积分函数的具体用法可使用`help(scipy.integrate)`或到[官方文档][ref02]查看。

[ref01]:https://pypi.org/ "python库"
[ref02]:https://docs.scipy.org/doc/scipy/reference/tutorial/integrate.html "scipy文档"

- 例3-1 使用梯形法计算\\( \int _0 ^{2\pi} e ^{ -0.5x } \sin (x + \frac{\pi}{4}) {\rm d} x \\)。

使用函数`trapz()`：

    #!/usr/bin/env python
    import math
    import numpy as np
    from scipy.integrate import trapz

    def f(x):
        return np.sin(x+math.pi/4) * np.exp(-0.5*x)

    x = np.linspace(0, 2*math.pi, 2000)
    result = trapz(f(x),x)
    result = "%.15f" % result
    print result

输出为：

    0.811859633350380

- 例3-2 使用Simpson法计算\\( \int _0 ^1 \frac{4}{1+x^2} {\rm d} x \\)。

使用函数`quad()`：

    #!/usr/bin/env python
    import numpy as np
    from scipy.integrate import quad

    def f(x):
        return 4.0/(1+x**2)

    x = np.linspace(0, 1, 200)
    result = quad(lambda x: f(x), 0, 1)
    print result

输出为：

    (3.1415926535897936, 3.4878684980086326e-14)

`quad()`有一些参数可以设置，通过`import scipy; help(scipy.integrate.quad)`查看。

#### 3.2 多重积分

使用scipy计算多重积分可使用`dblquad`, `tplquad`, `nquad`。

- 例3-3 对于不同的积分区域\\( D \\)，计算\\( \iint _D xy + e ^x {\rm d} \sigma \\)。

(1) \\( D: 0 \leqslant x \leqslant 1 , 0 \leqslant y \leqslant 2 \\)

(2) \\( D: 0 \leqslant y \leqslant 2 , y \leqslant x \leqslant 2y \\)

(1) 解法：

    #!/usr/bin/env python
    import numpy as np
    import math
    from scipy.integrate import dblquad

    def f(x,y):
        return x * y+ math.e ** x

    result = dblquad(lambda x, y: f(x,y), 0, 2, lambda x: 0, lambda x: 1)

    print result

输出：

    (4.43656365691809, 4.9255751221256473e-14)

(2) 解法：

    import numpy as np
    import math
    from scipy.integrate import dblquad

    def f(x,y):
        return x * y+ math.e ** x

    result = dblquad(lambda x, y: f(x,y), 0, 2, lambda x: x, lambda x: 2*x)

    print result

输出：

    (26.410018917641466, 2.932101108315151e-13)

- 例3-4 计算三重积分\\( \iiint _{\Omega} y \sin x + z \cos x {\rm d} \upsilon \\)，其中\\( \Omega : 0 \leqslant x \leqslant \pi , 0 \leqslant y \leqslant 1 , -1 \leqslant z \leqslant 1 \\)。

数值解法：

    #!/usr/bin/env python
    import numpy as np
    import math
    from scipy.integrate import tplquad
    def f(x,y,z):
        return y*np.sin(x) + z*np.cos(x)

    result = tplquad(lambda z, y, x: f(x,y,z), 0, math.pi, lambda x: 0, lambda x: 1, lambda x,y: -1, lambda x,y: 1)

    print result

输出：

    (1.9999999999999998, 2.2204460492503128e-14)
