---
title: UVA-12003 Array Transformer 解题笔记
date: 2023-07-27 00:52:08
tags: ["算法数据结构", "题解", "数据结构", "分块"]
---

# 题目来源

[UVA - 12003 - Array Transformer](https://vjudge.net/problem/UVA-12003/origin)

[Vjudge 页面](https://vjudge.net/problem/UVA-12003)

# 题意简述

长度为 $n\le 3\times10^5$ 的序列做 $m\le 5\times10^4$ 次操作，每次操作询问 $[L,R]$ 之间小于 $v$ 的数的个数 $k$ ，然后将 $a_p$ 修改为 $\displaystyle \left\lfloor \frac{u*k}{R-L+1} \right\rfloor$ 。

# 解题思路

显然这题用线段树是很容易解的，不过我把这个题当做分块的练习题来做，不使用任何树形数据结构。

假设块长为 $s$ 每次修改完后将 $p$ 所在块内的数据排序，复杂度 $O(s\log s)$ 。

之后每次查找可以用二分查找，单块查找的时间复杂度是 $O(\log s)$ ，可能涉及的块数是 $\displaystyle O(\frac{n}{s})$ ，结合的复杂度为 $\displaystyle O(\frac{n\log s}{s})$ 

总复杂度为 $\displaystyle O\left(m(\frac{n\log s}{s}+s\log s) \right)$ 。

深入分析发现， $s$ 增大会导致块数减少，查找的复杂度降低，在数学上体现为 $\displaystyle\frac{\log s}{s}$ 单调递减；但与此同时， $s$ 的增大会增大排序的复杂度，在数学上体现就是 $s\log s$ 单调递增。

这两个趋势是相互对抗的，因此就涉及到权衡，需要选择合适的块长 $s$ 。

为了求出 $\displaystyle f(s)=\frac{n\log s}{s}+s\log s= \left(\frac{n}{s}+s \right) \log s$ 的最小值，考虑对 $s$ 求导。

$$
\begin{aligned}
f'(s)
& = \left(-\frac{n}{s^2}+1 \right)\log s + \left(\frac{n}{s}+s \right)\frac{1}{s}\\
& = \frac{n(1-\log s)}{s^2} + \log s + 1
\end{aligned}
$$

令 $f'(s)=0$ ，可得约束条件

$$\frac{n(\log s-1)}{s^2}=\log s+1$$

$$\frac{n}{s^2}=\frac{\log s+1}{\log s-1}$$

当 $s\rightarrow\infty$ 或 $s$ 很大时， $\displaystyle\frac{\log s+1}{\log s-1}\rightarrow1$ ，此时有 $s=\sqrt{n}$ 。

事实上也没有必要搞得这么复杂。如果我们近似地认为 $\log s$ 并不影响复杂度，那么 $\displaystyle O\left(m(\frac{n\log s}{s}+s\log s) \right)$ 就可以直接简化为 $\displaystyle O\left(m(\frac{n}{s}+s)\right)$ ，就是标准的根号分块。

但是这种用数学方法寻找最优块长的思想是很重要的。

# AC代码

