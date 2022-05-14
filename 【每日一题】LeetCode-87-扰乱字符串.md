---
title: 【每日一题】LeetCode 87.扰乱字符串
tags:
  - leetcode
  - acwing
  - 每日一题
  - 算法
abbrlink: c4df32de
date: 2021-04-16 11:44:09
---

## [LeetCode 87.扰乱字符串](https://leetcode-cn.com/problems/scramble-string/)

#### 思路

这道题目前没有思路...先贴一道标程等什么时候理解了再补，如果有会推DP的大佬欢迎指点小海豚- -

#### C++代码

```c++
class Solution {
public:
    bool isScramble(string s1, string s2) {
        int n = s1.size();
        vector<vector<vector<bool>>> f(n, vector<vector<bool>>(n, vector<bool>(n+1)));
        for(int k=1;k<=n;k++)
        {
            for(int i=0;i+k-1<n;i++)
            {
                for(int j=0;j+k-1<n;j++)
                {
                    if(k==1)
                    {
                        if(s1[i]==s2[j])    f[i][j][k]=true;
                    }
                    else
                    {
                        for(int u=1;u<k;u++)
                        {
                            if(f[i][j][u] && f[i+u][j+u][k-u] || f[i][j+k-u][u] && f[i+u][j][k-u])
                            {
                                f[i][j][k]=true;
                                break;
                            }
                        }
                    }
                }
            }
        }
        return f[0][0][n];
    }
};
```

## [AcWing 94.递归实现排列型枚举](https://www.acwing.com/problem/content/96/)

#### 思路

本题要求是按照字典序给出全排列顺序，因此需要逐个字符的递归式判断，所以可以判定此题就是用DFS求解即可，当每次递归到边界时候说明一个方法已经走完了，这时候就可以结束当前递归，依次输出数字，然后继续下一个（注意状态恢复）即可，最终的边界部位直接退出程序即可

#### C++代码

```c++
#include<iostream>

using namespace std;

const int N=10;

int n;
int nums[N];
bool st[N];

void dfs(int u)
{
    if(u>n)
    {
        for(int i=1;i<=n;i++)
            cout<<nums[i]<<' ';
        cout<<endl;
        return;
    }
    for(int i=1;i<=n;i++)
    {
        if(!st[i])
        {
            nums[u]=i;
            st[i]=true;
            dfs(u+1);
            st[i]=false;
            nums[u]=0;
        }
    }
}

int main()
{
    cin>>n;
    dfs(1);
    return 0;
}
```

