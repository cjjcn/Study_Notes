---
title: ACM-NEFUOJ-最小树-Prim算法
tags:
  - ACM
  - NEFUOJ
  - 搜索
  - Prim
abbrlink: d6807259
date: 2020-12-24 18:24:07
---

# 最小树1

### Description

```
某省长调查交通情况，发现本省交通事故发生不断，于是决定在本省内全部修建地铁。
该省长得到的统计表中列出了任意两市之间的距离，为了确保任何两个市都可以直接
或者间接实现地铁交通，并要求铺设的地铁总长度最小，请计算最小的地铁总长度。
```

### Input

```
测试输入包含若干测试用例。每个测试用例的第一行给出市的数目n,（n &lt; 50);随后的
n(n-1)/2行对应市之间的距离，每行给出一对正整数，分别是两个市的编号，以及两
市之间的距离。为简单起见，市从1到n编号，当n为0时，输入结束，该样例不做处理。
```

### Output

```
对每个测试用例，在一行里输出最小的地铁总长度，保留两位小数。
```

### Sample Input

```
3
1 2 1.8
1 3 2.9
2 3 4.5
0
```

### Sample Output

```
4.70
```

## Code

```c++
#include<bits/stdc++.h>

using namespace std;
const int INF = 99999999;

double mp[60][60],dis[60];
int vis[60];

double find(int n)
{
	double mi,sum=0;
	memset(vis, 0 ,sizeof(vis));
	for(int i=1;i<=n;i++)
	{
		dis[i]=mp[1][i];
	}
	vis[1]=1;
	for(int i=1;i<n;i++)
	{
		mi=INF;
		int pos;
		for(int j=1;j<=n;j++)
		{
			if(!vis[j]&&dis[j]<mi)
			{
				mi=dis[j];
				pos=j;
			}
		}
		vis[pos]=1;
		sum+=mi;
		for(int j=1;j<=n;j++)
		{
			if(!vis[j]&&dis[j]>mp[pos][j])
				dis[j]=mp[pos][j];
		}
	}
	return sum;
}

int main()
{
	int n,a,b;
	double l;
	while(cin>>n&&n)
	{
		for(int i=0;i<n*(n-1)/2;i++)
		{
			cin>>a>>b>>l;
			mp[a][b]=mp[b][a]=l;
		}
		double ans=find(n);
		printf("%.2lf\n", ans);
	}
	return 0;
}
```



# 最小树2

### Description

```
在森林里住了n只小熊，他们分别叫小熊A，小熊B……，小熊们决定修建水泥路让他们能更加方便的往来，使得任何一只小熊都能轻松到达其他小熊的家，同时小熊们希望修建的水泥路最短。
```

### Input

```
测试输入若干实例，每个测试实例第一行给出小熊的数目n和小熊们能直接通往的m条道路，（n≤26,m &lt; 100)接下来m行，每行为两只小熊的名字（分别为A,B,C.........,若n为3，则只会出现A,B,C,依次类推）以及这两只小熊之间的距离（为正整数）。
```

### Output

```
输出最短的水泥路的长度，若不能满足任何一只小熊到其他所有小熊的家，则输出-1.
```

### Sample Input

```
3 3
A B 1
A C 2
B C 4
3 1
A B 2
```

### Sample Output

```
3
-1
```

## Code

```c++
#include<bits/stdc++.h>

#define INF 0x3f3f3f3f

using namespace std;

int mp[30][30],dis[30];
int vis[30],n;

void init()
{
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			if(i==j)	mp[i][j]=0;
			else
				mp[i][j]=INF;
		}
	}
};

int find(int n)
{
	memset(vis, 0, sizeof(vis));
	int mi,pos,sum=0;
	for(int i=1;i<=n;i++)	dis[i]=mp[1][i];
	vis[1]=1;
	for(int i=1;i<n;i++)
	{
		mi=INF;
		int flag=0;
		for(int j=1;j<=n;j++)
		{
			if(!vis[j]&&dis[j]<mi)
			{
				mi=dis[j];
				pos=j;
				flag=1;
			}
		}
		if(!flag)	return -1;
		sum+=mi;
		vis[pos]=1;
		for(int j=1;j<=n;j++)
		{
			if(!vis[j]&&dis[j]>mp[pos][j])
			{
				dis[j]=mp[pos][j];
			}
		}
	}
	return sum;
}

int main()
{
	int m,l;
	string a,b;
	while(cin>>n>>m)
	{
		init();
		for(int i=0;i<m;i++)
		{
			cin>>a>>b>>l;
			int x=a[0]-'A'+1;
			int y=b[0]-'A'+1;
			mp[x][y]=mp[y][x]=l;
		}
		int ans=find(n);
		cout<<ans<<endl;
	}
	return 0;
}
```



# 最小树3

### Description

```
有n个站，求从1站到n站的最短路线。
```

### Input

```
输入第一行n和m,  n表示有n个站，m表示有m条道路，(n,m&lt;100)接下来m行每一行输入三个数a,b,d,表示a和b之间有一条长为d 的路。
```

### Output

```
输出从1到n的最短距离。
```

### Sample Input

```
5 4
1 2 1
1 5 5
2 4 2
4 5 1 
2 1
1 2 3
```

### Sample Output

```
4
3
```

## Code

```c++
#include<iostream>
#include<cstdio>

using namespace std;

int e[101][101];
int n,m;

void initial()
{
    for(int i=1;i<=n;i++)
     for(int j=1;j<=n;j++)
      if(i==j)
       e[i][j]=0;
      else 
       e[i][j]=99999999;
}
void find()
{
    for(int k=1;k<=n;k++)
     for(int i=1;i<=n;i++)
      for(int j=1;j<=n;j++)
       if(e[i][j]>e[i][k]+e[k][j])
        e[i][j]=e[i][k]+e[k][j];
}
int main()
{
    while(cin>>n>>m)
    {
        initial();
        for(int i=1;i<=m;i++)
        {
            int a,b,c;
            cin>>a>>b>>c;
            e[a][b]=e[b][a]=c;
        }
        find();
        cout<<e[1][n]<<endl;
    }
}
```

