---
title: 【每日一题】LeetCode 263.丑数
tags:
  - leetcode
  - acwing
  - 每日一题
abbrlink: 72212b99
date: 2021-04-10 15:42:50
---

## [LeetCode 263.丑数](https://leetcode-cn.com/problems/ugly-number/)

#### 思路

模拟题，根据题目条件写判断即可

#### C++代码

```c++
class Solution {
public:
    bool isUgly(int n) {
        if(n<=0)    return false;
        while(n%2==0)   n/=2;
        while(n%3==0)   n/=3;
        while(n%5==0)   n/=5;
        if(n==1)    return true;
        else
            return false;
    }
};
```

## [AcWing 62.丑数](https://www.acwing.com/problem/content/58/)

#### 思路

这里要求第n个丑数，对于效率的要求就更高了，但是思路依然是比较清晰，就是在数组里面把2，3，5从小到大的数用集合存起来，然后根据性质还有升序的规则一个一个的算出来即可（三路归并）但是这三路里面是有重合的，所以在写的时候还要考虑一下去重的问题，最后得到的这个总的集合就是所有只包含2，3，5为质因子

#### C++代码

```c++
class Solution {
public:
    typedef long long ll;
    int getUglyNumber(int n) {
        if(n<2)    return n;
        vector<int> f(1,1);
        int i=0,j=0,k=0;
        ll t=0;
        while(--n)
        {
            t=min(f[i]*2, min(f[j]*3,  f[k]*5));
            if(t==f[i]*2)   i++;
            if(t==f[j]*3)   j++;
            if(t==f[k]*5)   k++;
            f.push_back(t);
        }
        return f.back();
    }
};
```



