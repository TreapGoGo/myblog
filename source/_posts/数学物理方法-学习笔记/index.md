---
title: 数学物理方法 学习笔记
date: 2023-09-11 07:56:30
categories: "数学"
tags: ["学习笔记", "数学", "物理", "复变函数"]
---

# 前言

本文是武汉大学江先阳老师 2023 年秋《数学物理方法》课程的同步笔记。

由于我之前已经自学了《复变函数与积分变换》，这篇笔记的开设只是为了让我上课时能够不摆烂。

感谢江先阳老师和物理科学与技术学院！

# 第零篇 绪论

江先阳，电路设计爱好者，主要研究方向集成电路设计。

## 引子

> 早发白帝城
>
> 唐（李白）
> 
> 朝辞白帝彩云间，千里江陵一日还。
> 
> 两岸猿声啼不住，轻舟已过万重山。

这首诗包含了数学物理方法的一些研究对象，例如时间、速度、波等，是物理学的诗意表达。

## 简介

数学物理方法：建立和研究描述物理现象的数学模型时所用到的数学方法。

本课程分为三个部分：

1. 复变函数
2. 数学物理方程
   1. 线性方程
   2. \* 非线性方程
   3. \* 积分方程
3. 特殊函数

数学物理方法以**概念、符号、关系式、方程等**方式来反映事物的复杂过程，它的表述方式和分析手段尤其特殊性。

数学物理方法研究物理问题有三个步骤：

1. 将物理问题翻译成数学语言，即导出定解问题
2. 求解
3. 求得的解答进行分析

数学物理方法是物理美和数学美的结合。

## 如何学

1. 掌握普通物理（力、电、热）中的重要结论、高等数学中的微积分和常微分方程；
2. 课堂学习跟上思路，适当做笔记，一次记住，而不是课下重学；
3. 课下复习、练习、提问题；
4. 多交流

每个同学需要出一道考试题。期末考试的题目，就来自于各位同学自己出的题（老师可能会改编）。

## 辅助资料

吴崇试 《数学物理方法》 北京大学出版社 2020

菲利克斯·克莱因 《高观点下的初等数学》 华东师范大学出版社 2020

## 课程考核

平时成绩：30%

* 期中考试
* 平时作业
* 签到率（每节课都有，总共至少签8次）~~（暗示可以旷课8次）~~
* 出期末考试题

期末考试：70% **（卷面分必须在55分以上，否则捞不动）**

# 第一篇 复变函数论

## 第一章 解析函数

### 第一节 复数及其运算

#### 引言

复变函数的内容：

* 将实变函数中函数、极限、连续、微商、积分、级数等概念推广到复变函数中。
* 解除了实数领域中的若干禁令。
* 建立了三角函数和指数函数、双曲函数的关系。

复变函数的直接应用：

* 解偏微分方程的边值问题，如：保角变换法；
* 解微分方程的初值问题，如：拉普拉斯变换法；
* 计算实积分，如：留数定理

本章的中心问题是解析函数的问题。

#### 复数的概念

复数的表示方法有两种：

$$
z=(x,y)=x+\mathrm{i}y
$$

因为复数和复平面上的点一一对应，所以可以写成坐标形式，但一般还是推荐写成多项式 $x+\mathrm{i}y$ 形式。

复数的共轭：

$$
\overline{z}=x-\mathrm{i}y
$$

共轭体现在复平面上，其实就是某个点关于 $x$ 轴的对称点或镜像。

复数的实部和虚部

$$
x=\text{Re}z
$$

$$
y=\text{Im}z
$$

性质：

1. 若 $z_1=x_1+\mathrm{i}y_1,z_2=x_2+\mathrm{i}y_2$ ，则 $\displaystyle z_1=z_2 \Leftrightarrow \begin{cases}x_1=x_2 \\ y_1=y_2\end{cases}$ 
2.  $z$ 无大小，也不能比大小，只能判等。
3. 对于有理运算 $R(a,b,c,\cdots)$ ，共轭复数满足 $\displaystyle\overline{R(a,b,c,\cdots)}=R(\overline{a},\overline{b},\overline{c},\cdots)$ 

#### 复数的表示方法

1. 几何表示
   1. 点 $z$ 
   2. 向量 $\overrightarrow{oz}$ 
   3. 极坐标 $(\rho,\phi)$ 
   4. 复球表示

    上述表示方法在中， $z$ 的模 $\displaystyle\rho=|z|=|\overrightarrow{oz}|=\sqrt{x^2+y^2}$ 

    $z$ 的辐角是多值， $\displaystyle\phi=\text{Arctg} \frac{y}{x}=\text{Arg}z$ 

    规定辐角主值 $\text{arg}z$ 满足

$$
-\mathrm{\pi} < \arg z \le \mathrm{\pi}
$$

$$
\text{Arg}z=\arg z \pm 2k \mathrm{\pi},k=0,1,2,\cdots
$$

因为 $\displaystyle\arctan \frac{y}{x}$ 是一个单值函数，其值域是 $\displaystyle(-\frac{\mathrm{\pi}}{2},\frac{\mathrm{\pi}}{2})$ ，不能覆盖到 $\displaystyle(-\mathrm{\pi},\mathrm{\pi}]$ ，所以反正切求辐角主值必须加以修正。

具体的， $\arg z$ 与 $\displaystyle\arctan \frac{y}{x}$ 的关系是

$$
\arg z = 
\begin{cases}
    \arctan \frac{y}{x}, & x>0,y>0 \text{（第一象限）} \\
    \arctan \frac{y}{x}+\mathrm{\pi}, & x<0,y>0 \text{（第二象限）}\\
    \arctan \frac{y}{x}-\mathrm{\pi}, & x<0,y<0 \text{（第三象限）} \\
    \arctan \frac{y}{x}, & x>0,y<0 \text{（第四象限）}
\end{cases}
$$

2. 代数表示
   
$$
z=
\begin{cases}
    x+ \mathrm{i} y \\
    \rho \cos \varphi +  \mathrm{i} \rho \sin \varphi \\
    \rho  \mathrm{e} ^{ \mathrm{i} \varphi}
\end{cases}
$$

3. 黎曼球表示

    规定 $\infty$ 为复平面上的无穷远点，其实部和虚部没有意义，但是可以作为整体参与运算。其运算规则与实变函数的无穷一样。

#### 复数的运算

复数的运算结果、运算规则都与实数相符合，且满足 $\mathrm{i} ^2 = -1$ 。

规定 $z_1=x_1+\mathrm{i}y_1, z_2=x_2+\mathrm{i}y_2$ 则有

$$
z_1\pm z_2=(x_1\pm x_2)+\mathrm{i}(y_1\pm y_2)
$$

$$
z_1\times z_2=(x_1x_2-y_1y_2)+\mathrm{i}(x_1y_2+y_1x_2)
$$

$$
\mathrm{e}^{\mathrm{i}\phi_1} \cdot \mathrm{e}^{\mathrm{i}\phi_2} = \mathrm{e}^{\mathrm{i}(\phi_1+\phi_2)}
$$

$$
\mathrm{e}^{\mathrm{i}\phi_1} / \mathrm{e}^{\mathrm{i}\phi_2} = \mathrm{e}^{\mathrm{i}(\phi_1-\phi_2)}
$$

$$
z_1^2-z_2^2=(z_1+z_2)(z_1-z_2)
$$

$$
(z_1+z_2)^n = \sum_{m=0}^n C_n^m z_1^m z_2^{n-m}
$$

$$
z^n=|z|^n \mathrm{e}^{\mathrm{i}n\text{Arg}z}
$$

$$
\sqrt[m]{z} = \sqrt[m]{|z|} \mathrm{e} ^ {\mathrm{i} \frac{\arg z + 2k \mathrm{\pi}}{m}},
\begin{cases} k=0,1,2,\cdots,m-1, \\ m\ge2  \end{cases}
$$

$$
(\cos \varphi + \mathrm{i} \sin \varphi)^n = \cos n\varphi + \mathrm{i} \sin n\varphi \text{（棣莫弗公式）}
$$

#### 作业

习题1.1: 1(2),(4); 2(3); 4(2); 6(2); 7(2)

### 第二节 复变函数

#### 定义

> 设 $E$ 为一点集， $f(z):z\in E\rightarrow w=u+\mathrm{i}v$ ，其中 $u,v$ 为实数。
> 
> 则 $w=f(z)$ 为复变函数，其中 $z\in E$ ， $E$ 为定义域， $w$ 为值域。

分类

* 单值
  * 单叶 $z\leftrightarrow w:(w=az+b)$ 
  * 多叶 $\displaystyle z_1,z_2,z_3,\cdots \rightarrow w : (w=z^2)$  
* 多值（例如 $\sqrt[3]{z}$ ）

#### 几何意义

将平面上的点集 $E$ 映射到点集 $F$ 。

例如，点集 $|z|=1$ 经过 $w=z^2$ 映射之后到点集 $\{(1,0)\}$ 

#### 区域

此部分直观理解即可，不需要记定义。

邻域：$\forall z\in|z-z_0|<\varepsilon$ 的点集称为 $z_0$ 的邻域。

去心邻域：$0<|z-z_0|<\varepsilon$ 表示去心邻域。

内点：若 $z_0$ 总有一个领域 $N(z_0,\varepsilon)$ 全含于点集 $\sigma$ 内，则称 $z_0$ 为 $\sigma$ 的内点。

区域：若点集 $\sigma$ 满足：

1. 全由内点组成
2. 设 $z_1\in\sigma, z_2\in\sigma$ ，且 $z_1,z_2$ 可以用全在 $\sigma$ 中的折线连接。

则称 $\sigma$ 是区域。

区域不包含边界，因为边界上的点不是内点。

外点：若 $z_0\notin \sigma$ 并且总有一个邻域 $N(z_0,\varepsilon)$ 不含有 $\sigma$ 的点，则称 $z_0$ 为 $\sigma$ 的外点。

界点：若 $z_0\notin \sigma$ 并且有总一个邻域 $N(z_0,\varepsilon)$ 含有 $\sigma$ 的点，则称 $z_0$ 为 $\sigma$ 的界点。

边界：全体界点构成区域的边界。

边界正向：沿着边界走，区域总在左方，就是边界的正方向。简单理解为逆时针就是正方向。

闭区域： $\overline{\sigma}=\sigma+l$ 。

单连通区域：在区域内作任何简单闭曲线，曲线上的点都是属于此区域的。不能包含：“洞”或“点洞”

复连通区域：如果一个区域不是单连通区域，就是复连通区域。

#### 极限和连续性

定义与实变类似，不需要记忆。

> $w=f(z):\forall \varepsilon>0, \exist \delta>0,$ 如果 $0<|z-z_0|<\delta$ 时有 $|f(z)-w_0|<\varepsilon$ ，
> 
> 则 $\lim_{z\to z_0} {f(z)} = w_0$ 为极限

> 若 $\lim_{z\to z_0} {f(z)} = f(z_0)$ ，
> 
> 则 $f(z)$ 在 $z_0$ 点连续。

### 第三节 微商及解析函数

#### 微商

定义与实变类似。

>  $w=f(z)$ 是 $z$ 点及 $N(z,\varepsilon)$ 上的单值函数，
>
> 若 $\displaystyle\lim_{\Delta z \to 0} {\frac{f(z+\Delta z) - f(z)}{\Delta z}}$ 存在极限，
>
> 则记 $\displaystyle f'(z) = \lim_{\Delta z\to 0} {\frac{\Delta f}{\Delta z}}$ ，称为 $f(z)$ 在 $z$ 点的导数。

注意：

* 如果极限没有注明方向， $\displaystyle \lim_{\Delta z\to 0} {\frac{\Delta f}{\Delta z}}$ 是指自变量从任意方向趋于 $0$ ，需要考虑整个平面内的趋近方式。
* 可导必然连续，连续不一定可导，例如 $f(z)=\text{Re}z=x$ 在复平面中处处连续，但处处不可导。

#### 可导的充分必要条件

可导的必要条件：柯西-黎曼条件（C-R条件）

$$
\left\{
\begin{aligned}
    \displaystyle & \frac{\partial {u}}{\partial {x}} = \frac{\partial {v}}{\partial {y}} \\
    \displaystyle & \frac{\partial {v}}{\partial {x}} = - \frac{\partial {u}}{\partial {y}}
\end{aligned}
\right.
$$

可导的充分条件：

同时满足

*  $u_x,u_y,v_x,v_y$ 均连续
*  满足 C-R 条件

#### 解析函数

定义

> 若 $w=f(z)$ 在点 $z_0$ 以及 $N(z_0,\varepsilon)$ 可导，则称 $w=f(z)$ 在 $z_0$ 点解析（Analytic,regular,holomorphic）。
>
> 若 $w=f(z)$ 在区域 $\sigma$ 内处处可导，则称 $w=f(z)$ 在区域 $\sigma$ 解析，记作 $f(z)\in H(\sigma)$ 。

解析是比可导更加严苛的条件。解析必然可导，可导不一定解析。

如果确定谈论的 $\sigma$ 是一个区域，那么可导和解析就完全等价。

不解析的点成为**奇点**。

解析的必要条件、充分条件和可导是一样的。

若 $f(z)=u+\mathrm{i}v \in H(\sigma)$ ，则有如下性质：

* 设 $\displaystyle \Delta = \frac{\partial^2 {}}{\partial {x^2}} + \frac{\partial^2 {}}{\partial {y^2}}$ 为拉普拉斯算子，则有 $\displaystyle \Delta u = 0, \Delta v = 0$ 。
*  $\nabla u \cdot \nabla v = 0$ 
* 已知 $u,v$ 之一均可求出解析函数。
* 解析函数的和差积商仍为解析函数。

### 第四节 初等解析函数

#### 幂函数

* 定义

$$
w=z^n(0,\pm 1,\pm 2,\cdots)
$$

* 解析区域：除 $z=0$ 的复平面
* 满足幂运算 $z^m \cdot z^n = z^{m+n}$ 
* 对于多项式函数 $P(z),Q(z)$ ，有理函数 $\displaystyle w=\frac{P(z)}{Q(z)}$ 除了 $Q(z)=0$ 处外全部解析

#### 指数函数

* 定义

$$
w=\mathrm{e}^z=\mathrm{e}^{x+\mathrm{i}y}=\mathrm{e}^x(\cos y+\mathrm{i}\sin y)
$$

* 解析区域：整个复平面
* 满足实变函数相同的性质
* 新性质
  * 周期性： $\mathrm{e}^{z+\mathrm{i}2k \mathrm{\pi}} = e^z(k=0,\pm1,\pm2,\cdots)$
  *  $\mathrm{e}^z$ 不一定大于 $0$  

#### 三角函数

* 定义

$$
\sin z=\frac{\mathrm{e}^{\mathrm{i}z}-\mathrm{e}^{-\mathrm{i}z}}{2 \mathrm{i}}
$$

$$
\cos z=\frac{\mathrm{e}^{\mathrm{i}z}+\mathrm{e}^{-\mathrm{i}z}}{2}
$$

$$
\tan z=\frac{\sin z}{cos z},\cot z=\frac{\cos z}{\sin z},\sec z=\frac{1}{\cos z}, \csc z=\frac{1}{\sin z}
$$

* 解析区域：复平面（除分母为 $0$ 外）
* 与实函数相同的性质
* 新性质： $|\sin z|,|\cos z|$ 不再有界，而有可能是任何正数

#### 双曲函数

* 定义

$$
\sinh z=\frac{\mathrm{e}^z-\mathrm{e}^{-z}}{2},\cosh z=\frac{\mathrm{e}^z+\mathrm{e}^{-z}}{2}
$$

* 解析区域：复平面
* 与实函数相同的性质： $\cosh^2z-\sinh^2z=1$ ……
* 新性质
  * 初等单值函数与相应实函数的定义在形式上相同
  * 在其解析区域内均解析
  * 满足实函数具有的性质

#### 根式函数

是多值函数

* 定义

    若 $w^n=z$ 则记 $w=\sqrt[n]{z}(n=2,3,\cdots)$ 

* 是多值函数

    为了和单值函数一样研究连续性和解析性，就需要单值化，通过限制定义域，例如限制辐角范围

* 支点：当变量绕支点一周时函数值会改变的点，绕其 $n$ 周以后函数值还原的支点为 $n-1$ 阶支点

    对于 $w=\sqrt{z}$ ，有两个支点，分别为 $z=0,\infty$ ，都是一阶支点

* 单值分支：限制 $z$ 的变化范围得到的若干单值函数
* 支割线：连接支点割开 $z$ 平面的线（当 $z$ 连续变化时，不得跨越支割线，不一定是直线）
* 黎曼面：互相交叠的若干叶 $z$ 平面
* 解析性：每一单值支均解析，分支点以及割线上的点都是奇点（如果某一点不再割线上，就解析）

#### 对数函数

是多值函数

* 定义

    若 $z=\mathrm{e}^w$ ，则 $w=\text{Ln}z$ 

* 多值性的体现： 体现在 $z$ 的辐角与 $w$ 的虚部的对应关系上
* 支点： $0,\infty$ 
* 单值分支： $\text{Ln}z=\ln|z|+\mathrm{i}(\arg z+2k \mathrm{\pi})$  
* 主值支： $\ln z=\ln|z|+\mathrm{i}\arg z$ 
* 黎曼面：无穷多叶
* 解析性：每一单值支均解析
* 性质：和实数的对数运算一样

#### 讨论多值函数支点的方法

对于每个单项式，分解因式。对于每个因式的零点，考虑绕零点几周后才能还原。最后用排列组合的方法得到总的值数。

## 第五节 解析函数的几何性质

复变函数 $w=f(z)$ 给出了从 $z$ 平面上将点集 $E$ 映射到 $w$ 平面上点集 $F$ 的对应关系， $w$ 称为像点， $z$ 称为原像。

### 单叶变换定理

我们需要从变换

$$
w=f(z)=u(x,y)+\mathrm{i}v(x,y)
$$

解出 $x$ 和 $y$ 作为 $u,v$ 的单值函数。允许这样做的条件就是 $w$ 的雅可比行列式不等于 $0$ 

$$
J=
\begin{vmatrix}
    \frac{\partial {u}}{\partial {x}} & \frac{\partial {u}}{\partial {y}} \\
    \frac{\partial {v}}{\partial {x}} & \frac{\partial {v}}{\partial {y}}
\end{vmatrix}
\ne0
$$

具体地，