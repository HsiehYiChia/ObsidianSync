---
base: "[[閱讀清單.base]]"
Status: Done
Date: 2020-07-01
Category: For Work
---
### A thorough guide to the design and implementation of the Linux kernel







#  Chapter 7 Interrupts and Interrupt Handlers

1. 有些中斷會被分為top/bottom halves兩部分, top halves速度要很快只做必要的事情, 通常是回應發出中斷的device說:"我收到中斷了, `你可以繼`續了", bottom halves則把剩下的工作處理完成
**例子: **網卡發中斷給CPU, CPU將封包從網卡的記憶體搬到主記憶體(這就是top halves), 其他工作才在bottom halves完成
2. 

![[20200702_103148.jpg]]
