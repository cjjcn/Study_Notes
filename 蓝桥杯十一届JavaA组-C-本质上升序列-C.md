---
title: 蓝桥杯十一届JavaA组-C++解题
tags:
  - ACM
  - 蓝桥杯
abbrlink: 1162e7f2
date: 2020-11-20 20:05:45
---

本人随便乱写，目前正确性未知

<!--more-->

# C.本质上升序列



```c++
#include<bits/stdc++.h>

using namespace std;

bool access[4][4];

int dfs(int idx, int x, int y)
{
	if(x<0 || y<0 || x>=4 || y>=4)	return 0;
	if(access[y][x])	return 0;
	if(idx>=15)	return 1;
	int count=0;
	access[y][x]=true;
	count+=dfs(idx+1, x+1, y);
	count+=dfs(idx+1, x, y+1);
	count+=dfs(idx+1, x-1, y);
	count+=dfs(idx+1, x, y-1);
	access[y][x]=false;
	return count;
}

int main(){

	int count =0;
	for(int i=0;i<16;i++)
	{
		count+=dfs(0, i%4, i/4);
	}
	cout<<count;
}
```



# H. 奇偶覆盖

线段树初步版本，未完善
```c++
#include<bits/stdc++.h>

using namespace std;

const int N=1e5+5;

struct node
{
    int l,r,h;
    bool operator< (const node& a)const
    {
        return h<a.h;
    }
}e[N*2];

int a[N*2];
int sum[N*2*4],flag[N*2*4];

void cal(int l,int r,int root)
{
    sum[root]=(a[r]-a[l])-sum[root];
}

void pushup(int root)
{
    sum[root]=sum[root<<1]+sum[root<<1|1];
}

void pushdown(int l,int r,int root)
{
    if(!flag[root])
        return ;
    flag[root<<1]^=1;
    flag[root<<1|1]^=1;
    int mid=l+r>>1;
    cal(l,mid,root<<1);
    cal(mid,r,root<<1|1);
    flag[root]=0;
}

void update(int l,int r,int root,int ql,int qr)
{
    if(l>=ql&&r<=qr)
    {
        cal(l,r,root);
        flag[root]^=1;
        return ;
    }
    pushdown(l,r,root);
    int mid=l+r>>1;
    if(mid>ql)
        update(l,mid,root<<1,ql,qr);
    if(mid<qr)
        update(mid,r,root<<1|1,ql,qr);
    pushup(root);
}

int main()
{
    int n;
    scanf("%d",&n);
    int x1,x2,y1,y2,all=0;
    for(int i=1;i<=n;i++)
    {
        scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
        e[2*i-1].l=min(x1,x2);
        e[2*i-1].r=max(x1,x2);
        e[2*i-1].h=min(y1,y2);
        e[2*i].l=min(x1,x2);
        e[2*i].r=max(x1,x2);
        e[2*i].h=max(y1,y2);
        a[2*i-1]=x1,a[2*i]=x2;
    }
    sort(a+1,a+1+n*2);
    all=unique(a+1,a+1+n*2)-a-1;
    for(int i=1;i<=2*n;i++)
        e[i].l=lower_bound(a+1,a+1+all,e[i].l)-a,e[i].r=lower_bound(a+1,a+1+all,e[i].r)-a;
    sort(e+1,e+1+2*n);
    ll ans=0;
    for(int i=1;i<=2*n;i++)
    {
        ans+=sum[1]*(e[i].h-e[i-1].h);
        update(1,all,1,e[i].l,e[i].r);
    }
    printf("%lld\n",ans);
}
```