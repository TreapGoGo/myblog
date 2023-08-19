---
title: 抽象代数 学习笔记
date: 2023-08-19 18:43:52
tags:
---

# 第零章 前言

本文根据B站UP主Maki的完美算数教室的抽象代数教学视频整理而来。

在此感谢Maki老师！也非常崇拜他能够在本科大二就讲出这么精品的课程。

~~我是不是也可以讲一讲微经/宏经呢？~~

# 第一章 基础知识

$$集合+运算+公理=结构$$

封闭性： $G\times G \rightarrow G$ ，例如在实数集合中，任何两个数相乘得到的结果依然在实数集中。

通过阅读下面的几个定义，可以帮助我们快速理解抽象代数到底要做什么，是用什么方式思考的。

1. 定义 $(S,\cdot)$ 为好结构，当且仅当

   1.  $\exist e \in S, \forall a \in S, a\cdot e=e\cdot a = a$ 

    称 $e$ 是该结构的单位元（identity element）。

    推论：如果 $(S,\cdot)$ 为好结构，那么单位元有且只有一个。

    证明：假如存在两个单位元 $e_1,e_2$ ，那么 $e_1=e_1\cdot e_2=e_2$ ，那么这两个单位元相等，说明单位元是唯一的。

2. 定义 $(G,\cdot)$ 为群，当且仅当

   1.  $\forall a,b,c\in G, a\cdot(b\cdot c)=(a\cdot b)\cdot c$ （结合律）
   2.  $\exist e\in G, \forall a\in G, a\cdot e=e\cdot a=a$ （单位元）
   3.  $\forall a\in G, \exist a^{-1}\in G, a\cdot a^{-1}=a^{-1}\cdot a=e$ （逆元）

    大的结构，一般也会满足小结构的性质，比如这里的群，单位元也是唯一的。

# 第二章 群

## 基本定义

设二元运算符号 $\cdot$ 满足 $\cdot:S\times S\rightarrow S$ 。通常简记 $\cdot(a,b）$ 为 $a\cdot b$ 。（封闭性）

定义 $(G,\cdot)$ 为群，当且仅当
 
   1.  $\forall a,b,c\in G, a\cdot(b\cdot c)=(a\cdot b)\cdot c$ （结合律）
   2.  $\exist e\in G, \forall a\in G, a\cdot e=e\cdot a=a$ （ $e$ 为单位元，identity element）
   3.  $\forall a\in G, \exist a^{-1}\in G, a\cdot a^{-1}=a^{-1}\cdot a=e$ （ $a^{-1}$ 为逆元，inverse element）
    若满足以上三条性质，则称 $(G,\cdot)$ 为群。
   4.  $\forall a,b\in G, a\cdot b=b\cdot a$ （交换律）
    若这四条性质都满足，则称 $(G,\cdot)$ 为阿贝尔群（Abel grouop）。

因为简单加法运算“ $+$ ”满足阿贝尔群的所有性质，为了方便，人们约定俗成地使用 $(G,+)$ 表示阿贝尔群，即使此处的加号不一定代表简单加法运算。当 $(G,+)$ 为阿贝尔群时，通常称 $e$ 为加法单位元，使用 $-a$ 表示 $a$ 的逆元。

### 例题

**例题 1**  $(\mathbb{Z}, +),(\mathbb{Q}, +),(\mathbb{R}, +),(\mathbb{C}, +)$ 是不是群？是不是阿贝尔群？

解：

|          | $(\mathbb{Z}, +)$  | $(\mathbb{Q}, +)$  | $(\mathbb{R}, +)$  | $(\mathbb{C}, +)$  |
| :------: | :----------------: | :----------------: | :----------------: | :----------------: |
|  结合律  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
|  单位元  |         0          |         0          |         0          |         0          |
|   逆元   |        $-a$        |   $-\frac{p}{q}$   |       $(-a)$       |  $-a-b\mathrm{i}$  |
|  交换律  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
|    群    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| 阿贝尔群 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
|   称呼   |      整数加群      |     有理数加群     |      实数加群      |      复数加群      |


**例题 2** 已知 $(n\mathbb{Z},+), n\in \mathbb{N}^*$ ，其中 $n\mathbb{Z}=\{nk,k\in \mathbb{Z} \}$ ，证明 $(n\mathbb{Z},+)$ 封闭；

解：设 $a,b \in n\mathbb{Z}, a=nk_1, b=nk_2$ ，则有 $a+b=n(k_1+k_2) \in n\mathbb{Z}$ ，故封闭。

**例题 3** 为什么 $(\mathbb{N},+)$ 不是群？

解：满足结合律，单位元存在，但逆元不存在。除 $0$ 的逆元为 $0$ 以外，其他自然数的逆元是负数，不在集合内，不满足逆元的要求。

**例题 4** 为什么 $(\mathbb{R},\cdot)$ 不是群？

解：非零数有逆元，但 $0$ 没有逆元。假设 $0$ 有逆元 $a$ ，则 $a\cdot0=1$ ，但是 $a\cdot0=0\ne1$ ，矛盾。

**例题 5** 证明 $(\mathbb{R}^*,\cdot),(\mathbb{Q}^*,\cdot)$ 是阿贝尔群。

解：先证封闭性。两个不为零的实数相乘，得到的数依然是实数，且不为 $0$ 。其满足结合律和交换律，单位元为 $1$ ，逆元为 $\frac{1}{a}$ ，因此是阿贝群。

**例题 6** 为什么 $(\mathbb{Z}^*,\cdot)$ 不是群？

解： $1$ 是单位元，但是除 $1$ 和 $-1$ 以外的每个数都没有逆元。比如 $2$ ，使得 $a\cdot 2=1$ 的 $a=\frac{1}{2}\notin \mathbb{Z}^*$ 。

**例题 7** 证明 $(\mathbb{R}^n,+)$ 是阿贝尔群。

解：先证封闭性，两个 $n$ 维向量相加，得到的结果依然是 $n$ 维向量。再证结合律和交换律，显然成立。其单位元为 $0_n$ 。逆元为 $-u$ 。

**例题 8** 证明 $(\mathbb{R}^{m\times n},+)$ 是阿贝尔群。

解：先证封闭性，两个 $m\times n$ 矩阵相加，得到的结果依然是 $m\times n$ 矩阵。交换律和结合律显然成立。其单位元为 $O_{m\times n}$ ，其逆元为 $-\mathrm{A}$ 。

**例题 9** 为什么 $(\mathbb{R}^{n\times n},\times)$ 不是群？

解：其单位元为单位矩阵 $I_n$ ，但 $O_{n}$ 没有逆矩阵。假设存在矩阵 $\mathrm A$ 使得 $\mathrm A\times O_n=I_n$ ，但 $\mathrm A\times O_n=O_n\ne I_n$ ，矛盾。

**例题 10** 定义一般线性群（General Linear Group）为 $GL_n(\mathbb R)=\{\mathrm A\in \mathbb R^{n\times n}:|\mathrm A|\ne0\}$ ，则 $(GL_n(\mathbb R),\cdot)$ 是群，但不是阿贝尔群。

解：

* 封闭性：对于 $|\mathrm A|\ne0,|\mathrm B|\ne0$ ，有 $|\mathrm{AB}|=|\mathrm A||\mathrm B|\ne0$ ，故封闭；
* 结合律：矩阵乘法满足结合律；
* 单位元： $I_n$ ；
* 逆元：定义就等价于可逆矩阵。
* 交换律：矩阵乘法不满足交换律。

因此一般线性群是群，但不是阿贝尔群。

**例题 11** 定义特殊线性群（Special Linear Group）为 $SL_n(\mathbb R)=\{\mathrm A\in \mathbb R^{n\times n}:|\mathrm A|=1\}$ ，则 $(SL_n(\mathbb R),\cdot)$ 是群，但不是阿贝尔群。

解：

* 封闭性：对于 $|\mathrm A|=1,|\mathrm B|=1$ ，有 $|\mathrm{AB}|=|\mathrm A||\mathrm B|=1$ ，故封闭；
* 结合律：矩阵乘法满足结合律；
* 单位元： $I_n$ ；
* 逆元：利用 $\mathrm{AA^{-1}}=I$ ，可得 $|\mathrm{AA^{-1}}|=|I|=1=|\mathrm A||\mathrm A^{-1}|=|\mathrm A^{-1}|$ ，因此 $\mathrm A^{-1}\in\mathbb R^{n\times n}$ 。
* 交换律：矩阵乘法不满足交换律。

因此特殊线性群是群，但不是阿贝尔群。

## 子群



# 第三章 环

# 第四章 因子分解

# 第五章 二次数域

# 第六章 域

# 第七章 伽罗瓦理论