title: 删除数字问题的数学证明
tags:
  - 证明
id: 1134
categories:
  - 算法/数据结构
date: 2013-11-18 00:04:49
---

A1...An组成的序列，删除第一个**非递减**数字的**前一个**数字，那么余下的它是一个最大的数。

证明：
设第一个非递减的数字为Ai+1 ，那么前一个数字是Ai，同时有Ai &lt; Ai+1
假设存在其他解....然后要么是**更低位**的数字Aj,要么是**更高位**的 Ak &gt; Ak+1。

对于更低位的数字,
删除Ai， A1  ... Ai-1   <span style="color: #ff0000;"><del>Ai</del></span>   Ai+1  ...  Aj-1  Aj   Aj+1
删除Aj,   A1  ... Ai-1   Ai   Ai+1  ...  Aj-1  <span style="color: #ff0000;"><del>Aj</del></span>   Aj+1

两者比较

A1  ... Ai-1   <span style="color: #ff0000;">Ai+1</span>  ...  Aj-1  Aj   Aj+1
A1  ... Ai-1   <span style="color: #ff0000;">Ai</span>   Ai+1  ...  Aj-1  Aj+1

因为Ai &lt; Ai+1, 所以删除Ai情况组成的数大于删除Ak情况组成的数。

对于更高位的Ak &gt; Ak+1,同理可证。