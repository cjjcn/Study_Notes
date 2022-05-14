---
title: ACM-DFS-Practice
tags: 
- ACM
- DFS
abbrlink: 7a871ff6
date: 2020-11-17 17:53:56
---
# 部分和问题

> 给定整数a1,a2...,an,判断是否可以从中选出若干数，使他们的和恰好为k
<!--more-->
```c
#include<iostream>
#include<cstdio>

using namespace std;

const int maxn=20 + 5;

int a[maxn],n,k;

bool dfs(int i,int sum)
{
    if(i==n)    return sum == k;
    if(dfs(i+1, sum))   return true;
    if(dfs(i+1, sum+a[i]))  return true;
    return false;
}

int main(){
    cin>>n;
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    cin>>k;
    if(dfs(0,0))    cout<<"Yes"<<endl;
    else cout<<"No"<<endl;
    return 0;
}
```

# Lake Counting(POJ2386)
```c
#include<iostream>
#include<cstdio>

using namespace std;

const int maxn = 100 + 5;

int N,M;
char f[maxn][maxn];

void dfs(int x, int y)
{
    f[x][y]='.';
    for(int dx=-1;dx<=1;dx++)
        for(int dy=-1;dy<=1;dy++)
        {
            int nx=x+dx, ny=y+dy;
            if(0 <= nx && nx < N && 0<=ny && ny< M && f[nx][ny]=='W')
                dfs(nx,ny);
        }
    return;
}

void solve()
{
    int res=0;
    for(int i=0;i<N;i++)
    {
        for(int j=0;j<M;j++)
        {
            if(f[i][j]=='W')
            {
                dfs(i, j);
                res++;
            }
        }
    }
    cout<<res<<endl;
}

int main(){
    cin>>N>>M;
    for(int i=0;i<N;i++)
        for(int j=0;j<M;j++)
            cin>>f[i][j];
    solve();
    return 1;
}
```