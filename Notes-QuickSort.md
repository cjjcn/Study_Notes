---
title: Notes-QuickSort
tags: ACM
abbrlink: 12aa90f7
date: 2020-10-10 12:03:02
---
# 思路
快排基本思路应该就是二分+递归，从两侧同时（实则先从右往左）往中间找，同时和参变量对比，发现位置颠倒后交换位置，然后通过二分将其一块一块的分割开，直到分割到一个元素位置，即完成了快排。

<!--more-->

# 代码
```C++
#include<bits/stdc++.h>

using namespace std;

int a[101],n;

void quicksort(int left,int right)
{
	int i,j,t,temp;//temp存基准数 
	if(left>right)	return;
	
	temp=a[left];
	i=left;
	j=right;
	while(i!=j)
	{
		while(a[j]>=temp && i<j)	j--;
		while(a[i]<=temp && i<j)	i++;
		
		if(i<j)
		{
			t=a[i];
			a[i]=a[j];
			a[j]=t;
		}
	}
	
	a[left]=a[i];
	a[i]=temp;
	
	quicksort(left, i-1);
	quicksort(i+1,right);
	return;
}

int main(){
	cin>>n;
	for(int i=1;i<=n;i++)
		cin>>a[i];
	quicksort(1,n);
	
	for(int i=1;i<=n;i++)
	{
		cout<<a[i]<<" ";
	}
	
	return 0;
}
```

# 总结
快排应该是最常用的模板了，时间复杂度也比较理想

PS.致敬一波快排的提出者东尼·霍尔(C. A. R. Hoare)