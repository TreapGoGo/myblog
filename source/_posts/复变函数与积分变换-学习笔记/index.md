---
title: 复变函数与积分变换 学习笔记
date: 2023-07-26 06:24:50
tags:
---

# 前言

本文由周国全老师的数学物理方法、翔翔的学习频道在B站上传的视频总结而来，借用了华中科技大学的讲义。

感谢周国全老师、UP主翔翔和华中科技大学！

# 第一章 解析函数

## 第一节 复数及其运算

### 基本定义

对于复数 $z=x+\mathrm{i}y\in\mathbb{C}$ ，称 $x=\mathrm{Re}z$ 为其实部， $y=\mathrm{Im}z$ 为其虚部。两复数相等，当且仅当他们的实部和虚部分别相等。用 $\overline{z}$ 或 $z^*$ 表示与 $z$ 实部相等、虚部相反的**共轭复数**（complex conjugation）。

通过两个共轭复数可以得出实部和虚部，这个方法非常重要。

$$x=\frac{z+\overline{z}}{2}$$

$$y=\frac{z-\overline{z}}{2\mathrm{i}}$$

复数与复平面上的点一一对应，与向量一一对应。

复数的表示方法：

1. 复向量 $\overrightarrow{OP}$
2. 三角函数表示法（极坐标） $\displaystyle z=\rho\cos\varphi+\mathrm{i}\rho\sin\varphi$ ，
3. 指数表示法 $\displaystyle z=\rho e^{\mathrm{i}\varphi}$
4. 黎曼球表示法

### 三角函数表示法

$\displaystyle \varphi=\mathrm{Arg}z$ 为辐角， $\rho$ 为模

辐角 $\varphi$ 满足 $\displaystyle\tan\varphi=\frac{y}{x}$ ，其主值 $\mathrm{arg}z\in(-\pi,\pi]$ 在第一、四象限与 $\displaystyle\arctan\frac{y}{x}$ 相等，在第二、第三象限与 $\displaystyle\arctan\frac{y}{x}$ 相差 $\pi,-\pi$ 。

理解： $\arctan$ 值域为 $\displaystyle\left(-\frac{\pi}{2},\frac{\pi}{2}\right)$ ，算出来的角都落在第一、第四象限。为了使得辐角主值和复数的坐标相匹配，对于原本在第二（第三）象限但却被 $\arctan$ 算到第四（第一）象限的那些角顺时针（逆时针）旋转 $180\degree$ ，使其回到第二（第三）象限。

### 黎曼球表示法（了解即可）

复球面与复平面切于复球面南极点 $S$ ，复球面的北极点 $N$ 与复球面上一点 $A'$ 延长线交复平面于点 $A$ 。

复球面上每一点与复平面上每一点一一对应，所以可以用 $A'$ 的球坐标来代表 $A$ 的坐标。

当 $A'\rightarrow N$ 时，复平面上的 $A$ 趋于无穷远点。

![复球面示意图](复球面示意图.png)

### 复数的运算

复数的四则运算

三角形式：

* 乘法： $\displaystyle z_1z_2=r_1r_2[\cos(\theta_1+\theta_2)+\mathrm{i}\sin(\theta_1+\theta_2)]$
* 除法： $\displaystyle z_1z_2=\frac{r_1}{r_2}[\cos(\theta_1-\theta_2)+\mathrm{i}\sin(\theta_1-\theta_2)]$
* 乘方：参见棣莫弗公式

指数表达式：

* 乘法： $\displaystyle z_1\cdot z_2=\rho_1\rho_2\mathrm{e}^{\mathrm{i}(\varphi_1+\varphi_2)}$
* 除法： $\displaystyle \frac{z_1}{z_2}=\frac{\rho_1}{\rho_2}\mathrm{e}^{\mathrm{i}(\varphi_1-\varphi_2)}$
* 乘方： $\displaystyle z^n=\rho^n \mathrm{e}^{\mathrm{i}n\varphi}$
* 开方： $\displaystyle \sqrt[m]{z}=\sqrt[m]{\rho}\mathrm{e}^{\mathrm{i}\frac{\varphi}{m}}=\sqrt[m]{\rho}\mathrm{e}^{\mathrm{i}\frac{\mathrm{arg}z+2k\pi}{m}}, \ m=2,3,\cdots;k=0,\plusmn1,\plusmn2,\cdots$

复数开方有 $n$ 个 $n$ 次方根，其辐角也有 $n$ 个不同的取值， $k$ 把 $0,1,\cdots.n-1$ 取遍即可得到这 $n$ 个根，后面的都是重复的。这些根通常以共轭复根的形式配对出现，可以用这个规律来检查计算结果。

引申：如果三次方程至少有一个实根，如果有复数根，那么这两个复数根共轭。

棣莫弗公式：本质是欧拉公式当 $\rho=1$ 时的特例

$$(\cos\varphi+\mathrm{i}\sin\varphi)^n=\cos n\varphi+\mathrm{i}\sin n\varphi$$

### 复数的几何意义

复数与平面上的点一一对应。

应用：平面曲线 $f(x,y)=0$ 化为复表示方程 $F(z)=0$ 的方法

* 将  $\displaystyle\begin{cases}x=\frac{z+\overline{z}}{2} \\ y=\frac{z-\overline{z}}{2\mathrm i}\end{cases}$ 代入原方程 $f(x,y)=0$ ，即可得到以复数 $z$ 表示的方程。

两个复数相加，符合对角线规则。两个复数作差，符合三角形规则。

根据“平行四边形对角线平方和等于四条边平方和”，可得

$$|z_1+z_2|^2+|z_1-z_2|^2=2(|z_1|^2+|z_2|^2)$$

对于焦点在 $F_1,F_2$ 的椭圆上的一点 $P(x,y)$ ，易知 $\displaystyle|PF_1|=\left|x-(-\frac{c}{2})+(y-0)\mathrm i\right|=\left|z-\frac{c}{2}\right|$ ，同理 $\displaystyle|PF_2|=\left|z-\frac{c}{2}\right|$ .

根据椭圆的定义，椭圆方程也可以表示为  

$$\displaystyle \left|z+\frac{c}{2}\right|+\left|z-\frac{c}{2}\right|=2a$$

同理，直线 $ax+by+c=0$ 的复表示方程为

$$(a-\mathrm{i}b)z+(a+\mathrm{i}b)\overline{z}+2c=0$$

圆 $(x-a)^2+(y-b)^2=r^2$ 的复表示方程为

$$|z-(a+\mathrm{i}b)|=r$$

<!-- 以上内容是根据 B 站周国全录播记录，太过繁琐，故切换为翔翔的学习频道 -->

## 第二节 复变函数

### 定义

设 $E\subset \mathbb{C}$ ，对于 $\forall z \in E$ 都有一个或多个 $w$ 使得

$$
w=f(z),z \in E
$$

则称 $w=f(z)$ 是 $z$ 的复变函数。

注意这里的函数一个自变量可能对应多个因变量，例如 $w=\sqrt{z}$ 就是一个多值函数。

同时注意到，若 $z=x+\mathrm{i}y$ ，则有

$$
w=f(z)=u(x,y)+\mathrm{i}v(x,y)
$$

这本质是一个关于 $x,y$ 的二元函数，因此研究复变函数其实也就是研究二元函数。

### 区域

需要注意的只有以下几点：

1. 沿着区域的边界走，区域在左方，则走向为边界的正向；
2. 闭区域 $\overline{\sigma}=\sigma+l$ 包括了区域和边界。

### 复变函数的连续性与极限

类似于二元函数的连续性，详细证明要用到 $\varepsilon-\delta$ 语言，这里不做展开。

性质：

1. 若两个函数在某点连续，则其和差积商在该点依然连续；
2. 在某点连续的函数，其复合函数在该点仍然连续；
3. 若 $f(z)$ 在闭区域 $\overline{\sigma}$ 上连续，则
   1. $f(z)$ 在 $\overline{\sigma}$ 上有界
   2. $|f(z)|$ 在 $\overline{\sigma}$ 上有最大值和最小值
   3. $f(z)$ 在 $\overline{\sigma}$ 上一致连续
4. $f(z)$ 在 $z_0$ 连续 $\Leftrightarrow$  $u(x,y),v(x,y)$ 在 $z_0$ 点连续。

## 第三节 解析函数

### 复变函数导数的定义

$$
\lim_{\Delta z\to 0} {\frac{\Delta f}{\Delta z}} = \lim_{\Delta z\to 0} {\frac{f(z+\Delta z)-f(z)}{\Delta z}}
$$

其求导法则与实变函数的求导法则一模一样，不做展开。

### 解析与柯西-黎曼条件

若 $f(z)$ 在邻域 $U(z_0)$ 中处处可导，则称 $f(z)$ 在 $z_0$ 处解析。解析又称为全纯。

解析必然可导，可导不一定解析

 $f(z)=u(x,y)+\mathrm{i}v(x,y)$ 在区域 $D$ 上解析的充分必要条件是：

 $f(z)$ 在区域 $D$ 上有一阶连续偏导数，且满足柯西黎曼方程

$$
\frac{\partial {u}}{\partial {x}} = \frac{\partial {v}}{\partial {y}} \\
$$

$$
\frac{\partial {u}}{\partial {y}} = -\frac{\partial {v}}{\partial {x}}
$$

复变函数求导，可以用 C-R 方程转换形式。

### 调和函数

若二元实函数 $u(x,y)$ 有二阶连续偏导数，且满足拉普拉斯方程

$$
\frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} = 0
$$

则称 $u(x,y)$ 为调和函数。

若 $f(z)$ 是解析函数，则其实部 $u(x,y)$ 和虚部 $v(u,v)$ 都是调和函数，且称这两个函数互为共轭调和函数。

### 初等解析函数

#### 指数函数

$$
\mathrm{e}^z=\mathrm{e}^{x+\mathrm{i}y}=\mathrm{e}^x(\cos y+\mathrm{i}\sin y)
$$

性质：

1. 是单值函数，因为定义中的 $\mathrm{e}^x, \cos y, \sin y$ 都是单值函数。
2.  $\mathrm{e}^z$ 除无穷远点外处处有定义。当 $y=0$ 时， $\mathrm{e}^z=\mathrm{e}^x$ ，此时满足 $\displaystyle\lim_{x\to -\infty} {\mathrm{e}^z}=0,\lim_{x\to +\infty} {\mathrm{e}^z}=+\infty$ .
3.  $\displaystyle|\mathrm{e}^z|=\mathrm{e}^x$ ，注意这个等式两边都是实数。
4.  $\displaystyle (\mathrm{e}^z)'=\mathrm{e}^z$ 
5. $\text{Arg } \mathrm{e}^z=y+2k \mathrm{\pi},  (k=0,\pm1,\pm2,\cdots)$
6. 周期为 $2 \mathrm{\pi} \mathrm{i}$ 。这个周期是对于 $z$ 的周期，而不是对于 $x$ 或 $y$ 的周期。

![指数函数周期性示意图](指数函数周期性示意图.png)

指数函数非常重要。其他的初等函数都通过指数函数来定义。

#### 对数函数

对数函数定义为指数函数的反函数。满足方程 $\mathrm{e}^w=z$ 的函数 $w=f(z)$ 就是对数函数，记作 $w=\text{Ln }z$ 。

推导过程如下：令 $z=r \mathrm{e}^{\mathrm{i}\theta},w=u+\mathrm{i}v$ ，代入定义式 $e^w=z$ 得到

$$
\mathrm{e}^u\cdot \mathrm{e}^{\mathrm{i}v} = r\cdot \mathrm{e}^{\mathrm{i}\theta}
$$

为了使得该等式成立，需要满足：

$$
\left\{
\begin{aligned}
    & u=\ln {r}=\ln {|z|} \\
    & v=\theta=\text{Arg }z
\end{aligned}
\right.
$$

综合上述式子，得到 $\text{Ln }z$ 的具体计算公式：

$$
w=\text{Ln } z=\ln |z|+\mathrm{i}(\theta+2k \mathrm{\pi}), (k=0,\pm1,\pm2,\cdots)
$$

性质：

1.  $\text{Ln } z$ 是多值函数。 $k=0$ 时的函数值是函数的主值，记为 $w=\ln {z}$ ，这就是一个单值函数。对于某一个确定的 $k$ ，称 $w=\ln {z}+2k \mathrm{\pi}\mathrm{i}$ 为 $\text{Ln }z$ 的一个分支。
2. 定义域为 $z\ne0$ 。这是因为 $\ln|z|,\text{arg }z$ 在原点都没有意义，也可以理解为 $\mathrm{e}^w\ne0$ 。
3.  $\text{Ln } z$ 的各个分支在除去原点和负实轴的复平面内连续且解析，且满足 $\displaystyle \frac{\mathrm{d} {\text{Ln }z}}{\mathrm{d} {z}}=\frac{\mathrm{d} {\ln {z}}}{\mathrm{d} {z}} = \frac{1}{z}$ 。

### 幂函数

其定义为：

$$
w=z^\alpha=\mathrm{e}^{\alpha\text{Ln }z}
$$

性质：

1.  $(z^\alpha)'=\alpha z^{\alpha-1}$ 
2.  $z^0=1$ 
3.  $\displaystyle z^\frac{m}{n}=\sqrt[n]{z^m}$ 
4.  若 $\alpha$ 为无理数或复数， $z^\alpha$ 一般是多值函数，此时处原点与负实轴外处处解析。

### 三角函数

根据欧拉公式 $\mathrm{e}^{\mathrm{i}\theta}=\cos\theta+\mathrm{i}\sin\theta$ ，有 $\mathrm{e}^{-\mathrm{i}\theta}=\cos\theta-\mathrm{i}\sin\theta$ 

故有

$$
\cos \theta=\frac{\mathrm{e}^{\mathrm{i}\theta}+\mathrm{e}^{-\mathrm{i}\theta}}{2} \\
\sin \theta=\frac{\mathrm{e}^{\mathrm{i}\theta}-\mathrm{e}^{-\mathrm{i}\theta}}{2 \mathrm{i}} 
$$

将其中的 $\theta$ 全部换成 $z$ ，得到正余弦函数的定义：

$$
\cos z=\frac{\mathrm{e}^{\mathrm{i}z}+\mathrm{e}^{-\mathrm{i}z}}{2} \\
\sin z=\frac{\mathrm{e}^{\mathrm{i}z}-\mathrm{e}^{-\mathrm{i}z}}{2 \mathrm{i}} 
$$

其他三角函数的定义仿照实变函数退出即可。

性质:

1. 是单值函数。
2. 周期性、可导性、奇偶性、零点与实变函数一样。
3. 所有三角公式依然适用。
4. 有界性不成立，因为 $|\cos z|\le1,|\sin z|\le1$ 显然不满足。

### 反三角函数

运用三角函数的定义，求出使得 $\cos w=z$ 成立的 $w$ ，便可定义 $w=\text{Arccos }z$ 。

$$
z=\cos w= \frac{\mathrm{e}^{\mathrm{i}w}+\mathrm{e}^{-\mathrm{i}w}}{2} \\
\Rightarrow (\mathrm{e}^{\mathrm{i}w})^2-2z\mathrm{e}^{\mathrm{i}w}+1=0 \\
\Rightarrow \mathrm{e}^{\mathrm{i}w}=z+\sqrt{z^2-1} \\
\Rightarrow w=\text{Arccos }z=-\mathrm{i}\text{Ln }(z+\sqrt{1+z^2})
$$

同理可得

$$
\text{Arcsin }z =-\mathrm{i}\text{Ln }(\mathrm{i}z+\sqrt{1-z^2})\\
\text{Arctan }z = \frac{\mathrm{i}}{2} \text{Ln }\frac{\mathrm{i}+z}{\mathrm{i}-z}
$$

### 双曲函数与反双曲函数

略