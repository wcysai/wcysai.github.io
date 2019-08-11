---
layout: post
title: Elegant Brute Force
---

今天在ZJU训练[XIV Open Cup named after E.V. Pankratiev. GP of SPb](http://opentrains.snarknews.info/~ejudge/team.cgi?contest_id=010231)的时候遇到了这样一个有趣的题目，看起来是一个十分简单的bitmask dp，但是数据范围却令人感到头疼:

> Alice和Bob正在玩一个游戏。 给定$n$个字符串$s_1,s_2,\dots,s_n$. 双方轮流选择$26$个字母中的$1$个，且不能选择已经被选过的字母。当一个人选择完了一个字母后，如果只用所有选择过的字母可以组成某个字符串$s_i$,那么这个人输掉游戏。现在想问在双方决策都最优的情况下，是先手会赢还是后手会赢?

如果考虑生成函数的话，很明显答案就是$$[x^m]\prod\limits_{i=1}^{n}\frac{1}{1-x^{a_i}}$$,然而对于$m\leq 10^{18}$的庞大限制，显然无法求出$x^m$项之前的系数，而且这个做法也并没有用到$a_{i-1}\mid a_i$的性质。

很明显，这里的每个字符串都可以转化成一个bitmask，游戏的要求也变成了每次添加一位，使得不存在某个bitmask是当前bitmask的子集。

如果这里的字符集的大小只有$\vert \Sigma \vert=20$，这就是一个十分常规的简单题，首先利用高位前缀和或者[SOS DP](http://codeforces.com/blog/entry/45223),可以在$O(\vert \Sigma \vert2^{\vert \Sigma\vert})$的时间内计算出所有不能到达的bitmask, 然后再用一个$O(\vert \Sigma \vert2^{\vert \Sigma\vert})$的dp即可计算出最终答案。然而这里的限制条件是$\vert \Sigma \vert=26$,这样的做法显然无法在时限内通过。然而，虽然这题涉及到bitmask，可明显无法使用std::bitset进行优化，那么应该如何处理呢?

考虑我们是否还有什么性质没有利用到，然而结论是......似乎没有。我们现在的复杂度是$O(26\cdot 2^{26})$，如果能够将那个$26$的常数消掉，似乎复杂度就是正确的。然而我们应该怎样消掉这么大的一个常数呢......?我们需要再发掘一些可用的信息。我们注意到，我们之前无论是在计算高位前缀和还是进行dp的时候，我们的数组都是一个bool值。而如果我们使用unsigned int的话，便可以在同样的时间复杂度内存储32倍的信息!

于是答案便呼之欲出了: 我们建一个大小为$2^{21}$的unsigned int的数组$a$，数组$a[mask]$内存储的信息表示前21位是$mask$的bitmask中后$5$位一共$2^5=32$种情况下有哪些bitmask的值是为$1$的。计算高位前缀和和dp的时候都要分块内和块间两种情况进行讨论，这样的时间复杂度是$O((2^5+31)\cdot2^{21})$，可以在时限内通过。通过预处理和硬编码一些bitmask，最终的程序可以做到非常短。

写这篇博文的时候突然想起来这个technique似乎在哪里见过......好像是寒武纪camp的dreamoon contest?这个technique和bitset压位以及Method of Four Russians都有点相似，但又有不同，可以说是一种非常优雅而又具有技巧性的暴力了w。