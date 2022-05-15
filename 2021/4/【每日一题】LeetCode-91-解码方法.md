---
title: 【每日一题】LeetCode 91.解码方法
tags:
  - leetcode
  - acwing
  - 算法
  - 每日一题
abbrlink: d73dd87b
date: 2021-04-21 20:22:22
---

## 断更说明

前两天停止更新了两天，不是因为我放弃挣扎了，而是后面几天的每日一题都被预判了。。。在之前的文章（推送）里面都可以找到，今天题目恢复正常，每日一题也开始正常更新

之后可能会把几天的每日一题汇总起来一次发推送，提高每篇文章的丰富度，如果有任何好的建议，欢迎给我留言。

## [LeetCode 91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

#### 思路

本题的大致意思是要将一段编码后的字符串解码，而按照对应的转换关系，编码后的数字会有很多种不同的划分方式对应着不同的解码方式，这里就需要用到动规的思想，先判断当前如果是1-9即可算一种排列方式，接着再判断当前数和前一个数字组合以后能够组成一个10-26之间的数（与字母对应）即可认为也存在一种情况，直接加上这种情况即可。

#### C++代码

```c++
class Solution {
public:
    int numDecodings(string s) {
        int n=s.size();
        s=' '+s;
        vector<int> f(n+1);
        f[0]=1;
        for(int i=1;i<=n;i++)
        {
            if(s[i]>='1' && s[i]<='9')  f[i]+=f[i-1];
            if(i>1)
            {
                int t=(s[i-1]-'0') * 10 + s[i] - '0';
                if(t>=10 && t<=26)
                    f[i]+=f[i-2];
            }
        }
        return f[n];
    }
};
```

## [AcWing 821.跳台阶](https://www.acwing.com/problem/content/823/)

#### 思路

一道基本的动规题目，斐波那契的应用

#### C++代码

```c++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=20;

int n;
int f[N];

int main()
{
    cin>>n;
    f[0]=1, f[1]=1;
    for(int i=2;i<=n;i++)
        f[i]=f[i-1]+f[i-2];
    cout<<f[n]<<endl;
    return 0;
}
```

