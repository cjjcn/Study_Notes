---
title: PWN学习-保护概述和溢出实例
tags:
  - CTF
  - pwn
abbrlink: cd4fd3e2
date: 2021-07-20 23:20:36
---

## 0x00 检查保护情况

在编写漏洞利用代码的时候，需要特别注意目标进程是否开启了DEP(Linux下对应NX)、ASLR(Linux下对应PIE)等机制，例如存在DEP(NX)的话就不能直接执行堆栈上的数据，存在ASLR的话各个系统调用的地址就是随机化的

```bash
checksec read
```

### ①CANNARY(栈保护)

这个选项表示栈保护功能有没有开启。

栈溢出保护是一种缓冲区溢出攻击缓解手段，当函数存在缓冲区溢出攻击漏洞时，攻击者可以覆盖栈上的返回地址来让shellcode能够得到执行。当启用栈保护后，函数开始执行的时候会先往栈里插入cookie信息，当函数真正返回的时候会验证cookie信息是否合法，如果不合法就停止程序运行。攻击者在覆盖返回地址的时候往往也会将cookie信息给覆盖掉，导致栈保护检查失败而阻止shellcode的执行。在Linux中我们将cookie信息称为canary。

gcc在4.2版本中添加了-fstack-protector和-fstack-protector-all编译参数以支持栈保护功能，4.9新增了-fstack-protector-strong编译参数让保护的范围更广。

因此在编译时可以控制是否开启栈保护以及程度，例如：
```
gcc -fno-stack-protector -o test test.c //禁用栈保护

gcc -fstack-protector -o test test.c //启用堆栈保护，不过只为局部变量中含有 char 数组的函数插入保护代码

gcc -fstack-protector-all -o test test.c //启用堆栈保护，为所有函数插入保护代码
```
### ②FORTIFY

这个保护机制查了很久都没有个很好的汉语形容，根据我的理解它其实和栈保护都是gcc的新的为了增强保护的一种机制，防止缓冲区溢出攻击。由于并不是太常见，也没有太多的了解。

举个例子可能简单明了一些：

一段简单的存在缓冲区溢出的C代码



```
void fun(char *s) {
 
        char buf[0x100];
 
        strcpy(buf, s);
 
        /* Don't allow gcc to optimise away the buf */
 
        asm volatile("" :: "m" (buf));
        }
```



用包含参数-U_FORTIFY_SOURCE编译



```
08048450 <fun>:
 
  push   %ebp               ;
 
  mov    %esp,%ebp
 
  sub    $0x118,%esp        ; 将0x118存储到栈上
 
  mov    0x8(%ebp),%eax     ; 将目标参数载入eax
 
  mov    %eax,0x4(%esp)     ; 保存目标参数
 
  lea    -0x108(%ebp),%eax  ; 数组buf
 
  mov    %eax,(%esp)        ; 保存
 
  call   8048320 <strcpy@plt>
 
  leave                     ;
 
  ret
```



用包含参数-D_FORTIFY_SOURCE=2编译



```
08048470 :
 
  push  %ebp              ;
 
  mov    %esp,%ebp
 
  sub    $0x118,%esp        ;
 
  movl  $0x100,0x8(%esp)  ; 把0x100当作目标参数保存
 
  mov    0x8(%ebp),%eax    ;
 
  mov    %eax,0x4(%esp)    ;
 
  lea    -0x108(%ebp),%eax  ;
 
  mov    %eax,(%esp)        ;
 
  call  8048370 <__strcpy_chk@plt>
 
  leave                      ;
 
  ret
```



我们可以看到gcc生成了一些附加代码，通过对数组大小的判断替换strcpy, memcpy, memset等函数名，达到防止缓冲区溢出的作用。

### ③NX（DEP）

NX即No-eXecute（不可执行）的意思，NX（DEP）的基本原理是将数据所在内存页标识为不可执行，当程序溢出成功转入shellcode时，程序会尝试在数据页面上执行指令，此时CPU就会抛出异常，而不是去执行恶意指令。

gcc编译器默认开启了NX选项，如果需要关闭NX选项，可以给gcc编译器添加-z execstack参数。

例如：

gcc -z execstack -o test test.c

在Windows下，类似的概念为DEP（数据执行保护），在最新版的Visual Studio中默认开启了DEP编译选项。

### ④PIE（ASLR）

一般情况下NX（Windows平台上称其为DEP）和地址空间分布随机化（ASLR）会同时工作。

内存地址随机化机制（address space layout randomization)，有以下三种情况

0 - 表示关闭进程地址空间随机化。

1 - 表示将mmap的基址，stack和vdso页面随机化。

2 - 表示在1的基础上增加栈（heap）的随机化。

可以防范基于Ret2libc方式的针对DEP的攻击。ASLR和DEP配合使用，能有效阻止攻击者在堆栈上运行恶意代码。

Built as PIE：位置独立的可执行区域（position-independent executables）。这样使得在利用缓冲溢出和移动操作系统中存在的其他内存崩溃缺陷时采用面向返回的编程（return-oriented programming）方法变得难得多。

liunx下关闭PIE的命令如下：

sudo -s echo 0 > /proc/sys/kernel/randomize_va_space

PIE有关的知识详情：https://blog.csdn.net/counsellor/article/details/81543197

### ⑤RELRO

设置符号重定向表格为只读或在程序启动时就解析并绑定所有动态符号，从而减少对GOT（Global Offset Table）攻击。RELRO为” Partial RELRO”，说明我们对GOT表具有写权限。

总之，需要执行

sudo -s echo 0 > /proc/sys/kernel/randomize_va_space//可选，如果要关闭系统的ALSR功能就执行这个

gcc -no-pie -fno-stack-protector -z execstack -m32 -o read read.c

## 0x01 查看程序使用了哪些函数

`objdump`是用来查看目标文件或可执行的目标文件的构成的gcc工具。

```
-j name
--section=name //仅仅显示指定名称为name的section的信息
-t
--syms 显示文件的符号表入口。类似于nm -s提供的信息
```

### ***example***

```
objdump -t -j .text read//查看read程序的.text段有哪些函数
```

### 常用的bash指令

```
mkdir test//创建目录命令

touch test.txt //创建文件（注：Linux下没有文件后缀名区分文件类型之说，系统文件类型只有可执行文件和不可执行文件）

rm -rf /var/log/httpd/access//删除文件夹

rm -f fileName//删除文件
```

## 0x02 计算返回值偏移

## 0x03 查看exploit的地址

```
send(data): 发送数据

sendline(data) : 发送一行数据，相当于在末尾加n      

recv(numb=4096, timeout=default) : 给出接收字节数,timeout指定超时

recvuntil(delims, drop=False) : 接收到delims的pattern

（以下可以看作until的特例）

recvline(keepends=True) : 接收到n，keepends指定保留n

recvall() : 接收到EOF

recvrepeat(timeout=default) : 接收到EOF或timeout

interactive() : 与shell交互
```

## 0x04 编写exp
