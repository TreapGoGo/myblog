---
title: 针对TimeLockFi的经济模型设计——金融视角下的贴现交易与累积交易
date: 2024-03-30 20:45:39
tags:
---


# 概述

[zhangsi_si 提出的 TimelockFi ](https://talk.nervos.org/t/rgb-timelockfi/7905) 在我看来是一个非常好的叙事，于是我借用了传统金融学中的一些理论构建了一套更加完善的经济模型，本文介绍了这一模型的理论基础和基本业务逻辑。目前我正在和另一位朋友实现这个项目，制作一个 CKB 上的特殊交易所。

# 贴现与累积

TimelockFi 的概念再此不在赘述，不了解的朋友可以去读 zhangsi_si 的文章。

首先，为什么会产生利息、贴现率、收益率这些概念？[凯恩斯主义认为，人都具有某种“流动性偏好”](https://zh.wikipedia.org/wiki/%E6%B5%81%E5%8A%A8%E6%80%A7%E5%81%8F%E5%A5%BD)。现货或者现金是最具有流动性的资产，每个人都渴望获得更多的现金；与此同时，人们持有存款、股票、期货、房地产等流动性更差的资产，目的就是为了获得比持有现金更大的收益。凯恩斯的理论从现代金融的角度看来是很成问题的，但是如果我们不纠结那些学院派的辩经，使用这种视角理解 TimelockFi 也无可厚非。

金融学上对于一堆“跨期”的现金流，总是倾向于把不同时刻的现金流转化到同一时刻去，这样才方便进行比较。这种转化有两种方式（或者说方向）：

* 贴现：给定未来时刻实现的现金流，计算出等效的当下的现金流
* 累积：给定当下的现金流，计算出未来某时刻实现的等效现金流

例如，你收到一张支票，凭借这张支票，你有权在 1 年后从银行提走 100 万元现金。不过此时你急需用钱，但支票缺乏流动性。为了把支票换成钱，你可以去找银行办理“票据贴现”业务，把支票给银行，让银行立刻给你 97 万元现金。此时的**贴现率**就是 3% 。

再如，你去银行存款，存 100 万元，定期一年，利率为 3% 。对于你来说，这就是一种累积，你把现在的现金流转化为了未来的现金流，牺牲了流动性，但获取了 3% 的收益。

# 年化复利与零息债券

金融学上计算收益率、贴现率等参数时，习惯性使用**年化复利**，因为这样可以让收益计算变得更加平滑连续。**本文中的利率一词，若无特殊说明，都是指年化复利。**

若年化复利为 $r$ ，当前的现金流大小为 $A$ ，那么 $t$ 时间之后的等效现金流为

$$
A_t = A \mathrm{e}^{rt}
$$

称为累积公式

若 $t$ 时间后的现金流大小为 $A_t$ ，那么换算到现在的等效现金流为

$$
A = A_t \mathrm{e}^{-rt}
$$

称为贴现公式

RGB++ 资产的 `timelock` 使其成为了一种具有价值但可能缺乏流动性的资产。若 `timelock==0` ，那么这其实就是现货，是最具有流动性的。 `timelock` 越大，流动性越差，其价格就应该越低。市场上不同的资产持有者有着不同的流动性偏好，因此就会产生不同 `timelock` 之间资产的交易。

实际上，被锁定一定期限的 RGB++ 资产对应的是金融学上的**零息债券**。零息债券是一种特殊的债券，它到没有利息，到期只偿付面值，但是发行的时候是折价发行，到期之间的每一次转手交易也是折价买卖，零息债券的价值会随着时间的推移自动升值。若其他条件不变，`timelock` 越短，资产的价值越高。换句话说，RGB++ 资产的价值也会随着时间的推移逐渐提升。

# 订单簿 or AMM？

其实，我们还没确定我们到底要做 order book 还是 AMM 。[如何用 Cell 模型实现这么复杂的工程，其实前人早有研究](https://medium.com/nervosnetwork/layer-2-dex-designs-on-nervos-ckb-63e11282aaa0)——我们现在站在巨人的肩膀上集中于经济模型的设计。不过我已经为两种模式都设计了经济模型。

在 order book 模式下，针对到期时间相同或相近的锁定代币开设盘口，买方和卖方竞价。合约负责撮合交易请求，未能成交的交易请求将被暂存在 Cell 中。

在 AMM 模式下，各种不同期限的锁定代币（包括已解锁的现货）将被投入一个池子中， LP 可以向池子里投入现货或债券以提供流动性从而赚取手续费，而交易者可以根据自己的需要进行交易操作。

不管采用何种模型，我们都必须解决一个问题：如何定价。

# 定价模型：平均贴现率

我们设一个时间微元 $t$ ，作为不可再分的最小时间单位， $t$ 的长度可以任意给定。

假设目前市场上现货价格为 $p_0$ ，在未来若干时刻 $t,2t,3t,\cdots,nt$ 都有解锁的代币，每一时刻解锁的代币数量为 $x_1,x_2,x_3,\cdots,x_n$ ，他们的价格是 $p_1,p_2,p_3,\cdots,p_n$ ，做成表格如下：

| 时间  | 解锁代币数量 | 价格  |
| :---: | :----------: | :---: |
| $1t$  |    $x_1$     | $p_1$ |
| $2t$  |    $x_2$     | $p_2$ |
| $3t$  |    $x_3$     | $p_3$ |
|  ...  |     ...      |  ...  |
| $nt$  |    $x_n$     | $p_n$ |

显然有 $p_0 > p_1 > p_2 > \cdots > p_n$ ，因为远期资产的流动性最差，价格最低。

从 $0\sim t$ 时刻，设贴现率为 $r_1$ ，有 $p_0 = p_1\mathrm{e}^{r_1 t}$ ，得 $\displaystyle r_1 = \frac{1}{t} \ln \frac{p_0}{p_1}$ ；

从 $0\sim 2t$ 时刻，设贴现率为 $r_2$ ，有 $p_0 = p_2\mathrm{e}^{r_2 t}$ ，得 $\displaystyle r_2 = \frac{1}{2t} \ln \frac{p_0}{p_2}$ ；

从 $0\sim nt$ 时刻，设贴现率为 $r_n$ ，有 $p_0 = p_n\mathrm{e}^{r_n t}$ ，得 $\displaystyle r_n = \frac{1}{nt} \ln \frac{p_0}{p_n}$ ；

完美市场条件下，理论上有 $r_1 = r_2 = \cdots = r_n$ ，即贴现率应该和期限长短无关（称之为收益率期限结构是平的）。实际上，大多数时候这些贴现率都不是相等的。

确定一个**平均贴现率** $\overline{r}$ 意义重大，我简要概括为以下三点：

* 其一，平均贴现率对于不同期限资产的互相交易是非常必要的，因为这样才能保证交易的公平性
* 其二，设立一个机制促进不同期限的贴现率趋于一致，有助于促进协议经济模型的稳定性，减少滑点和无常损失
* 其三，如果这些贴现率差距过大，实际上是存在套利空间的，只需要卖出贴现率高的代币、买入贴现率低的代币，就可以赚取无风险收益

如何确定 $\overline{r}$ ，这个问题仁者见仁智者见智。有些人可能会求算数平均数，有些人会求几何平均数，还有些人可能会以数量为权重求加权平均数。在我看来，这些参数非常简便易用，而且彼此之间相差不大。然而，他们的构造是基于直觉，而不是基于专业的金融视角。

金融学上的**一价定律**表明，如果两个金融资产现金流特征相同，那么他们就是等价资产，具有相同的价格。现金流特征包括三个方面：现值、终值和收益率。

我选择将市场上所有未到期资产的终值以 $\overline{r}$ 贴现，结果应当等于其现值。注意这里的 $\overline{r}$ 是方程中的待解的未知数。

$$
x_1p_1\mathrm{e}^{\overline{r}\cdot t} + x_2p_2\mathrm{e}^{\overline{r}\cdot 2t} + x_3p_3\mathrm{e}^{\overline{r}\cdot 3t} + \cdots + x_np_n\mathrm{e}^{\overline{r}\cdot nt} = (x_1+x_2+\cdots+x_n)p_0
$$

这个式子看上去有些恐怖，我们换元，令 $a_i=x_ip_i, y_i=\mathrm{e}^{\overline{r}it}, b=(x_1+x_2+\cdots+x_n)p_0$ ，化为

$$
a_1 y + a_2 y^2 + a_3 y^3 + \cdots + a_n y^n = b
$$

可以看出，这是一个关于 $y$ 的高次多项式方程。如果 $n \ge 5$ ，方程是没有解析解的。我们固然可以用数值方法（如牛顿迭代法）求出一个数值解，但这样就把问题搞得太复杂了。因此我们需要转换思路，寻找一个近似解。

如果你学过高中数学或者微积分就会知道，当 $x \to 0$ 时， $\mathrm{e}^x \sim x+1$ ，他们是可以互相代换的量。我们方程中的指数函数能否利用等价无穷小替换化简呢？

在我们的经济模型中， $n$ 是一个很大的数，但再大也是有限大，因为“在无穷久的时间后解锁的代币”本质就是一个空头支票罢了，根本没法兑现，所以完全没有交易价值。然而，时间微元 $t$ 是可以趋于 $0$ 的。同时 $\overline{r}$ 是一个有限大的数。综上，指数 $\overline{r}nt$ 可以趋于 $0$ 。

故有 $t \to 0$ 时， $\mathrm{e}^{\overline{r}it} \sim \overline{r}it+1$ ，方程化为

$$
x_1p_1(\overline{r}\cdot t+1) + x_2p_2(\overline{r}\cdot 2t+1) + x_3p_3(\overline{r}\cdot 3t+1) + \cdots + x_np_n(\overline{r}\cdot nt+1) = (x_1+x_2+\cdots+x_n)p_0
$$

这是一个关于 $\overline{r}$ 的一次方程，解得

$$
\begin{aligned}
    \overline{r}
    & = \frac{x_1(p_0-p_1) + x_2(p_0-p_2) + \cdots + x_n(p_0-p_n)}{x_1p_1t + 2x_2p_2t + \cdots + nx_np_nt} \\
    & = \frac{\displaystyle\sum_{i=1}^n x_i(p_0-p_i)}{\displaystyle\sum_{i=1}^n ix_ip_it} 
\end{aligned}
$$

这就是最理想的平均贴现率。它十分贴近方程的真实数值解，而且看起来比较简洁，计算它所需的性能开销并不大。

补充说明：可能有人会质疑，当 $t\sim0$ 时， $\overline{r}$ 的分母为零，贴现率变成了无穷大。我对此的解释是，当时间微元变得越来越小时，为了保持原来的最大交易周期 $nt$ 不变， $n$ 也会相应地进行一定调整，分母的项数增多，以至于虽然每一项都趋于 $0$ ，求和后也不是 $0$ 。

# 交易举例：贴现、累积和掉期

假如你有一笔未解锁的 RGB++ 资产，将在 $qt(q\in\N)$ 时刻解锁，面值为 $x_q$ 。可是你觉得这个币很快就要归零了，想赶紧换成已解锁的现货卖掉。那么你就需要进行贴现。如果目前市场上的平均贴现率为 $\overline{r}$ ，你能够贴现得到的现货数额就是 $x_0 = x_q \mathrm{e}^{-\overline{r}qt}$ 。显然有 $x_0 < x_q$ ，贴现后你掌握的代币数量减少了。

假如你目前有 $x_0$ 个已解锁的现货资产，你觉得这个代币未来会涨而且现在你用不上，同时又希望获取更大的收益，你可以将这些现货换为 $wt(w\in\N)$ 时刻解锁的资产。如果交易是公平的，你将得到的 $wt(w\in\N)$ 时刻解锁的资产的数量为 $x_w = x_0 \mathrm{e}^{\overline{r}wt}$ 。显然有 $x_w > x_0$ ，累积后你掌握的代币数量增加了。这其实是一种投资，多出来的代币就是你的投资收益。

上面的交易其实可以综合起来，你可以将 $qt$ 时刻解锁的资产调换为 $wt$ 时刻解锁的资产。如果 $w>q$ ，你延长了锁定期限，并得到更多代币；如果 $w<q$ ，你缩短了锁定期限，并得到更少代币。

# 工程实现的相关细节和待解决的问题

无论是采用 order book 还是采用 AMM ，上述定价模型都是十分有价值的。

如果我们使用 order book 实现，就需要考虑“盘口到底开在时间轴上的什么位置”这个问题。

* 关于盘口状态：一种是静态盘口，选定若干固定时刻，例如北京时间每天早上八点，将解锁时间在此附近的代币放在此盘口交易；一种是动态盘口，选择距今的时间段，例如一天期、两天期、一周期等。
  * 静态盘口可能会导致挂单的限价随着时间的推移而失真，交易者不得不经常调整限价；动态盘口需要处理好挂单的自动调期问题，例如原本解锁距今两天的代币一直没成交，就得在一天后调到一天期盘口上。
* 关于盘口间隔：盘口间隔过大会导致价格失真，例如两个代币可能在同一天解锁，可是一个是在凌晨 1:00 解锁，另一个是在 23:00 解锁，他们的价格会有很大差异；盘口间隔过小会导致流动性稀缺，成交变得困难。
* 关于误差处理：对于每一个区块都建立一个盘口是不现实的，必须把一个未解锁资产放到距离较近的盘口上去，并根据平均贴现率 $\overline{r}$ 进行一定的误差修正（这种修正可以在前端完成）。

如果我们使用 AMM 实现，就需要考虑流动性问题。

* 流动性激励：LP 可以向流动性池中投入现货或任意时刻解锁的资产，但时间轴上不同时刻的流动性丰裕程度不同，流动性较为稀缺的时间点附近需要给出更高的收益，鼓励 LP 向这些时间点提供流动性。
* 交易费计算：流动性较为稀缺的时间点附近，需要提高交易的费用，补偿给 LP ，同时抑制交易者对某一时间点解锁的资产的狂热情绪，避免出现流动性枯竭。
* 误差修正：如果交易者想要买的是 $wt$ 时刻解锁的资产，有可能流动性池中并没有足够的资产卖给交易者，这时候就需要调动 $wt$ 时刻附近解锁的资产，并进行一定的误差修正与配平。

# 总结

TimelockFi 确实是一个很好的叙事。不过我刚从 ETH 开发转到 CKB 上来，两边思维方式不太一样，初学起来很痛苦，生态开发相关的基础设施也不太完善。还希望 CKB 社区的前辈们多多提携。