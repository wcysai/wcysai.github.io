---
layout: post
title: Did you see the convex hull?
---

今天介绍的是13年比赛[Petrozavodsk Summer-2013. Gennady Korotkevich Contest 1](http://opentrains.snarknews.info/~ejudge/team.cgi?contest_id=001421)中通过人数最少的一个题目 *Icy Roads of Nomel*. 初看似乎是一个十分简单的题目，但庞大的数据范围却让人望而却步。在解决问题的过程中十分巧妙地用到了一些几何性质，最后的AC代码也是十分简短，但是思考过程不可谓不复杂。在我看来，这道题的巧妙程度相比于同样是利用几何性质的[Google Code Jam World Finals 2015 C Pretty Good Proportion](https://code.google.com/codejam/contest/5224486/dashboard#s=p2)可谓是有过之而无不及:

> 对于一个$n\times m$的网格图，我们有$n+1$条横线以及$m+1$条竖线，以及$(n+1)(m+1)$个交叉点。Acesrc想要从网格图左上角的交叉点$(0,0)$，每次只能向下走一格或者向右走一格，最终走到右下角的交叉点$(n,m)$。 每条横线和竖线都有一个权值，其中从上至下第$i$条横线的权值是$a_i$，从左至右第$i$条竖线的权值是$b_i$.当Acesrc位于交叉点$(i,j)$的时候，他向右走一格所需要的代价是$a_i$，向下走一格的代价是$b_j$。试问Acesrc走到右下角所需花费的最小代价之和是多少? 其中数据范围满足$n,m\leq 5\cdot 10^5,1\leq a_i,b_i\leq 10^9$

首先$O(nm)$的动态规划显然是不可取的。为了察觉到题目中蕴含的性质，我们先从一些基本情况进行考虑. 假设我们现在想从$(i_1,j_1)$到达$(i_2,j_2)$，其中$i_1<i_2,j_1<j_2$,且只考虑以下两种走法: 先从$(i_1,j_1)$到$(i_1,j_2)$,再从$(i_1,j_2)$到$(i_2,j_2)$; 或是先从$(i_1,j_1)$到$(i_2,j_1)$,再从$(i_2,j_1)$到$(i_2,j_2)$。

对于第一种走法，我们所需的代价是$x=b_{j_1}(i_2-i_1)+a_{i_2}(j_2-j_1)$;对于第二种走法，所需的代价则是$y=a_{i_1}(j_2-j_1)+b_{j_2}(i_2-i_1)$. 为了比较大小，我们做差得到$\Delta=y-x=(b_{j_2}-b_{j-1})(i_2-i_1)-(a_{i_2}-a_{i_1})(j_2-j_1)=\Vert X\times Y\Vert$,

其中

$X=(i_2-i_1，a_{i_2}-a_{i_1}),Y=(j_2-j_1，b_{j_2}-b_{j_1})$

这里向量的叉积就神奇地出现了!顺着这样的思路，我们建立两个二维平面上的点集$S_{1}=\{(i,a_{i})\vert 0\leq i\leq n\}$和$S_{2}=\{(i,b_{i})\vert 0\leq i\leq m\}$.然后一个贪心方法是根据每次对于右下位置叉积是否大于零决定向右走一格还是向左走一格。 这样显然是不正确的，反例也很好构造。 然而，对于这个贪心方法进行一些改造，我们便可以得到正确解法，首先是下面的一个结论:

> 对于点集$S_1$和$S_2$，在最优解下，只有在下凸包上的点会被经过

这个结论的证明并不复杂，只要利用一些叉积的几何性质即可，在这里略去（才不是因为懒得画图）。这样，我们可以得到一个改进了的算法: 根据下一步的叉积的符号，选择在第一个点集的凸包上向右移动一步，或者在第二个点集的凸包上向右移动一步。对于这个改进了的贪心，竟然可以被证明是正确的!证明方法也不难，就留作读者思考吧(笑)。 至此，我们以$O(n+m)$的复杂度解决了这个问题。

AC代码链接: [Petrozavodsk Summer-2013. Gennady Korotkevich Contest 1 I. Icy Roads of Nomel]([https://github.com/wcysai/CodeLibrary/blob/master/Contests/Petrozavodsk%20Summer-2013.%20Gennady%20Korotkevich%20Contest%201/I.cpp](https://github.com/wcysai/CodeLibrary/blob/master/Contests/Petrozavodsk Summer-2013. Gennady Korotkevich Contest 1/I.cpp))