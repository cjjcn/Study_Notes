---
title: 【AcWing】算法学习-二进制-更新中
tags:
  - acwing
  - 二进制
abbrlink: e7fde3bf
date: 2021-04-11 14:29:15
---

## 二进制的笔记

n的二进制表示中第k位是几，例如

>n=15=(1111)2

#### 思路

1. 先把第k位移到最后一位(n>>k)
2. 观察一下个位是几(x&1)

#### 公式

`n>>k&1`

#### 函数

`lowbit(x)`返回x的最后一位1

```
x=1010 lowbit(x)=10
x=101000 lowbit(x)=1000
```

**实现原理**:x&-x(=x&(~x+1))

## 例题：[AcWing 801.二进制中1的个数](https://www.acwing.com/problem/content/803/)

```c++
#include<iostream>

using namespace std;

int lowbit(int x)
{
    return x & -x;
}

int main()
{
    int n;
    cin>>n;
    while(n--)
    {
        int x;
        cin>>x;
        int res=0;
        while(x)    x-=lowbit(x), res++;
        cout<<res<<' ';
    }
    return 0;
}
```

