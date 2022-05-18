---
title: NEFUOJ208-宫锁珠帘
tags:
  - ACM
  - NEFUOJ
abbrlink: cd90dd49
date: 2021-03-08 17:44:43
---

## 题目

### Description

```
新年期间湖南卫视又上映了不少宫闱大戏，晴川穿走了，又来了个宫锁珠帘的袁珊珊，袁珊珊不够美，没锁住观众的心，特别是咱们整ACM的，对这些宫闱大戏当然是不屑一顾了，可这新亮剑的李云龙咱们还是有得一看的，最近听说A地又驻扎了一个骑兵营，李云龙心里就乐了，心想老子吃了骑兵营这么多亏，这下终于有机会自个儿也能整个啥骑兵营威风威风了，于是拿出地图决定抄最短的路立马拿下这个骑兵营。
```

### Input

```
每组数据第一行包含两个正整数N和M(0 &lt; N &lt; 100,0 &lt; M &lt; 100)，分别代表李云龙现有地图上显示的村子或者驻扎地的总数目，分别以0～N-1编号。
接下来是M行道路信息。每一行有三个整数A,B,X(0 &lt;= A,B &lt; N,A!=B,0&lt; X &lt; 10000),表示A和B之间有一条长度为X的双向道路。
再接下一行有两个整数S,T(0 &lt;= S,T &lt; N)，分别代表起点和终点。
```

### Output

```
输出从S到T的最短路的长度。若不存在这样的一条路，则输出-1.
```

### Sample Input

```
5 4
0 1 5
0 3 4
3 2 7
3 2 6
0 2
4 1 
2 3 1 
1 2
```

### Sample Output

```
10
-1
```

## 思路

初次读题，下意识的想到了推DP或者进行路径搜索，后来看了一眼数据量(0<M<100)...果断放弃推导，立地暴力，输入所有路径以后把每条路径都搜索一遍找出最短的路径即可

## 代码

```c
#include<iostream>
#include<cstdio>

using namespace std;

const int MAXN=99999;
const int N=110;
int map[N][N];

void init()
{
	for(int i=0;i<N; i++ )
		for(int j=0;j<N; j++ )
			map[i][j]=MAXN;
}

void find(int n)//纯暴力
{
	for(int k=0;k<n;k++)
		for(int i=0;i<n;i++)
			for(int j=0;j<n;j++)
				map[i][j]=min(map[i][j], map[i][k]+map[k][j]);
}

int main()
{
	init();
	int n,m;
	while(cin>>n>>m)
	{
	for(int i=0;i<m;i++)
	{
		int a,b,w;
		cin>>a>>b>>w;
		map[a][b]=min(map[a][b], w);
	}
	int s,t;
	cin>>s>>t;
	find(n);
	if(map[s][t]==MAXN)
		cout<<"-1"<<endl;
	else
		cout<<map[s][t]<<endl;
	}
	return 0;
}
```

