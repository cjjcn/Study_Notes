---
title: CTF-RE-学习记录-汇编-2
tags:
  - CTF
  - RE
abbrlink: 134388c0
date: 2020-12-10 19:24:17
---

# 汇编工具DTDebug

下载后设置好odd与插件路径，同时在属性中设置为管理员身份运行（无Administrator权限进入的时候一直在提示）

# 寄存器

存储数据：

CPU>内存>硬盘

32位CPU: 8 16 32

64位CPU: 8 16 32 64

EIP:存储CPU下一次执行的指令集，不能作他用

## 通用寄存器

### 32位寄存器

| 32位 | 16位 | 8位         |
| ---- | ---- | ----------- |
| WAX  | AX   | AL（低8位） |
| ECX  | CX   | CL          |
| EDX  | DX   | DL          |
| EBX  | BX   | BL          |
| ESP  | SP   | AH（高8位） |
| EBP  | BP   | CH          |
| ESI  | SI   | DH          |
| EDI  | DI   | BH          |

8位，16位寄存器同时存在于32位之中，8位指令改32位中的1位，16位改4位

## MOV指令

1. 立即数到寄存器

2. 寄存器到寄存器

3. 寄存器到内存

4. 内存到寄存器

   ```
   MOV EAX,1//把1存储到EAX寄存器
   MOV EDX,EAX//把EAX里面的值存到EDX
   ```

## 内存相关

```
mov byte ptr ds[存储地址]
```

已经申请过的地址，在堆栈窗口内找，可以直接使用

> 一个字节-Byte
>
> 两个字节-WORD
>
> 四个字节-DWORD

```
MOV DWORD PTR DS:[],EAX//前提需要保证宽度相同
```

## 内存地址的5种形式

1. [立即数]

   读取内存的值:

   ```
   MOV EAX,DWORD PTR DS:[0x13FFC4]
   ```

   向内存写入数据

   ```
   MOV DWORD PTR DS:[0x13FFC4],EAX
   ```

2. [reg]  ___reg代表寄存器 可以是8个通用寄存器中的一个___

   读取内存的值：

   ```
   MOV ECX,0x13FFD0
   MOV EAX,DWORD PTR DS:[ECX]
   ```

   向内存中写入数据：

   ```
   MOV EDX,0x13FFD8
   MOV DWORD PTR DS:[EDX],0x87654321
   ```

3. [reg+立即数]

   读取内存的值：

   ```
   MOV ECX,0x13FFD0
   MOV EAX,DWORD PTR DS:[ECX+4]
   ```

   向内存中写入数据：

   ```
   MOV EDX,0x13FFD8
   MOV DWORD PTR DS:[EDX+0xC],0x87654321
   ```

4. [reg+reg*{1,2,4,8}] (数组的汇编形式)

   代表八个32位的寄存器加上八个32位寄存器乘以1，2，4，8

   读取内存的值：

   ```
   MOV EAX,13FFC4
   MOV ECX,2
   MOV EDX,DWORD PTR DS:[EAX+ECX*4]
   ```

   向内存中写入数据:

   ```
   MOV EAX,13FFC4
   MOV ECX,2
   MOV DWORD PTR DS:[EAX+EAX*4],87654321
   ```

5. [reg+reg*{1,2,4,8}+立即数]

   读取内存的值:

   ```
   MOV EAX,13FFC4
   
   MOV ECX,2
   
   MOV EDX,DWORD PTR DS:[EAX+ECX*4+4]
   ```

   向内存中写入数据：

   ```
   MOV EAX,13FFC4
   MOV ECX,2
   MOV DWORD PTR DS:[EAX+ECX*4+4],87654321
   ```

   

# 数据的存储模式

大端模式：数据高位在低位，数据低位在高位

小端模式：数据低位在低位，数据高位在高位

# MOVS指令

BYTE/WORD/DWORD分别简写为MOVSB MOVSW MOVSD

```
MOVS BYTE PTR ES:[EDI],BYTE PTR DS:[ESI]
```

DF位置：每次MOVS指令之后地址减少相应变更（加减具体看DF的值）字节的地址

DF的值表示方向位：0为+，1为-

# STOS指令：将AL/AX/EAX的值存储到[EDI]的指定存储单元

STOS BYTE PTR ES:[EDI]

# REP指令

MAV ECX,10

REP MOVESD//ECX作为REP指令的计数器

EAX通常用作返回值

ESP,EBP通常用作堆栈

ESI,EDI通常用作串复制的首地址和目标地址

# 堆栈相关指令

堆栈的使用是从大地址向小地址使用

ESP 栈指针寄存器

## 堆栈的使用

堆栈使用之后要改变ESP指向的位置，防止被覆盖

例如：

```
SUB ESP,8
```

使用之后取消：

```
ADD ESP,8
```

## 心得

C语言当中局部变量为何赋初始值的原因也就昭然若揭了，因为在内存中ESP寄存器只是指向了未被使用的堆栈，堆栈里面的内容会被覆盖，但是不会被清空，因此创建变量以后他的初始值是未知的，因此创建局部变量需要赋初值

## PUSH&POP指令

功能：

1. 将数压入栈顶
2. 栈顶指针+1

```
PUSH DWORD PTR DS:[18FFA4]
POP EAX
POP ECX
```

# 修改EIP的指令

## KMP指令

MOV EIP简写为JMP

## CALL指令

