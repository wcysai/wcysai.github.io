---
layout: post
title: On nim multiplication and some related games
---

这篇blog要介绍的是有关nim multiplication(nim积)的概念以及一些应用。

在这篇blog中，我会用$\oplus$来表示nim和运算，用$\otimes$来表示nim积运算。

# Turning Games

Turning Games,指的是一类这样的游戏: 有$n$枚硬币排成一排，其中每枚硬币可能正面向上也可能反面向上。两个人轮流操作，每次操作包括将一些(限定的)集合中的硬币翻面，但必须保证翻面的硬币的集合中最右边的一枚一开始一定是正面朝上的。当一个人无法操作时，他将输掉游戏。可以发现，在这样的定义下，Turning Games永远是impartial且finite的，所以可以使用Sprague-Grundy定理计算胜负态。对于这一类Turning Games,有一个十分好用的定理:

> 定理: Turning Games中某一个局面的$SG$值，等于局面中每个正面朝上的硬币单一存在时的局面的$SG$值的nim和（异或和）

这个定理并不难证明，我们可以考虑把翻面的操作当作加一个相同的copy，因为在nim和的定义下两个相同数的nim和是$0$，所以可以发现这两种方式是等价的，也就是说每个位置的$SG$值是独立的。

下面我们介绍一些经典的Turning Games的例子:

## Turning Turtles

在这个例子中，每个人可以翻转一枚或者两枚硬币，但最右边一枚硬币一开始一定是正面朝上的。

对于这个游戏，每个位置的SG值应该是怎么样的呢? 实际上，对于从左往右第$x$枚硬币($x$从$0$开始)，它的$SG$值为$G(x)=x+1$,也就是说，我们可以把每个朝上的硬币看做一个Nim堆，堆的大小就是它的编号加一。

> 定理: 对于Turning Turtles游戏，$G(x)=x+1$

这个的证明可以通过考虑这个游戏和Nim​游戏的等价，并不难考虑。

## Mocking Turtles

在这个例子中，每个人可以翻转一枚,两枚或者三枚硬币，但最右边一枚硬币一开始一定是正面朝上的。

这个问题不像前一个问题有那么显然的特征，于是我们可以先利用Sprague-Grundy定理打出前几项的表来观察:

| $n$    | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   | 13   | 14   | 15   | 16   |
| ------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $G(n)$ | 1    | 2    | 4    | 7    | 8    | 11   | 13   | 14   | 16   | 19   | 21   | 22   | 25   | 26   | 28   | 31   | 32   |

通过打表我们可以看出$G(n)$总是$2n$或者$2n+1$,进一步观察发现$G(n)=\begin{cases} 2n & \text{$n$的二进制中有奇数个$1$}\\ 2n+1 & \text{$n$的二进制中有偶数个$1$ }\end{cases}$

证明的话可以考虑通过Sprague-Grundy定理，$G(n)$是不是以下任何一种形式的最小的非负整数:

​														$$0,G(a),G(a)\oplus G(b)$$

然后利用数学归纳法进行证明。

## Moebius, Mogul, Gold Moidores, and The Mock Turtle Theorem

对于上述两个游戏的综合，也就是每个人可以翻转$[1,t]$枚硬币，但最右边一枚硬币一开始一定是正面朝上的。

我们之前讨论了$t=2$和$t=3$的情况，实际上，只有$t=2m+1$的情况是有趣的(为什么?之后会说)。对于$t=3$的情况称作Mocking Turtles,$t=5$的情况称作Moebius,$t=7$的情况称作Mogul,$t=9$的情况称作Gold Moidores.对于$t>3$情况下的$G(n)$的计算没有直接的公式，但是下面的The Mock Turtle Theorem给出了$t=2m$和$t=2m+1$下$G(n)$的关系:

> The Mock Turtle Theorem: 对于$t=2m+1$的情况，所有$G(n)$的二进制位中都包含奇数个$1$,且$t=2m$时的$G(n)$是$t=2n+1$时的$G(n)$值去掉最后一个二进制位的值（即除以二下取整）

对于这个定理的证明以及关于这些游戏的更多信息，可以参照由Elwyn R.Berlekamp,John H.Conway和Richard K.Guy写的*Winning Ways For Your Mathematical Plays,Volume 3*

## Motley

这个例子中，每个人可以翻转任意枚硬币，但最右边一枚硬币一开始一定是正面朝上的。

很显然，这个游戏除非一开始就没有正面朝上的硬币，否则先手必胜。这个游戏的$SG$函数如下所示:

> 定理: 对于Motley游戏，$G(x)=2^x$

## Twins,Trplets, Etc.

在Twins游戏中，每个人必须翻转**恰好**$2$枚硬币，但最右边一枚硬币一开始一定是正面朝上的。

在Triplet游戏中，每个人必须翻转**恰好**$3$枚硬币，但最右边一枚硬币一开始一定是正面朝上的。

很显然，Twins游戏的$SG$值就是Turning Turtles游戏前面加$1$个$0$，Triplets游戏的$SG$值就是Mocking Turtles游戏前面加$2$个$0$，依次类推。

## Ruler

在这个例子中，每个人可以翻转任意枚**连续**的硬币，但最右边一枚硬币一开始一定是正面朝上的。

对于Ruler游戏，容易证明以下定理成立:

> 定理: 对于Ruler游戏，$G(x)=2^i$，其中$i$是满足$2^i \leq x+1$的最大的$i$

## An example

这是一道来自[Petrozavodsk Summer-2014. Warsaw U Contest](http://opentrains.snarknews.info/~ejudge/team.cgi?contest_id=001447)的题，题面如下:

> Jack和Chip在玩翻硬币游戏，规则是每个人可以翻转任意枚的硬币，但最右边一枚硬币一开始一定是正面朝上的，且翻转硬币的下标集合排序后构成等差数列。给定初始$n\leq 3000$枚硬币的正反情况，问先后手谁会获胜，若先手获胜，给出一个获胜的第一步操作。

这个题目，对于知道了翻硬币游戏中每个硬币的$SG$值独立就很简单了，可以通过$dp$在$O(n(\frac{n}{1}+\frac{n}{2}+\frac{n}{3}+\dots+\frac{n}{n})=n^2\log{n})$的时间里计算出所有位置的$SG$值，第一步操作同样枚举即可。

# The 2-Dimensional Case

下面我会介绍一些二维的Turning Game, 其定义是在一个二维平面上翻硬币，其中每枚硬币可能正面向上也可能反面向上。两个人轮流操作，每次操作包括将一些(限定的)集合中的硬币翻面，但必须保证翻面的硬币的集合中最右下的一枚一开始一定是正面朝上的。当一个人无法操作时，他将输掉游戏。

很明显，二维Turning Game也具有之前介绍的$SG$值独立性，在这里我们把第$a$行第$b$列的硬币正面朝上的$SG$值记为$SG(a,b)$.

## Acrostic Twins

这里是一个比较简单的例子。在这个例子中，每个人可以翻转同一行的两枚或者同一列的硬币，但最右下一枚硬币一开始一定是正面朝上的。我们直接给出计算$SG$值的方法,证明略去:

> 定理: 对于Acrostic Twins游戏，$G(a,b)=a\oplus b$

## Turning Corners

这是一个更加有趣的例子，也正是在这个例子里我们得以引出nim multiplication的概念。在这个例子中，每个人可以翻转某一个矩形的四个角上的硬币，但最右下一枚硬币一开始一定是正面朝上的。

根据Sprague-Grundy定理，$G(a,b)=mex\{G(a',b),G(a,b'),G(a',b')\}$,其中$a'$和$b'$是满足$0\leq a'<a,0\leq b'<b$的任意数字。在这里，我们先给出$a,b<16$的$SG$值的表（下标从$0$开始):

| 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   | 13   | 14   | 15   |
| 0    | 2    | 3    | 1    | 8    | 10   | 11   | 9    | 12   | 14   | 15   | 13   | 4    | 6    | 7    | 5    |
| 0    | 3    | 1    | 2    | 12   | 15   | 13   | 14   | 4    | 7    | 5    | 6    | 8    | 11   | 9    | 10   |
| 0    | 4    | 8    | 12   | 6    | 2    | 14   | 10   | 11   | 15   | 3    | 7    | 13   | 9    | 5    | 1    |
| 0    | 5    | 10   | 15   | 2    | 7    | 8    | 13   | 3    | 6    | 9    | 12   | 1    | 4    | 11   | 14   |
| 0    | 6    | 11   | 13   | 14   | 8    | 5    | 3    | 7    | 1    | 12   | 10   | 9    | 15   | 2    | 4    |
| 0    | 7    | 9    | 14   | 10   | 13   | 3    | 4    | 15   | 8    | 6    | 1    | 5    | 2    | 12   | 11   |
| 0    | 8    | 12   | 4    | 11   | 3    | 7    | 15   | 13   | 5    | 1    | 9    | 6    | 14   | 10   | 2    |
| 0    | 9    | 14   | 7    | 15   | 6    | 1    | 8    | 5    | 12   | 11   | 2    | 10   | 3    | 4    | 13   |
| 0    | 10   | 15   | 5    | 3    | 9    | 12   | 6    | 1    | 11   | 14   | 4    | 2    | 8    | 13   | 7    |
| 0    | 11   | 13   | 6    | 7    | 12   | 10   | 1    | 9    | 2    | 4    | 15   | 14   | 5    | 3    | 8    |
| 0    | 12   | 4    | 8    | 13   | 1    | 9    | 5    | 6    | 10   | 2    | 14   | 11   | 7    | 15   | 3    |
| 0    | 13   | 6    | 1    | 9    | 4    | 15   | 2    | 14   | 3    | 8    | 5    | 7    | 10   | 1    | 12   |
| 0    | 14   | 7    | 9    | 5    | 11   | 2    | 12   | 10   | 4    | 13   | 3    | 15   | 1    | 8    | 6    |
| 0    | 15   | 5    | 10   | 1    | 14   | 4    | 11   | 2    | 13   | 7    | 8    | 3    | 12   | 6    | 9    |

## Nim Multiplication

可以观察到$G(0,n)=0$,$G(1,n)=1$，于是我们定义nimber意义下的乘法nim multiplication: $a\otimes b=G(a,b)$,我们把$a\otimes b$称作$a$和$b$的nim-product.

令人惊奇的是，nim-multiplication和nim-addition有着类似普通乘法和加法的结合律:

> Nim Distributive law: $a\otimes (b\oplus c)=a \otimes b \oplus a \otimes c$

nim-multiplication本身还具有交换律和结合律:

> Nim Multiplication Commutativity law: $a\otimes b=b \otimes a$
>
> Nim Multiplication Associativity law: $a\otimes (b\otimes c)=(a\otimes b)\otimes c$

那么，应当如何计算$a\otimes b$呢?相较于nim addition,nim multiplication的计算要复杂很多，且nim multiplication的计算很大程度上要依靠对于Fermat powers of $2$的计算。对于nim multiplication有以下定理:

> 如果$N$是一个Fermat powers of $2$，即$N=2^{2^n}$,其中$n$是非负整数，那么有:
>
> $N \otimes a = N\times a$, 其中$a<N$
>
> $N\otimes N=\frac{3}{2}N$
>
> 如果$a,b<2^{2^n}$,那么$a\otimes b< 2^{2^n}$

因此，对于$a\otimes b$的计算，我们可以将$a$和$b$拆成一些$2$的次幂的nim和，利用分配律转为计算$a'\otimes b'$,其中$a'$和$b'$都是$2$的次幂，然后再把$a'$和$b'$拆成一些Fermat powers of $2$的nim乘积利用结合律计算即可。正常的实现情况下，如果$a,b<A$，那么计算$a\otimes b$的复杂度是$O((\log A)^2)$的。

附上我的nim multiplcation模板: [NimMultiplication.cpp](https://github.com/wcysai/CodeTemplates/blob/master/Others/NimMultiplication.cpp),实际实现时还可以打出一些小范围的表减少程序运算的常数。

## The Tartan Theorem

那么，这个nim multiplcation的定义有什么用呢? 事实上，如果我们把两个一维的turning game $A$和$B$结合到一起，表示所选的行应该遵从turning game $A$中的规定，所选的列应该遵从turning game $B$中的规定，那么我们把这个游戏叫做一个tartan game,用$A\times B$表示。

对于tartan game,有着如下的定理:

> The Tartan Theorem: 对于一个tartan game $A\times B$，每个位置的$SG$值可以这样计算:
>
> $G_{A\times B}(a,b)=G_{A}(a)\otimes G_{B}(B)$

因此，对于之前的Turning Corners游戏，我们可以知道Turning Corners=Twins$\times$Twins

考虑如下的Rugs游戏: 每次我们可以翻转一个矩形的硬币，但最右下一枚硬币一开始一定是正面朝上的。我们可以发现Rugs=Ruler$\times$Ruler,因此通过Tartan Theorem来计算Rugs游戏的$SG$值。

实际上，Tartan定理对更高维的情况也适用，这时每个状态的$SG$值是一维情况下各个状态的$SG$值的nim multiplcation。

## A Field is Born!

对于nim addition和nim multiplication，有一个十分简洁但强大的结论。

> 定理: 对于某个非负整数$n$, 以及$S=\{x\vert x\in N, x<2^{2^n} \}$,$(S,\oplus,\otimes)$构成了一个特征为2的域。

在几天前的[XX Open Cup, Grand Prix of Warsaw](https://official.contest.yandex.ru/opencupXX/contest/13614)中就有一个需要这个定理的题目[L.Lati@s](https://official.contest.yandex.ru/opencupXX/contest/13614/problems/L/),在了解一些博弈论基础以及对nim multiplication和The Tartan Theorem的情况下，题目可以转化为:

> 求一个$n\times n$的矩阵$A$在nim addition和nim multiplcation所定义的域下的permanent. 其中$1\leq n\leq 150$, $0\leq A_{i,j}<2^{64}$.

计算一般域下矩阵的permanent显然是NP-Hard的，但是这个域的characteristic是2，因此矩阵的permanent就等于它的行列式(例:求一个二部图的完美匹配数模二意义下的余数)，高斯消元即可。这里还涉及到一些复杂度的优化，有兴趣的同学可以尝试一下。