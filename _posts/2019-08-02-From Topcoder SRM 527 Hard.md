---
layout: post
title: Starting From Topcoder SRM 527 Div1 Hard...
---

这个问题最初是在Petr Mitrichev 的博客上看到的，来自于由rng_58\(Makoto Soejima\)出的Topcoder SRM 527 Div.1 Hard, 题面描述可以在Topcoder网站上找到[P8XCoinChange](https://community.topcoder.com/stat?c=problem_statement&pm=11155&rd=14552)。 题意大概是这样的:

> 现在有$n$种面值的货币$a_1,a_2,\dots,a_n$，满足$1=a_1<a_2<\dots<a_n$且对于$2\leq i\leq n$有$a_{i-1}\mid a_i$,现在要求用这$n$种面值的货币购买价值为$m$的物品的方案数，假设每种货币的数量无限，答案对$10^6+3$取模。其中$1\leq n\leq 40,1\leq m,a_i\leq 10^{18}$

如果考虑生成函数的话，很明显答案就是$$[x^m]\prod\limits_{i=1}^{n}\frac{1}{1-x^{a_i}}$$,然而对于$m\leq 10^{18}$的庞大限制，显然无法求出$x^m$项之前的系数，而且这个做法也并没有用到$a_{i-1}\mid a_i$的性质。

这题的标答是一个$O(n^3\log{m})$的运用了巧妙的矩阵乘法的做法，可以参照由rng_58提供的SRM官方题解[tutorial](https://apps.topcoder.com/wiki/display/tc/SRM+527)，里面有很生动形象的解释以及有趣的图片（笑），这里简单说一下这个做法的思路:

> 把题目转化为，有一只兔子(题解里的Usagi)从位置$0$开始，想要跳到位置$m$，每次必须选择数组$a$中的一个长度进行跳跃,且每次跳跃的距离单调不增，求合法跳跃序列的个数。
>
> 假设兔子每次选择可以跳跃的最长距离向右跳，并将兔子经过的这些位置打上标记，可以发现，对于所有合法的跳跃序列，一定会经过所有被打上标记的点。(为什么?)
>
> 建立矩阵$A$，其中$A[i][j]$表示跳跃的长度不超过$a_i$,且最后一步跳跃的长度是$a_j$的方案数,我们只要对每个在数组$a$内的长度计算出这个矩阵，然后用矩阵快速幂和矩阵乘法合并就可以得到最终答案了。（为什么?)
>
> 对于$a_i$的矩阵，可以通过$a_{i-1}$的矩阵进行矩阵快速幂得到，这部分的时间复杂度是$O(n^{3}\log{a_2}+n^{3}\log{\frac{a_3}{a_2}}+\dots+n^3\log{\frac{a_n}{a_{n-1}}})=O(n^3\log m)$,总体时间复杂度是$O(n^3\log m+n^4)$，足以解决本题。

然而，这题还有一个加强版[BZOJ4588](https://www.lydsy.com/JudgeOnline/problem.php?id=4588),在这里我们需要处理$T\leq 30$组测试数据，然而时限还是只有$1s$,用之前的做法无法在时限内通过，需要复杂度更为优秀的做法。

那么，对于这种$n$很小但$m$很大的问题，结合之前做过的一些题目，很容易会产生这样的猜想:用前$n$种货币购买价值为$m$的物品的方案数是$f_{n}(m)$否是一个关于$m$的$n+1$次多项式?

很遗憾，当$n=2,a_2=2$的时候便很容易验证上述猜想不成立。

然而，猜想错误的原因在何处呢? 因为对于$a_2=2$的情况，$f_{n}(m)=f_{n-1}(m)+f_{n-1}(m-2)+f_{n-1}(m-4)+\dots$显然和$f_{n}(m-1)$是互不相关的，故这是一个和奇偶性有关的函数，因此无法成为一个多项式。然而，当我们考虑所有$x=m-ka_n,k\geq 0$的位置的值的时候，可以发现，$f_{n}(x)$的确是一个$n+1$次的多项式!这一点可以通过数学归纳法进行证明。

接下来的事情就很简单了，利用上一篇博客[Some notes on Lagrange interpolation](https://wcysai.github.io/2019/08/02/some-notes-on-lagrange-interpolation/)的方法对多项式进行插值，便可以在$O(n^3)$的时间内求出一组数据的答案。

然而，类似的问题，还出现在了[XVII Open Cup named after E.V. Pankratiev, Grand Prix of China](http://opencup.ru/files/och/gp11/problems1-e.pdf)上\(C题\)，你能根据上面的介绍，想出应当如何解决吗？