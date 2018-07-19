---
layout: post
title: 数值微分方法
---

### 1. 问题背景

含有未知函数导数或微分的方程称为微分方程，所含有未知函数导数的最高阶数称为微分方程的阶。若微分方程中的求导变量只有一个，称为常微分方程，否则称为偏微分方程。\\( n \\)阶常微分方程的一般形式为

>\\( F(x, y', y'', \cdots, y^{(n)}) \\)

若存在



### 2. 常微分方程的数值解法

### 2.1 简介

### 2.2 数值算法原理




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



### 3. python中微分方程解法

### 3.1 解析法求解


### 3.2 数值解法
