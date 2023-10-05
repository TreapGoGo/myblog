---
title: 金融随机过程 学习笔记
date: 2023-10-05 16:24:23
tags:
---

# 前言

本文是本人自学清华大学出版社《随机过程及其在金融领域中的应用》时的同步笔记。

# 第 3 章 随机过程

## 随机过程的基本概念

随机过程  $\{X(\omega, t): \omega\in\Omega,t\in T\}$ 是关于时间参数 $t$ 和样本点 $\omega$ 的二元函数。它的自变量类型是两个数值（离散的或连续的），它的函数值类型是一个随机变量集合。简单理解，随机过程将 **二维平面上的某点** 映射到 **某个随机变量集合** 。有时可以简单表示成 $\{X_t(\omega)\}, \{X_t\}$ 或 $\{X(t)\}$  。

下面的伪代码可以帮助理解随机过程的定义。

```python
import RandomVariable

class RandomProcess:
    def __init__(omega: double, t:double) -> list:
        random_variables = []
        ...
        v = do_something(omega, t)
        random_variables.append(RandomVariable(v))
        ...
        return random_variables
```

* 对于确定的时间 $t_0$ ， $X(w, t_0)$ 是概率空间上的一维随机变量。

* 对于确定的样本点 $\omega_0$ ， $X(w, t_0)$ 是定义在 $T$ 上的实函数，称为对应于 $\omega_0$ 的一个样本函数（或样本轨道、实现）。

对于每一时刻 $t_1\in T$ ， $X_{t_1}$ 是一维随机变量，其一维分布函数为 

$$
F_{t_1} = P(X_{t_1}\le x_1)
$$

类似地，对于任意两个时刻 $t_1,t_2\in T$ ，随机过程的取值有 $X_{t_1},X_{t_2}$ 两个，其二维分布函数为

$$
F_{t_1,t_2}(x_1,x_2) = P(X_{t_1}\le x_1, X_{t_2}\le x_2)
$$

拓展到 $n$ 维，可得 $n$ 维联合分布函数

$$
F(x_1,\cdots,x_n; t_1,\cdots,t_n) = P(X_{t_1}\le x_1,\cdots,X_{t_n}\le x_n)
$$

 $n$ 维联合密度函数为

$$
f(x_1,\cdots,x_n; t_1,\cdots,t_n)
$$

可见，**分布函数和密度函数的维度数等于时间点的个数**。如果时间集 $T$ 是有限集，那么分布函数的全体

$$
\{F(x_1,\cdots,x_n; t_1,\cdots,t_n): n\ge1, t_1,\cdots,t_n\in T\}
$$

称为随机过程 $\{X_t\}$ 的有穷维分布函数。

## 随机过程的数字特征

由于随机过程本质上就是二维随机变量，其数字特征与二维随机变量的数字特征完全一致，没有特别之处。

对于任一时间 $t\in T$ ，随机过程 $\{X_t\}$ 的数学期望定义为

$$
\mu_{X_t} = E(X_t) = \int_{-\infty}^{+\infty}{x} \mathrm{d} {F_t(x)} = \int_{-\infty}^{+\infty}{xf_t(x)} \mathrm{d} x
$$

方差定义为

$$
\sigma_{X_t}^2 = V(X_t) = E[(X_t-E(X_t))^2]
$$

二阶中心矩定义为

$$
E(X_t^2) = \int_{-\infty}^{+\infty}{x^2} \mathrm{d} {F_t(x)} = \int_{-\infty}^{+\infty}{x^2f_t(x)} \mathrm{d} x
$$

对于任意 $t_1,t_2\in T$ ，其协方差函数定义为

$$
c_X(t_1,t_2) = \sigma_{X_{t_1},X_{t_2}} = E((X_{t_1}-E(X_{t_1}))(X_{t_2}-E(X_{t_2})))
$$

相关函数定义为

$$
R(t_1,t_2) = E(X_{t_1}X_{t_2})
$$

如果 $\forall t\in T$ 都有 $E(X_t^2) < +\infty$ ，称 $\{X_t, t\in T\}$ 为实二阶矩过程。

## 正态随机过程

如果随机过程 $\{X_t\}$ 的任意 $n$ 维分布都是正态分布，则称为正态随机过程或高斯随机过程。其概率密度为

$$
\begin{aligned}
    f(x_1,\cdots,x_n; t_1,\cdots,t_n) 
    & = \frac{1}{(2\pi)^{n/2}(\det \bm{\Sigma})^{1/2}} \exp \left\{-\frac{1}{2}(\bm x-\bm\mu)\bm \Sigma^{-1}(\bm x-\bm\mu)^T\right\} \\
    & = \frac{1}{(2\pi)^{n/2}\sigma_{X_1}\cdots\sigma_{X_n}} \exp \left\{-\frac{1}{2}\sum_{i=1}^n \frac{(x_i-\mu_{X_i})^2}{\sigma_{X_i}^2}\right\} \\
    & = \prod_{i=1}^n \frac{1}{\sqrt{2\pi}\sigma_{X_i}} \exp \left\{-\frac{(x_i-\mu_{X_i})^2}{2\sigma_{X_i}^2}\right\} \\
    & = f(x_1;t_1) \cdots f(x_n;t_n)
\end{aligned}
$$

也就是说，正态过程在 $n$ 个不同时刻 $t_1,\cdots,t_n$ 采样，得到的一族随机变量两两互不相关，相互独立。对于正态过程，不相关与独立是等价的。

## 泊松过程

设 $\{N_t\}$ 是一个泊松过程，其中 $N_t$ 表示从 $0$ 时刻到 $t$ 时刻事件发生的次数。其参数为 $\lambda$ ，表示单位时间内事件发生的期望次数，称为此过程的速率或强度。泊松过程是一个平稳增长的计数过程，是齐次的独立增量过程。

泊松过程的数字特征与泊松分布完全一致。

* 均值： $E(N_t) = \lambda t$ 
* 方差： $V(N_t) = \lambda t$ 
* 矩： $E(N_t^2) = \lambda t + (\lambda t)^2$ 
* 自相关函数： $R(t_1,t_2) = E(N_{t_1}N_{t_2}) = \lambda t_1 + \lambda^2 t_1t_2$ 
* 自协方差： $c_X(t_1,t_2) = \sigma_{N_{t_1},N_{t_2}} = \lambda t_1$ 
* 特征函数：略

其中 $t_1<t_2$ 。

验证一个过程是泊松过程，需要同时满足下面四个条件：

1.  $N_0=0$
2. 过程具有平稳的独立增量
3.  $P\{N_h=1\} = \lambda h + o(h), h>0$  
4.  $P\{N_h\ge2\} = o(h), h>0$ 

其中 $h$ 是一个很小的数。

直观地理解条件 3 ，其实就意味着，在一段很短的时间内，事件发生一次的概率其实和时间的长度正相关，而系数就是单位时间内发生的次数 $\lambda$ 。直观地理解条件 4 ，其实就意味着，在一段很短的时间内，事件连续发生两次的概率非常非常小，远小于发生一次的概率；如果时间段长度变短，那么这个概率将会以更快的速度趋于 $0$ 。

