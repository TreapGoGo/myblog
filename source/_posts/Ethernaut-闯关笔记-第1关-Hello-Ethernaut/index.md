---
title: Ethernaut 闯关笔记 第1关 Hello Ethernaut
date: 2023-11-16 10:28:45
tags:
---

点击 get new instance ，按 F12 键启动审查元素，点击“Console”，在其中按照题目要求输入指令，逐步解决谜题

```javascript
player
'0x15AfABaA426334636008Bc15805760716E8b5c5E'

getBalance(player)
Promise {<pending>}
    [[Prototype]]: Promise
    [[PromiseState]]: "fulfilled"
    [[PromiseResult]]: "6.126630930601673173"

ethernaut
n {methods: {…}, abi: Array(13), address: '0xa3e7317E591D5A0F1c605be1b3aC4D2ae56104d6', transactionHash: undefined, constructor: ƒ, …}

await contract.info()
'You will find what you need in info1().'

await contract.info1()
'Try info2(), but with "hello" as a parameter.'

await contract.info2("hello")
'The property infoNum holds the number of the next info method to call.'

await contract.infoNum()
42

await contract.info42()
'theMethodName is the name of the next method.'

await contract.theMethodName()
'The method name is method7123949.'

await contract.method7123949()
'If you know the password, submit it to authenticate().'

await contract.password()
'ethernaut0'

await contract.authenticate("ethernaut0")
```

最后点击 submit instance 即可通关。