---
layout: post
title: Some notes on Lagrange interpolation
---

有关Lagrange interpolation, 一般来说是对于一个$m$次多项式，给定\(m+1\)$个点值表示$(x_0,y_0),(x_1,y_1),\dots,(x_m,y_m)$,可以在$O(m^2)$时间内求出多项式的系数表示$f(x)$:
$$
f(x)=\sum\limits_{i=0}^{m}y_il_i(x)
$$
其中$l_i(x)$是Lagrange基函数:
$$
l_i(x)=\prod\limits_{0\leq j\leq m,i\neq j}\frac{x-x_j}{x_i-x_j}
$$


这个插值的正确性无需赘述，值得一提的是可以通过分治FFT以及多项式多点求值在$O(m(\log{m})^2)$的时间内快速求出。

然而，当点值表达式的形式是$x_{i}=i$的时候，多项式的表达式就变得更为简洁:
$$
f(x)=\sum\limits_{i=0}^{m}(-1)^{m-i}\binom{x-i-1}{m-i}\binom{n}{i}f(i)=\sum\limits_{i=0}^{m}\frac{(-1)^{m-i}}{i!(m-i)!}f(i)\prod\limits_{0\leq j\leq m,i\neq j}(n-j)
$$


在这种情况下，通过预处理$1--m$的的阶乘逆元以及$n-j$的前缀积，可以在$O(m)$的时间内通过$m+1$个点值插出$f(n)$的值。