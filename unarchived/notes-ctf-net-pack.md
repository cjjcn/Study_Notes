---
title: notes-ctf-net-pack(WinShark)
tags: CTF
abbrlink: 89cfb270
date: 2020-10-12 11:49:18
---
# 题目：shark on wire 1 
```
Description
We found this packet capture. Recover the flag.
```
<!--more-->
# 思路
这里懂得了winshark的一些基本用法（感谢wbl学长），例如常见的CTF的杂项的流量包分析，主要内容集中在TCP中进行TCP追踪流（stream）进行分析，bing查找之后发现了另一种常见的流量包拦截嗅探后寻找admin账号密码以获得flag的方法主要在*post*请求中查找未解密内容，也在这里一并记录。

然而，本题的流分析却在以上二者中都没有找到，回过头来返回udp包中进行追踪，在流中发现了fake flag和flag，本题得以解决

# 总结
之后有必要对TCP/IP协议和UDP，post请求等基本知识熟练掌握，了解加密解密和SSL（学长说的深一层的难度）