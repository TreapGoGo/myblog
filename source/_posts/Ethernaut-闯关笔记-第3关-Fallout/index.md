---
title: Ethernaut 闯关笔记 第3关 Fallout
date: 2023-12-07 11:45:12
tags:
---


# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import 'openzeppelin-contracts-06/math/SafeMath.sol';

contract Fallout {
  
  using SafeMath for uint256;
  mapping (address => uint) allocations;
  address payable public owner;


  /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }

  modifier onlyOwner {
	        require(
	            msg.sender == owner,
	            "caller is not the owner"
	        );
	        _;
	    }

  function allocate() public payable {
    allocations[msg.sender] = allocations[msg.sender].add(msg.value);
  }

  function sendAllocation(address payable allocator) public {
    require(allocations[allocator] > 0);
    allocator.transfer(allocations[allocator]);
  }

  function collectAllocations() public onlyOwner {
    msg.sender.transfer(address(this).balance);
  }

  function allocatorBalance(address allocator) public view returns (uint) {
    return allocations[allocator];
  }
}
```

要求：成为合约主人

# 攻击思路

在 Solidity 语言中，一个合约的构造函数一般用 `constructor()` 关键字定义。

在这个合约中，编写者尝试像其他语言那样，用一个与合约名称相同的函数作为构造函数。

然而，在定义构造函数时，他将合约名 `Fallout` 误写为 `Fal1out` ，这导致这个函数成为了一个普通的 public 函数。

任何人都可以在合约创建后调用这个函数，成为合约主人。

# 攻击代码

非常简单，只有一行

```solidity
await contract.Fal1out()
```

# 安全启示

1. 定义构造函数时，建议使用 `constructor()` 关键字，避免使用合约名
2. 涉及变更 `owner` 的函数，不应使用 `public` ，且函数名不要写错