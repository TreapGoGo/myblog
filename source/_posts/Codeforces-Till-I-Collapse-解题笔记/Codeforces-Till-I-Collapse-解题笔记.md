---
title: Codeforces-Till I Collapse 解题笔记
date: 2023-07-30 23:12:25
tags:
---

# 题目来源 

[Codeforces Round 406 (Div. 1) C. Till I Collapse](https://codeforces.com/contest/786/problem/C)

[Vjudge页面](https://vjudge.net/problem/CodeForces-786C)

# 题意简述

长度为 $n$ 的正整数序列 $a_i$ ，数字表示颜色。将序列划分成尽可能少的连续段，每段中颜色的数量不能超过 $k$ 。对于所有的 $k\in[1,n]$ 输出答案。

 $n\le1\times10^5,a_i\le n$ 

# 解题思路

暴力的贪心做法是，对于一个确定的 $k$ ，从左往右进行划分，尽可能把每一段划分得更长。因为所有的段都是连续的，一个接一个排列，显然，这样的贪心策略不会使得答案更劣。暴力的复杂度为 $O(nk)$ 。

显然，确定一个区间端点后，区间长度越大，区间内颜色数量也就越多（有可能相等，但不会变少）。这种单调性使得二分具有可行性。

算法如下：

1. 以第一个元素作为区间左端点 $l$ ；
2. 在 $l$ 的右侧进行二分查找，找到一个最大 $r$ 满足 $[l,r]$ 之间的颜色数量小于等于 $k$ ；
3.  $l\leftarrow r+1$ ，然后回到第 2 步；

复杂度为 $O(nk\sqrt{n}\log n)$ ，依然不是很优。

为了优化，可以预处理出以每个点作为左端点