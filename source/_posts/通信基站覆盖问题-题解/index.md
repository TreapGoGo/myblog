---
title: 通信基站覆盖问题 题解
date: 2023-06-28 22:14:24
categories: "题解"
tags: ["算法数据结构", "题解", "贪心", "堆"]
---

本题来自于某次付费答疑，经本人后续查证，题目出自印度德里理工学院的一次期中考试。

# 题面

Let us consider a long, quiet country road with houses scattered very sparsely along it. (We can picture the road as a long line segment, with an eastern endpoint and a western endpoint.) Further, let’s suppose the residents of all these houses are avid cell phone users. You want to place cell phone base stations at certain points along the road, so that every house is within 4 kilometers of one of the base stations. Give an efficient algorithm that achieves this goal, using as few base stations as possible. Prove the correctness of your algorithm.

# 题意简述

在一条公路两侧稀疏分布着若干村庄。一个通信基站的覆盖半径是4千米。要求放置尽可能少的通信基站，使得每个村庄都能收到通信基站的信号。

# 我的解答

Above all, the distance between the adjacent houses may be very far, which makes the road too long to deal with. However, it doesn't matter how far it is as long as the distance is greater than $8$ miles, because the stations between the two house can't reach both of them anyhow.

Thus, if it is greater than $8$ , we can assume that it is exactly $9$ .

For every house seated at $a_i$ , there are $9$ points where a station can reach it. These points are at $\{a_{i}-4,a_{i}-3,...,a_{i}+4\}$ . We can assume that the house contribute $1$ coverage to these $9$ points

Let $c[i]$ be the contributions that the $i$-th point received from nearby houses. By using a difference array, we can calculate $c$ with the time complexity of $O(n)$. 

To cover as many houses as possible with one station, we should place the station at the point whose $c[i]$ is as great as possible. So we do that in the decreasing order of $c$ until all the houses are covered.

(Proof: if we place a station at where $c[i]$ is smaller, it can't cover more houses than we did just now. So the needed stations can't be fewer. The answer must be inferior to that we had just now. )

To avoid covering a house unnecessarily repeatedly, we should mark the houses which have been covered. So every time we place a station at $i$ , we should mark the range $[i-4,i+4]$ as a no-station area.

(Proof: if we place a station outside the area, it can cover more houses than inside the area, and the house covered in the range won't be lost. )

To do this efficiently, we should push the $<c[i],i>$ pairs into a heap in the decreasing order of $c[i]$ . Every time we grab the top of the heap, pop it out, check if it's in a no-station area, place the station and mark the range if not.

To mark and check the range more efficiently, we can build a segment tree with tags, or just a binary indexed tree can tackle everthing.

The total time complexity is no more than $O(n\log n)$ .

# 印度德里理工学院官方题解

Solution: There is a simple greedy algorithm here. Let $h$ denote the left-most house. Then we place a base station 4km to the right of $h$. Now remove all houses
which are covered by this base station, and repeat. It is easy to show that if the algorithm locates base stations at $b_1, . . . , b_k$, and some other algorithm (which could be the optimal algorithm) places base stations at $b'_1,...,b'_k$(from left to right), then $b_1 \ge b'_1$, $b_2 \ge b'_2$, and so on. Therefore $k ≤ k'$.

公式测试$x$公式测试

公式测试 $x$ 公式测试