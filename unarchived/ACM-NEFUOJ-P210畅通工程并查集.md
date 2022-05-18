---
title: ACM-NEFUOJ-P210畅通工程并查集
tags:
  - ACM
  - 并查集
abbrlink: a630980b
date: 2020-12-19 15:59:29
---

题目：我已经明示到这个程度了你还不用并查集？

```c++
#include<bits/stdc++.h>

using namespace std;

const int MAXN=1010;

int F[MAXN];

int GetFather(int x)
{
    return F[x]==x?x:F[x]=GetFather(F[x]);
}

void Union(int a,int b)
{
    int t1=GetFather(a);
    int t2=GetFather(b);
    if(t1!=t2) F[t1]=t2;
}

int main()
{
    int n,m;

    while(cin>>n&&n)
    {
        cin>>m;
        for(int i=1;i<=n;i++) F[i]=i;
        int a,b;
        while(m--)
        {
            cin>>a>>b;
            Union(a,b);
        }
        int res=0;
        for(int i=1;i<=n;i++)
          if(F[i]==i) res++;
        printf("%d\n",res-1);
    }
    return 0;
}
```

