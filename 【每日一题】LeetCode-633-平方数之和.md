---
title: 【每日一题】LeetCode 633. 平方数之和
tags:
  - 每日一题
  - leetcode
  - acwing
abbrlink: 3d4cbd1d
date: 2021-04-29 18:26:08
---

## [LeetCode 633.平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)
#### 思路
优化掉一层循环即可
#### C++代码
```c
class Solution {
public:
    bool judgeSquareSum(int c) {
        for(int a=0; (long long)a*a<=c;a++)
        {
            int t=c-a*a;
            int r=sqrt(t);
            if(r*r==t)  
                return true;
        }
        return false;
    }
};
```
## [AcWing 1221.四平方和](https://www.acwing.com/problem/content/1223/)
#### 思路
哈希+二分，预处理以后按照条件输出即可，这里也需要优化掉两层循环
#### C++代码
```c
#include <iostream>
#include <cstring>
#include <algorithm>


using namespace std;


const int N = 5000010;

int n;
int C[N], D[N];

int main()
{
    scanf("%d", &n);
    memset(C, -1, sizeof(C));
    
    for(int c=0;c*c<=n;c++)
        for(int d=c;d*d+c*c<=n;d++)
        {
            int s=c*c+d*d;
            if(C[s]==-1)
                C[s]=c, D[s]=d;
        }
        
    for(int a=0;a*a<=n;a++)
        for(int b=a; a*a+b*b<=n;b++)
        {
            int s=n-a*a-b*b;
            if(C[s]!=-1)
            {
                printf("%d %d %d %d\n", a, b, C[s], D[s]);
                return 0;
            }
        }
    return 0;
}
```