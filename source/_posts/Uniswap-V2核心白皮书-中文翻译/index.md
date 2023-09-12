---
title: Uniswap V2核心白皮书 中文翻译
date: 2023-09-12 16:53:42
tags:
---

# 译者注

本文是对《Uniswap v2 Core》的中文翻译，原文的版权属于原作者或原著作权持有人。本翻译文档受到 MIT 许可协议的保护。请在使用本文档之前仔细阅读并遵守 MIT 许可协议的规定。

原文链接：https://docs.uniswap.org/whitepaper.pdf

<center><h1>Uniswap V2 核心</h1></center>

作者：

Hayden Adams - hayden@uniswap.org

Noah Zinsmeister - noah@uniswap.org

Dan Robinson - dan@paradigm.xyz

2020年3月

# 摘要

这份技术白皮书解释了 Uniswap V2 核心合约背后的若干设计决策。其内容涵盖了合约的一些新特性，其中包括

* 任意 ERC20 代币的交易币对；
* 一个经过加强的预言机：这个预言机允许其他合约预估某个给定的时间范围内的时间加权均价；
* “闪电交换”：这种交换方式允许交易者先收到资产并在其他地方使用资产，然后再在交易的最后为这些资产支付费用；
* 一个可以在未来开启的协议费。

这份白皮书描述了 Uniswap V2 的“核心”合约的机制，