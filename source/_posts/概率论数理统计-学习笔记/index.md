---
title: 概率论数理统计 学习笔记
date: 2023-07-21 20:26:37
categories: 数学
tags: ["学习笔记", "数学", "概率论"]
---

# 前言

感谢B站up主高数叔！每次上课之前打个铃，很有仪式感。

# 第一章 随机事件与概率

## 随机事件关系及其运算

|      名称       |              符号              |                                理解                                |                     集合定义                     |
| :-------------: | :----------------------------: | :----------------------------------------------------------------: | :----------------------------------------------: |
|  $A$ 包含 $B$   |          $A\supset B$          |                   事件 $B$ 发生必有事件 $A$ 发生                   |                $B$ 是 $A$ 的子集                 |
| $A$ 与 $B$ 相等 |             $A=B$              | 事件 $A$ 发生必有事件 $A$ 发生<br>且事件 $B$ 发生必有事件 $A$ 发生 |           $A$ 与 $B$ 包含的样本点相同            |
| $A$ 与 $B$ 的和 |           $A\cup B$            | 事件 $A\cup B$ 发生 $\Leftrightarrow$ 事件 $A$ 发生或事件 $B$ 发生 |                $A$ 与 $B$ 的并集                 |
| $A$ 与 $B$ 的积 |        $A\cap B \\ AB$         | 事件 $A\cap B$ 发生 $\Leftrightarrow$ 事件 $A$ 发生且事件 $B$ 发生 |                $A$ 与 $B$ 的交集                 |
| $A$ 与 $B$ 的差 | $A-B \\ A-AB \\ A\overline{B}$ | 事件 $A-AB$ 发生 $\Leftrightarrow$ 事件 $A$ 发生且事件 $B$ 不发生  |                $A$ 与 $B$ 的差集                 |
| $A$ 与 $B$ 互斥 |        $AB=\varnothing$        |                  事件 $A$ 与事件 $B$ 不会同时发生                  |           $A$ 与 $B$ 没有共同的样本点            |
| $A$ 的对立事件  |         $\overline{A}$         |        每次试验事件 $A$ 与  $\overline{A}$ 有且仅有一个发生        | $A\cup \overline{A}=S,A\overline{A}=\varnothing$ |

事件运算满足交换律、结合律、分配律和德摩根律。

### 德摩根律

$$\overline{A\cup B}=\overline{A}\cap\overline{B}, \overline{A\cap B}=\overline{A}\cup\overline{B}$$ 

$$\overline{A\cup B\cup C}=\overline{A}\cap\overline{B}\cap\overline{C}, \overline{A\cap B\cap C}=\overline{A}\cup\overline{B}\cup\overline{C}$$ 

简记为：**长杠变短杠，开口变方向**。


## 概率定义与性质

有限可加性：设 $A_1,A_2,...,A_n$ 是两两互不相容的事件，则有

$$P\left(\bigcup_{i=1}^n A_i \right)=\sum_{i=1}^n P(A_i)$$

减法公式： $P(A-B)=P(A\overline{B})=P(A)-P(AB)$ 。特别地，若 $A\subset B$， 则有 $P(A)\le P(B)$ ，且 $P(B-A)=P(B)-P(A)$  

加法公式：

$$P(A\cup B)=P(A)+P(B)-P(AB)$$

$$P(A\cup B \cup C)=P(A)+P(B)+P(C)-P(AB)-P(BC)-P(AC)+P(ABC)$$

$$P\left(\bigcup_{i=1}^n A_i \right)=\sum_{k=1}^n (-1)^{k-1} \sum_{1\le i_1<i_2<\cdots<i_k\le n} P\left( A_{i_1}A_{i_2}\cdots A_{i_k}\right) \ \text{(即 OI 中的容斥原理)}$$

## 古典概型与几何概型

古典概型：有限样本点，等可能性

$$P(A)=\frac{A\ 中基本事件个数\ x}{基本事件总数\ n}$$ 

几何概型：从均匀的有界几何图形（线段、平面、空间等）中随机抽取一点落在特定大小的子图形中的概率， 

$$P(A)=\frac{A\ 的长度（或面积、体积）}{\Omega\ 的长度（或面积、体积）}$$ 

## 条件概率与乘法公式

条件概率：已知 $A$ 发生情况下 $B$ 发生的概率

$$P(B\vert A)=\frac{P(AB)}{P(A)}, \ P(A)>0$$

性质：

1. $0\le P(B\vert A)\le 1$ 
2. $P(S\vert A)=1$
3. $P(\overline{A}\vert B)=1-P(A\vert B)$
4. $P(A+B\vert C)=P(A\vert C)+P(B\vert C)-P(AB\vert C)$ 

乘法公式：本质就是条件概率公式的变形

$$P(AB)=P(B\vert A)P(A)$$

## 全概率公式和贝叶斯公式

完备事件组：一组事件能将样本空间铺满，且他们两两之间没有交集，就是完备事件组。又称为样本空间的划分。

全概率公式：对于完备事件组 $B_1,B_2,...,B_n$ 有

$$P(A)=\sum_{i=1}^n P(AB_i)=\sum_{i=1}^n P(A\vert B_i)P(B_i)$$

理解：前一个等式是利用 $B$ 将 $A$ 进行完备划分，后一个等式是对中间项使用了乘法公式。

贝叶斯公式：

$$\displaystyle P(B_i\vert A)=\frac{P(B_iA)}{P(A)}=\frac{P(A\vert B_i)P(B_i)}{\displaystyle\sum_{j=1}^n P(A\vert B_j)P(B_j)}$$

理解：前一个等式是条件概率公式，后一个等式是分子用乘法公式（也可以理解为针对另一事件的条件概率公式逆运用），分母是全概率公式。

## 事件的独立性

$$
\begin{aligned}
A\ 和\ B\ 相互独立
    & \Leftrightarrow P(AB)=P(A)P(B) \\
    & \Leftrightarrow P(B)=P(B\vert A) \\
    & \Leftrightarrow P(B\vert A)=P(B\vert \overline{A}) \\
    & \Leftrightarrow P(A\vert B)=P(A\vert \overline{B})
\end{aligned}
$$

事件 $A$ 和 $B$ 相互独立，则 $A$ 和 $\overline{B}$ ， $\overline{A}$ 和 $B$ ， $\overline{A}$ 和 $\overline{B}$ 也相互独立。

如果两个事件的概率都不为零，那么他们独立则不互斥，互斥则不独立。

三个事件相互独立，必须同时满足

$$
\begin{aligned}
    & P(AB)=P(A)P(B) \\
    & P(AC)=P(A)P(C) \\
    & P(BC)=P(B)P(C) \\
    & P(ABC)=P(A)P(B)P(C)
\end{aligned}
$$

如果只满足前三个，叫做两两相互独立，但不是三个事件相互独立。

# 第二章 随机变量及其概率分布

## 离散型随机变量

### 基本概念

随机变量：随机变量 $X$ 是定义在随机试验样本空间 $S=\{e\}$ 上的单实值函数，记为 $X=X(e)$ 。实际上是一个函数，将事件映射为数值，使之可以用数学手段处理。

离散型随机变量：随机变量的全部可能取值是**有限个**或**可列无限个**。可列无限个是指能与自然数一一对应上。

对于离散型随机变量 $X$ 的所有可能取值 $x_k(k=1,2,...n)$ ， $X$ 取到各个可能取值的概率为 $P(X=x_k)=p_k,(k=1,2,3,...)$ 成为随机变量 $X$ 的概率分布（或分布律）。

性质：

1. $\displaystyle p_k\ge 0, (k=1,2,...)$ 
2. $\displaystyle \sum_{k=1}^{+\infty} p_k=1$ 

随机变量的分布函数：可以视为随机变量概率的前缀和， $F(x)=P(X\le x), -\infty<x<+\infty$ 。用于作差查询区间概率。

特点：

1. 单调不减
2. $F(a<X\le b)=F(b)-F(a)$  
3. $0\le F(x) \le 1,\ F(-\infty)=0, \ F(+\infty)=1$
4. 离散型随机变量的分布函数右连续，左不连续，存在跳跃间断点 

### 三个重要的离散型随机变量

0-1分布： $\displaystyle P(X=1)=p,\  P(X=0)=1-p$ ，其实是 $n=1$ 条件下的二项分布的特殊情况

二项分布：  $\displaystyle P(X=k)=C_n^k p^k q^{n-k},(k=0,1,2,...)$ ，简记为 $B(n,p)$ 

泊松分布： $\displaystyle P(X=k)=\frac{\lambda^k e^{-\lambda}}{k!},(k=0,1,2,...)$ ，简记为 $P(\lambda)$ 或 $\pi(\lambda)$ 。其中 $\lambda>0$ 是一个常数，表示单位时间（空间）内随机事件发生的平均次数。

### 泊松定理

设 $\lambda>0$ 是一个常数， $n$ 是任意正整数，设 $np_n=\lambda$ ，则对于任一固定的非负整数 $k$ ，有

$$\displaystyle \lim_{n\rightarrow \infty} C_n^k p_n^k(1-p_n)^{n-k}=\frac{\lambda^k e^{-\lambda}}{k!}$$

当 $n$ 很大， $p$ 很小的时候，二项分布就变成了泊松分布，且 $\lambda=np$ 。

$$P(X=k)=C_n^k p^k q^{n-k} \approx \frac{(np)^k e^{-np}}{k!}$$

优点：不需要再求出 $n$ 和 $p$ 具体是多少，只需要知道一个乘积就可以知道其分布情况。

## 连续型随机变量

### 基本概念

若存在非负可积函数 $f(x)$ ，使得对任意实数 $x$ 都有

$$F(x)=\int_{-\infty}^x f(t) \mathrm{d} t$$

则称 $f(x)$ 为连续型随机变量 $X$ 的概率密度。

简言之，概率密度函数是分布函数的导数，分布函数是概率密度函数的积分（从负无穷积到 $x$ ）。

特点：

1. $f(x)\ge 0$
2. $\displaystyle \int_{-\infty}^{+\infty} f(x) \mathrm{d} x = 1$  
3. $\displaystyle P(x_1<X\le x_2)=F(x_2)-F(x_1)=\int_{x_1}^{x_2}f(x) \mathrm{d} x$ ，端点取不取等都一样
4. 若 $f(x)$ 在点 $x$ 处连续，则有 $F'(x)=f(x)$ 

### 三个重要的连续型随机变量

|          |                                                    均匀分布                                                    |                                                 指数分布                                                  |                                                                   正态分布                                                                    |
| :------: | :------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------: |
|   记法   |                                                 $X\sim U[a,b]$                                                 |                           $X\sim E(\lambda) \ 或 \ X\sim \mathrm{Exp}(\lambda)$                           |                                                            $X\sim N(\mu,\sigma^2)$                                                            |
| 概率密度 |       $f(x)=\left\{\begin{aligned} & \frac{1}{b-a}, && a\le x\le b \\ & 0, && 其他\end{aligned}\right.$        | $f(x)=\left\{\begin{aligned}& \lambda \mathrm{e}^{-\lambda x}, && x>0 \\& 0, && 其他\end{aligned}\right.$ |            $\displaystyle \varphi(x)=\frac{1}{\sqrt{2\pi}\sigma} \mathrm{e}^{-\frac{(x-\mu)^2}{2\sigma^2}}, \\ -\infty<x<+\infty$             |
| 分布函数 | $F(x)=\left\{\begin{aligned}& 0, && x\le a \\& \frac{x-a}{b-a}, && a<x<b \\& 1, && x\ge b\end{aligned}\right.$ |   $F(x)=\left\{\begin{aligned} & 1-\mathrm{e}^{-\lambda x}, && x>0 \\ & 0, && 其他\end{aligned}\right.$   | $\displaystyle F(x)=\frac{1}{\sqrt{2\pi}\sigma}\int_{-\infty}^x \mathrm{e}^{-\frac{(t-\mu)^2}{2\sigma^2}} \mathrm{d} t, \\ -\infty<t<+\infty$ |
|   特点   |                                                 相当于几何概型                                                 |                                    无记忆性 $P(X>s+t\vert X>s)=P(X>t)$                                    |                                    关于 $x=\mu$ 对称，在 $x=\mu$ 取最大值，在 $\mu\plusmn\sigma$ 处是拐点                                     |


<!-- #### 均匀分布

记法： $X\sim U[a,b]$ 

概率密度：

$$
f(x)=
\left\{
\begin{aligned}
    & \frac{1}{b-a}, && a\le x\le b \\
    & 0, && 其他
\end{aligned}
\right.
$$

分布函数：

$$
F(x)=
\left\{
\begin{aligned}
    & 0, && x\le a \\
    & \frac{x-a}{b-a}, && a<x<b \\
    & 1, && x\ge b
\end{aligned}
\right.
$$

非常简单，就相当于几何概型。

#### 指数分布

记法： $X\sim E(\lambda)$ 或 $X\sim Exp(\lambda)$ 

概率密度：

$$
f(x)=
\left\{
\begin{aligned}
    & \lambda \mathrm{e}^{-\lambda x}, && x>0 \\
    & 0, && 其他
\end{aligned}
\right.
$$

分布函数：

$$
F(x)=
\left\{
\begin{aligned}
    & 1-\mathrm{e}^{-\lambda x}, && x>0 \\
    & 0, && 其他
\end{aligned}
\right.
$$

指数分布的无记忆性：对于任意的 $s,t>0$ ，有 $P(X>s+t\vert X>s)=P(X>t)$ 。

#### 正态分布

记法： $X\sim N(\mu,\sigma^2)$

概率密度：

$$\displaystyle \varphi(x)=\frac{1}{\sqrt{2\pi}\sigma} \mathrm{e}^{-\frac{(x-\mu)^2}{2\sigma^2}},-\infty<x<+\infty$$

关于 $x=\mu$ 对称，在 $x=\mu$ 取最大值，在 $\mu\plusmn\sigma$ 处是拐点。

分布函数：

$$F(x)=\frac{1}{\sqrt{2\pi}\sigma}\int_{-\infty}^x \mathrm{e}^{-\frac{(y-\mu)^2}{2\sigma^2}} \mathrm{d} y, -\infty<y<+\infty$$ -->

### 标准正态分布

$$X\sim N(0,1), \mu=0, \sigma=1$$ 

$$f(x)=\frac{1}{\sqrt{2\pi}} \mathrm{e}^{-\frac{x^2}{2}}$$

性质：

1. 关于 $x=0$ 对称， $f(-x)=f(x)$
2. 【重要】分布函数 $\displaystyle \Phi(-x)=1-\Phi(x)$
3. 【非常重要】正态分布标准化：若 $\displaystyle X\sim N(\mu,\sigma^2)$ 则 $\displaystyle Z=\frac{X-\mu}{\sigma}\sim N(0,1)$ 。此时 $\displaystyle F(x)=P(X\le x)=P(\frac{X-\mu}{\sigma}\le \frac{x-\mu}{\sigma})=P(Z\le \frac{x-\mu}{\sigma})=\Phi(\frac{x-\mu}{\sigma})$ 
4. $\displaystyle P(x_1<X<x_2)=P(\frac{x_1-\mu}{\sigma}<\frac{X-\mu}{\sigma}\le\frac{x_2-\mu}{\sigma})=\Phi(\frac{x_2-\mu}{\sigma})-\Phi(\frac{x_1-\mu}{\sigma})$
5. $\displaystyle P(X\ge x)=1-P(X<x)=1-\Phi(\frac{x-\mu}{\sigma})$ 
6. $\Phi(a)-\Phi(-a)=2\Phi(a)-1$ 

## 随机变量函数的分布

> Q：什么叫“随机变量函数”？
> 
> A：简单来说就是随机变量套随机变量。更具体地，即一个随机变量 $Y$ 关于另一个随机变量 $X$ 的变化而变化，将这种关系抽象为一种函数关系，例如 $Y=2X+8$ 。
> 
> 这里的 $X$ 和 $Y$ 不是具体的实数，而是随机变量，其具有的属性也不是一个数值，而包含了定义域（样本空间）、分布律（或概率密度）、分布函数等多个属性。
>
> 由于随机变量本身就是一种函数，那么随机变量套随机变量其实是一种复合函数，相关的求导运算需要满足复合函数的运算律。
>
> Q：什么叫“随机变量函数的分布”？
>
> A：其实就是在上述的那种函数关系中，我们更加关注的是，如何从其中一个随机变量的定义域/概率密度/分布函数/分布律出发，得出另一个随机变量的定义域/概率密度/分布函数/分布律。

### 离散型随机变量函数的分布

$$
\begin{matrix}
X的分布律 & \xleftrightarrow{代数运算} & Y的分布律 \\
\updownarrow & & \updownarrow\\
X的分布函数 & & Y的分布函数
\end{matrix}
$$

这四个量可以知一求三。

### 连续型随机变量函数的分布

分布函数法：知道一个概率密度，要求另一个概率密度，不能直接求，必须借助两个分布函数作为中间量。

$$
\begin{matrix}
X的概率密度 & & Y的概率密度 \\
\updownarrow & & \updownarrow\\
X的分布函数 & \xleftrightarrow{反表示} & Y的分布函数
\end{matrix}
$$

这四个量也可以知一求三。

