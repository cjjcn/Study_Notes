---
title: AcWing第42场周赛
tags:
  - 算法
  - acwing
abbrlink: ad9e07e4
date: 2022-03-13 15:09:01
---

# [4311. 最小值](https://www.acwing.com/problem/content/4314/)

## 思路

枚举暴力出奇迹

## 代码

```c++
#include<iostream>
#include<cstdio>

using namespace std;

int main()
{
    int n,m;
    cin>>n>>m;
    double res=0x3f3f;
    for(int i=0;i<n;i++)
    {
        double a,b;
        cin>>a>>b;
        res = min(res, a/b);
    }
    res*=m;
    //cout<<res<<endl;
    printf("%.6lf", res);
    return 0;
}
```

# [4312. 出现次数](https://www.acwing.com/problem/content/4315/)

## 思路

前缀和思想，记录字符串在前i个字符里面有多少个子串，然后根据输入的区间进行相减即可得出结果，特别注意数组操作的边界

## 代码

```c++
#include<bits/stdc++.h>

using namespace std;

int m,n,q;
string S,T;
int s[1010];

int main()
{
    cin>>n>>m>>q;
    cin>>S>>T;
    S= ' '+ S;
    for(int i=m;i<=n;i++)
    {
        s[i] = s[i-1];
        if(S.substr(i-m+1, m) == T)
        {
            s[i]++;
        }
    }
    //for(int i=0;i<=n;i++)   cout<<i<<": "<<s[i]<<endl;
    for(int i=0;i<q;i++)
    {
        int l,r;
        cin>>l>>r;
        l+= m-1;
        if(l > r)   cout<<0<<endl;
        else cout<<s[r]-s[l - 1]<<endl;
    }
    return 0;
}
```

# [AcWing 4313. 满二叉树等长路径](https://www.acwing.com/problem/content/4316/)

## 思路

画图可知对于满二叉树要求增加路径长度且要保证两边平衡，易证得按照自顶向下的优先度进行边长度添加所增加的路径长度最小，如果自下部进行添加则需要在各级父节点路径添加相应长度以保证树长度平衡。

其次，由提议可知在保证增加路径最小的前提下增加的长度为$d=max(x, y)$，则需要添加的长度可列式
$$
ans=d-x+d-y\\
= 2d-(x+y)\\
= |x-y|
$$
因此，根据上面式子，我们只需要保证各个子树自身平衡即可，而不用考虑当前节点的兄弟节点内部情况，由此得解

```c++
#include <iostream>
#include <cstring>
#include <algorithm>


using namespace std;

const int N = 2050;

int n;
int ans;
int w[N];

int dfs(int u)
{
    if(u * 2 > (1<<n+1) - 1)    return 0;
    int l = dfs(u*2) + w[u*2];
    int r = dfs(u*2+1) + w[u*2+1];
    ans+=abs(l-r);
    return max(l, r);
}

int main()
{
    cin>>n;
    for(int i=2;i<=(1<<n+1) - 1; i++)   cin>>w[i];
    dfs(1);
    cout<<ans<<endl;
    return 0;
}
```

