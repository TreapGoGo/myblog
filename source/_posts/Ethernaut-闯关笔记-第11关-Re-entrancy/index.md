---
title: Ethernaut 闯关笔记 第11关 Re-entrancy
date: 2023-12-13 18:59:32
tags:
---

# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;

import 'openzeppelin-contracts-06/math/SafeMath.sol';

contract Reentrance {
  
    using SafeMath for uint256;
    mapping(address => uint) public balances;

    function donate(address _to) public payable {
        balances[_to] = balances[_to].add(msg.value);
    }

    function balanceOf(address _who) public view returns (uint balance) {
        return balances[_who];
    }

    function withdraw(uint _amount) public {
        if(balances[msg.sender] >= _amount) {
        (bool result,) = msg.sender.call{value:_amount}("");
        if(result) {
            _amount;
        }
        balances[msg.sender] -= _amount;
        }
    }

    receive() external payable {}
}
```

# 攻击思路

很经典的重入攻击。

重入攻击的基本思路就是，合约向另一个合约发送代币时，如果触发了 `fallback` 函数，而 `fallback` 函数又可以在目标合约更新余额之前再次调用目标合约