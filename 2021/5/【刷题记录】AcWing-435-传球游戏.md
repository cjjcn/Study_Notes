---
title: 【刷题记录】AcWing 435. 传球游戏
tags:
  - leetcode
  - acwing
  - 算法
  - DP
abbrlink: 832f70bb
date: 2021-05-08 19:54:25
---

## [AcWing 435. 传球游戏](https://www.acwing.com/problem/content/437/)

#### 思路

一道简单的DP推导题目，推导过程中注意编号是循环的就好，每个人有两种状态，一种是从左边接到一种是从右边接到

#### C++代码

```c++
#include<iostream>

using namespace std;

const int N=35;

int n,m;
int f[N][N];

int main()
{
    cin>>n>>m;
    f[0][0]=1;
    for(int i=1;i<=m;i++)
        for(int j=0;j<n;j++)
            f[i][j]=f[i-1][(j-1+n)%n]+f[i-1][(j+1)%n];
    cout<<f[m][0]<<endl;
    return 0;
}
```

## [LeetCode 7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

#### 思路

依次从右往左计算出每位数字，然后逆序累加在一个整数中。
另外，这题有两点需要注意：

因为int型整数逆序后可能会溢出，所以我们要用long long记录中间结果；在C++中，负数的取模运算和数学意义上的取模运算不同，结果还是负数，比如 −12%10=−2，所以我们不需要对负数进行额外处理。

时间复杂度分析：一共有 O(logn)
位，对于每一位的计算量是常数级的，所以总时间复杂度是 O(logn).

#### C++代码

```c++
class Solution {
public:
    int reverse(int x) {
        int res=0;
        while(x)
        {
            if(x>0 && res>(INT_MAX - x % 10)/10)    return 0;
            if(x<0 && res<(INT_MIN -x %10)/10)  return 0;
            res=res*10+x%10;
            x/=10;
        }
        return res;
    }
};
```

## [AcWing 78.左旋转字符串](https://www.acwing.com/problem/content/74/)

#### 思路

见代码

#### C++代码

```c++
class Solution {
public:
    string leftRotateString(string str, int n) {
        return str.substr(n)+str.substr(0,n);
    }
};
```