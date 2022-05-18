---
title: 浅谈C语言中malloc与calloc的区别
abbrlink: 57cfdd39
date: 2021-03-14 09:44:38
tags:
- C
- 指针
---

学习指针过程中，总是看到同样的一个问题，有人用calloc来申请动态数组，有人用malloc来申请动态数组，我上网大概搜了一下，大致意思应该是calloc相当于封装好了malloc和memset，在创建的时候就一并初始化了，随之而来的就是malloc的效率相对要高一些。

  从原型上看，malloc的含义是“给我一个大小为size的连续内存”，而calloc貌似是“给我n个大小为size的内存”。

因为这种原型。有人说（不知道是不是官方也这么说）calloc返回的对象数组而malloc仅仅是一块连续的内存，然而，我翻阅了官方文档，发现calloc申请的动态内存释放时候也是只需要free一次就够了，跟malloc一致，这里强调malloc的这一点却没有说calloc，难道calloc不用记录？那么free是怎样释放用它申请的空间的？calloc究竟是个什么？这必须要看看calloc的源代码。

于是，我在apple找到了calloc的源代码

```c
#include "ansidecl.h"
#include <stddef.h>

/* For systems with larger pointers than ints, this must be declared.  */
PTR malloc (size_t);
void bzero (PTR, size_t);

PTR
calloc (size_t nelem, size_t elsize)
{
  register PTR ptr;  

  if (nelem == 0 || elsize == 0)
    nelem = elsize = 1;
  
  ptr = malloc (nelem * elsize);
  if (ptr) bzero (ptr, nelem * elsize);
  
  return ptr;
}
```

看完的结论就是：那没事了

