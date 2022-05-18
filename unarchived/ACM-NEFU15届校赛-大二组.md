---
title: ACM-NEFU15届校赛-大二组
tags:
  - ACM
abbrlink: 8a3e9951
date: 2020-12-09 17:46:31
---

# A.小林找工作

```c
#include<bits/stdc++.h>

using namespace std;

const int MAXN=1e5+10;
int p[MAXN];

int main()
{
	int n,m;
	cin>>n>>m;
	for(int i=1;i<=n;i++)
	{
		scanf("%d", &p[i]);
	}
	sort(p+1,p+n+1);
	for(int i=0;i<m;i++)
	{
		int temp;
		scanf("%d", &temp);
		int pos=lower_bound(p+1,p+n+1,temp)-p;
		if(pos>n)	pos=n;
		int ans=abs(temp-p[pos]);
		if(pos!=1)	ans=min(ans, abs(temp-p[pos-1]));
		printf("%d\n", ans);
	}
	return 0;
}
```
<!--more-->

# B.xx的树

```c
#include<bits/stdc++.h>

using namespace std;

const int MAXN=2e5+10;
typedef long long ll;

typedef struct{
	int to;
	int next;
}edge;

edge e[MAXN];
int cur=1,head[MAXN],vis[MAXN];
ll value[MAXN];

void add(int u,int v)
{
	e[cur].to=v;
	e[cur].next=head[u];
	head[u]=cur++;
}

void dfs(int u)
{
	vis[u]=1;
	for(int i=head[u];i!=0;i=e[i].next)
	{
		int v=e[i].to;
		if(vis[v])	continue;
		value[v]+=value[u];
		dfs(v);
	}
}

int main()
{
	int n,m;
	cin>>n>>m;
	for(int i=1;i<n;i++)
	{
		int u,v;
		cin>>u>>v;
		add(u,v);
		add(v,u);
	}
	for(int i=1;i<=m;i++)
	{
		ll u,d;
		cin>>u>>d;
		value[u]+=d;
	}
	dfs(1);
	for(int i=1;i<=n;i++)
		i==1?cout<<value[i]:cout<<" "<<value[i];
	return 0;
}
```



# C.xx玩游戏

找到规律，偶数列没有必败态，奇数列有（n+1）/2个必败态

```c
#include<bits/stdc++.h>

using namespace std;

typedef long long ll;

int main()
{
	int t;
	cin>>t;
	while(t--)
	{
		ll n;
		scanf("%lld", &n);
		ll t=(n+1)/2;
		ll lose=(1+t)*t/2;
		ll sum=(1+n)*n/2;
		printf("%lld\n", sum-lose);
	}
	return 0;
}
```



# D. xx的零食店(待补)

# E.qyh的签到题

跟去年校赛大一的一道题神似...

```c
#include<bits/stdc++.h>

using namespace std;

int n,q;
typedef long long ll;
const long long mod=1e9+7;
const int MAXN=1e5+5;
ll d1[MAXN],d2[MAXN],d3[MAXN];

void pre_sum(ll d[])
{
	for(int i=1;i<=n;i++)
	{
		d[i]+=d[i-1];
		d[i]%=mod;
	}
}

int main()
{
	scanf("%d%d",&n, &q);
	for(int i=0;i<q;i++)
	{
		int type,pos;
		scanf("%d%d", &type, &pos);
		if(type==1)	d1[pos]++;
		if(type==2)	d2[pos]++;
		if(type==3)
		{
			d3[pos]++;
			d3[pos+1]++;
		}
	}
	pre_sum(d3);	pre_sum(d3);	pre_sum(d3);
	pre_sum(d2);	pre_sum(d2);
	pre_sum(d1);
	for(int i=1;i<=n;i++)
	{
		i==1?printf("%lld", (d1[i]+d2[i]+d3[i])%mod):printf(" %lld", (d1[i]+d2[i]+d3[i])%mod);
	}
	return 0;
}
```



# G.天哥的序列

```c

```

