---
title: ACM-学习记录-DP-1
tags:
  - ACM
  - DP
abbrlink: 2470a0b3
date: 2020-11-26 19:43:43
---

# DPL_1_A: Coin Changing Problem

每次均有两种选择，即选择当前的，即为在当前状态+1，否则维持原来的T[j+d[i]]

<!--more-->

```c++
#include<iostream>
#include<cstdlib>
#include<cstring>

using namespace std;

const int INF=(1<<29);

int main()
{
	int n,m;
	int d[20+5];
	int T[50000 + 5];
	cin>>n>>m;
	for(int i=1;i<=m;i++)
		cin>>d[i];
	for(int i=0;i<50000+5;i++)
	{
		T[i]=INF;
	}
	T[0]=0;
	//for(int i=0;i<=50000+5;i++)	cout<<T[i]<<endl;
	for(int i=1;i<=m;i++)
	{
		for(int j=0;j+d[i]<=n;j++)
		{
			T[j + d[i]] = min(T[j+d[i]], T[j]+1);
		}
	}
	cout<<T[n]<<endl;
	return 0;
}
```

# DPL_1_B: 0-1 Knapsack Problem

```c++
#include<iostream>
#include<vector>
#include<algorithm>

#define NMAX 105
#define WMAX 10005

using namespace std;

struct Item{
	int value;
	int weight;
};

int N,W;
Item items[NMAX];
int C[NMAX][WMAX], G[NMAX][WMAX];

void compute(int &maxValue, vector<int> &selection)
{
	for(int w=0;w<=W;w++)
	{
		C[0][w]=0;
		G[0][w]=1;
	}
	for(int i=1;i<=N;i++)
		C[i][0]=0;
	for(int i=1;i<=N;i++)
	{
		for(int w=1;w<=W;w++)
		{
			C[i][w]=C[i-1][w];
			G[i][w]=0;
			if(items[i].weight>w)	continue;
			if(items[i].value + C[i-1][w-items[i].weight]>C[i-1][w])
			{
				C[i][w]=items[i].value + C[i-1][w-items[i].weight];
				G[i][w]=1;
			}
		}
	}
	maxValue=C[N][W];
	selection.clear();
	for(int i=N,w=W;i>=1;i--)
	{
		if(G[i][w]==1)
		{
			selection.push_back(i);
			w-=items[i].weight;
		}
	}
	reverse(selection.begin(), selection.end());
}

int main()
{
	int maxValue;
	vector<int> selection;
	cin>>N>>W;
	for(int i=1;i<=N;i++)
	{
		cin>>items[i].value>>items[i].weight;
	}
	compute(maxValue, selection);
	cout<<maxValue<<endl;
	return 0;
}
```

# NEFUOJ P21 最长上升子序列

```c++
#include<iostream>
#include<algorithm>

#define MAX 100000

using namespace std;

int n,A[MAX+1],L[MAX];

int lis()
{
    L[0]=A[0];
    int length=1;
    for(int i=1;i<n;i++)
    {
        if(L[length-1]<A[i])
        {
            L[length++]=A[i];
        }
        else
            *lower_bound(L, L+length, A[i])=A[i];
    }
    return length;
}

int main()
{
    cin>>n;
    for(int i=0;i<n;i++)
    {
        cin>>A[i];
    }
    cout<<lis()<<endl;
    return 0;
}
```



# DPL_3_A: Largest Square

```c++
#include<iostream>
#include<algorithm>
#include<cstdio>

using namespace std;

#define MAX 1400

int dp[MAX][MAX], G[MAX][MAX];

int getS(int H,int W)
{
    int maxWidth = 0;
    for(int i=0;i<H;i++)
    {
        for(int j=0;j<W;j++)
        {
            dp[i][j]=(G[i][j]+1)%2;
            maxWidth |= dp[i][j];
        }
    }
    for(int i=1;i<H;i++)
    {
        for(int j=1;j<W;j++)
        {
            if(G[i][j])
            {
                dp[i][j]=0;
            }
            else
            {
                dp[i][j]=min(dp[i-1][j-1], min(dp[i-1][j], dp[i][j-1])) + 1;
                maxWidth=max(maxWidth, dp[i][j]);
            }
        }
    }
    return maxWidth * maxWidth;
}

int main()
{
    int H,W;
    cin>>H>>W;
    for(int i=0;i<H;i++)
        for(int j=0;j<W;j++)
            cin>>G[i][j];
        cout<<getS(H,W)<<endl;
        return 0;
}
```

