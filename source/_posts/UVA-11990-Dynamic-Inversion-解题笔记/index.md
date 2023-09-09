---
title: UVA-11990 Dynamic Inversion 解题笔记
date: 2023-07-27 03:12:20
categories: 题解
tags: ["算法数据结构", "题解", "数据结构", "分块", "排序", "树状数组"]
---

# 题目来源

[UVA - 11990 Dynamic Inversion](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3141)

[Vjudge 页面](https://vjudge.net/problem/UVA-11990)

# 题意简述

将一个 $n\le 2\times10^5$ 的排列逐个删去 $m\le1\times10^5$ 个元素，问每次删除元素之前，排列中有多少个逆序对。

# 解题思路

本题和 {% post_link 'UVA-12003-Array-Transformer-解题笔记'%} 几乎一模一样，都是分块并在每次修改后保持块内有序，由此可使用二分查找实现高效的查询。

在求逆序对数量初值的时候使用树状数组即可，此后每一次删掉一个数字就查询其左侧比它大的数的数量，在总逆序对数量中减掉即可。

保持块内有序的方式依然可以使用冒泡排序，减少一个 $\log$ 。

与 UVA-12003 实在太像了，代码略。