---
title: ACM-DP-数塔问题
tags:
  - ACM
  - DP
abbrlink: 656af927
date: 2020-12-20 13:00:21
---

Description

在讲述DP算法的时候，一个经典的例子就是数塔问题，它是这样描述的： 

有如下所示的数塔，要求从顶层走到底层，若每一步只能走到相邻的结点，则经过的结点的数字之和最大是多少？ 

已经告诉你了，这是个DP的题目，你能AC吗?

Input

输入数据首先包括一个整数C,表示测试实例的个数，每个测试实例的第一行是一个整数N(1 <= N <= 100)，表示数塔的高度，接下来用N行数字表示数塔，其中第i行有个i个整数，且所有的整数均在区间[0,99]内。 

Output

对于每个测试实例，输出可能得到的最大和，每个实例的输出占一行。 

Sample Input

```
1
5
7
3 8
8 1 0 
2 7 4 4
4 5 2 6 5
```

Sample Output

```
30
```

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
	int T;
	scanf("%d",&T);
	while(T--)
	{
		int i,j,n;
		int a[200][200],dp[200];
		scanf("%d",&n);
		for(i=1;i<=n;i++)
		for(j=1;j<=i;j++)
		scanf("%d",&a[i][j]);
		memset(dp,0,sizeof(dp));
		for(j=1;j<=n;j++)
		dp[j]=a[n][j];                          //对最低层进行初始化 
		for(i=n-1;i>=1;i--)
		{
			for(j=1;j<=i;j++)
			dp[j]=max(dp[j],dp[j+1])+a[i][j];   //状态转移方程 ，由下往上求最大值 
		}
		printf("%d\n",dp[1]);
	}
	return 0;
 } 
```

