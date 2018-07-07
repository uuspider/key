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

>\\( S = \overset{n}\underset{i=1}{\sum} f( \xi_i ) \Delta x_i \\)

当\\( \lambda \to 0 \\)时，若\\( S \\)的极限存在，就称这个极限为函数\\( f(x) \\)在区间\\([a, b]\\)上的定积分：

>\\( \int _a ^b f (x) {\rm d} x = \lim\limits_{ \lambda \to 0} \overset{n}\underset{i=1}{\sum} f(\xi _i) \Delta x _i \\)

将\\([a, b]\\)进行等分，有

>\\( \Delta x _i = \frac{1}{n} (b-a) \\)

>\\( x _i = a + \frac{i}{n} (b-a), i=1,2, \cdots, n \\)

则

>\\( \int _a ^b f (x) {\rm d} x = \lim\limits_{n \to \infty} \\)
