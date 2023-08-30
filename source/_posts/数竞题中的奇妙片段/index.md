---
title: 数竞题中的奇妙片段
date: 2023-08-26 01:25:36
tags:
---



1. 求 $\displaystyle\lim_{n\to \infty} {n \sin(2\mathrm{\pi}\mathrm{e}n!)}$ .

    **分析** 本题乍一看有些反直觉， $\sin$ 是一个周期震荡的有界函数，与正无穷的 $n$ 相乘为什么还能有极限呢？问题就出现在 $2\mathrm{\pi}$ 上，这个 $2\mathrm{\pi}$ 的存在使得 $\mathrm{e}n!$ 的相位不是随机震荡的，而是有一个趋近的值。

    **解** 把  $\mathrm{e}n!$ 的整数部分和小数部分求出来，就可以用诱导公式舍弃整数部分，得出 $\sin$ 的真实值。

    由于 $\displaystyle\mathrm{e}=1+1+\frac{1}{2!}+\frac{1}{3!}+\cdots+\frac{1}{n!}+o\left(\frac{1}{(n+1)!}\right)$ ，所以

    $$

    \begin{aligned}
    \lim_{n\to \infty} n \sin(2\mathrm{\pi}\mathrm{e}n!)
        & = \lim_{n\to \infty} {n \sin\left(\frac{2 \mathrm{\pi}}{n+1}+o\left(\frac{1}{n+1}{}\right)\right)} \\
        & = \lim_{n\to \infty} {n \left(\frac{2 \mathrm{\pi}}{n+1}+o\left(\frac{1}{n+1}\right)\right)} \\
        & = 2 \mathrm{\pi}
    \end{aligned}  
    $$

    **评注** 由此可见，一个正无穷与一个周期有界函数相乘不一定就是正无穷，周期函数的相位控制的比较合适的时候，也有可能存在极限。

2. 求 $\displaystyle\lim_{n\to \infty} {\frac{1}{n}\ln {\frac{n!}{n^n}}}$ .

    **解** 阶乘式 $n!$ 作为一个整体很不好处理，这里应该拆开。

    $$
    \begin{aligned}
    \lim_{n\to \infty} {\frac{1}{n}\ln {\frac{n!}{n^n}}}
        & = \lim_{n\to \infty} {\frac{1}{n}\left(\ln \frac{1}{n}+\ln {\frac{2}{n}+\cdots+\ln {\frac{n}{n}}}\right)} \\
        & = \lim_{n\to \infty} {\frac{1}{n}\sum_{k=1}^n \ln {\frac{k}{n}}} \\
        & = \int_{0}^{1}{\ln {x}} \mathrm{d} {x} \\
        & = [x \ln {x} - x]_0^1 \\
        & = -1
    \end{aligned}
    $$

    **评注** 待求极限式同时包含 $\displaystyle\frac{1}{n}$ 与 $\displaystyle\frac{k}{n}$ 的时候，考虑使用定积分的定义，将和式转化为 $[0,1]$ 上的定积分来计算。

3. 施笃兹定理：设数列 $\{y_n\}$ 单调递增且 $\displaystyle\lim_{n\to \infty} {y_n} = +\mathrm{\infty}$ ，如果 $\displaystyle\lim_{n\to \infty} {\frac{x_{n+1}-x_n}{y_{n+1}-y_n}}$ 存在或为 $\infty$ ，则 $\displaystyle\lim_{n\to \infty} {\frac{x_n}{y_n}}=\lim_{n\to \infty} {\frac{x_{n+1}-x_n}{y_{n+1}-y_n}}$

    简记为： $\displaystyle\frac{\infty}{\infty}$ 形式的极限等于分子分母各自差分的极限。这里的差分类似于连续函数的求导，因此施笃兹定理又称为数列极限的洛必达法则。

4. 对于只给定递推式的数列 $x_n$ ，要证明收敛可能比较难办。

    可以考虑先证明其差分数列的无穷和 $\displaystyle\sum_{n=0}^\infty (x_{n+1}-x_{n})$ 绝对收敛，从而得到 $x_n$ 收敛。

5. 对于只给定递推式的数列 $x_n$ ，要求极限，应当这样做：

    1. 先用 4 中给的方法证明其收敛（证明极限存在）；
    2. 设其极限为 $a$ ；
    3. 将 $a$ 代入递推公式中的 $x_{n+1},x_n,x_{n-1}$ 等项，得到一个关于 $a$ 的一元方程；
    4. 解方程得到极限。
