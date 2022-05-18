---
title: ACM-NEFUOJ-P209湖南修路
tags:
  - ACM
  - Prim
abbrlink: '40276439'
date: 2020-12-19 15:51:59
---

# 思路

prim的最小生成树，套上肝就完事了

# 代码

```c++
#include<iostream>
#include<cstdio>
#include<string.h>

#define MAX 999999;
using namespace std;

int map[105][105],dist[105];
bool vis[105];
int prim(int n)
{
    int i,j,pos;
    int min,sum;
    sum=0;
    memset(vis,false,sizeof(vis));
    for(i=1; i<=n; i++)
    {
        dist[i]=map[1][i];
    }
    vis[1]=true;
    dist[1]=MAX;
    for(i=1; i<n; i++)
    {
        min=MAX;
        for(j=1; j<=n; j++)
        {
            if(!vis[j] && dist[j]<min)
            {
                min=dist[j];
                pos=j;
            }
        }
        sum+=min;
        vis[pos]=true;
        for(j=1; j<=n; j++)
        {
            if(!vis[j] && dist[j]>map[pos][j])
            {
                dist[j]=map[pos][j];
            }
        }
    }
    return sum;
}
int main()
{
    int n,a,b;
    int c,d;
    while(~scanf("%d",&n))
    {
        if(n==0)
            break;
        for(int i=0; i<(n*(n-1)/2); i++)
        {
            cin >> a>> b>>c>>d;
            if(d==1)
                map[a][b]=map[b][a]=0;
            else
                map[a][b]=map[b][a]=c;
        }
        int ans=prim(n);
        printf("%d\n",ans);
    }
    return 0;
}
```

