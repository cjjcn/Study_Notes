---
title: Notes-STL-dfs
tags: ACM
abbrlink: 3fbdba09
date: 2020-10-15 21:42:10
---

```C++
#include<iostream>

using namespace std;

int book[101],sum,n,e[101][101];

void dfs(int cur)
{
	cout<<cur<<" ";
	sum++;
	if(sum==n)	return;
	for(int i=1;i<=n;i++)
	{
		if(e[cur][i]==1 && book[i] == 0)
		{
			book[i]=1;
			dfs(i);
		}
	}
	return;
}

int main(){
	int m,a,b;
	cin>>n>>m;
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			if(i==j)	e[i][j]=0;
			else e[i][j]=99999999;
		}
	}
	for(int i=1;i<=m;i++)
	{
		cin>>a>>b;
		e[a][b]=1;
		e[b][a]=1;
	}
	
	book[1]=1;
	dfs(1);
	
	return 0;
}
```
