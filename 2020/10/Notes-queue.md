---
title: Notes-queue队列
tags: ACM
abbrlink: 64d83881
date: 2020-10-10 12:16:33
---

# 思路
队列的原理基本与站队一样，队首出，队尾入，变化以后也是大同小异，写起来主要就是注意struct的相关知识，以及伪指针（分别指向队首和队尾+1），队尾序号要+1以防首位变量数字重合造成不必要的麻烦（目前也不是很清楚会遇到什么）

<!--more-->

# 代码
```C++
#include<iostream>

using namespace std;

struct queue
{
	int data[100];
	int head;
	int tail;
};

int main(){
	struct queue q;
	int i;
	q.head=1;
	q.tail=1;
	for(i=1; i<=9;i++)
	{
		cin>>q.data[q.tail];
		q.tail++;
	}
	
	while(q.head<q.tail)
	{
		cout<<q.data[q.head];
		q.head++;
		q.data[q.tail]=q.data[q.head];
		q.tail++;
		q.head++;
	}
	return 0;
	
}
```

# 总结
写的过程中主要一开始用万能头和STL里面的queue冲突报错了。。（想了好久），其他没什么，队列作为广搜和Bellman-Ford的最短路核心算法还是很有必要理解并记忆的
