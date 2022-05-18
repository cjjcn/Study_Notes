---
title: ACM-学习记录-数据结构-1
tags:
  - ACM
abbrlink: ec23b92c
date: 2020-11-24 14:39:32
---

# AOJ-ALDS1_1_D Maximum Profit

本题主要考虑要将复杂度降到O(n)，否则过不了最后五组数据

```c++
#include<iostream>
#include<bits/stdc++.h>

using namespace std;

int main(){
	int n,maxv=-1e10;
	int stock[200000 + 5];
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>stock[i];
	}
	int minv=stock[0];
	for(int j=1;j<n;j++)
	{
		maxv=max(maxv, stock[j]-minv);
		minv=min(minv, stock[j]);
	}
	cout<<maxv<<endl;
	return 0;
}
```

<!--more-->

# STL标准库

## 栈stack

| 函数名  | 功能                 | 复杂度 |
| ------- | -------------------- | ------ |
| size()  | 返回栈的元素数       | O(1)   |
| top()   | 返回栈顶元素         | O(1)   |
| pop()   | 从栈中取出并删除元素 | O(1)   |
| push()  | 添加元素x            | O(1)   |
| empty() | 在栈为空时返回true   | O(1)   |

### ALDS1_3_A Stack

```c++
#include<iostream>
#include<cstdlib>
#include<stack>

using namespace std;

int main()
{
	stack<int> s;
	int a,b,x;
	string str;
	while(cin>>str)
	{
		if(str[0]=='+')
		{
			a=s.top(); s.pop();
			b=s.top();	s.pop();
			s.push(a+b);
		}
		else if(str[0]=='-')
		{
			b=s.top();	s.pop();
			a=s.top();	s.pop();
			s.push(a-b);
		}
		else if(str[0]=='*')
		{
			a=s.top();	s.pop();
			b=s.top();	s.pop();
			s.push(a*b);
		}
		else
		{
			s.push(atoi(str.c_str()));
		}
	}
	cout<<s.top()<<endl;
	return 0;
}
```

 ## 队列queue

| 函数名  | 功能                   | 复杂度 |
| ------- | ---------------------- | ------ |
| size()  | 返回队列元素数         | O(1)   |
| front() | 返回队头元素           |        |
| pop()   | 从队列中取出并删除元素 |        |
| push()  | 向队列中添加元素       |        |
| empty() | 在队列为空时返回true   |        |

```c++
#include<iostream>
#include<string>
#include<queue>
#include<algorithm>

using namespace std;

int main()
{
	int n,q,t;
	string name;
	queue<pair<string, int> > Q;
	cin>>n>>q;
	for(int i=0;i<n;i++)
	{
		cin>>name>>t;
		Q.push(make_pair(name, t));
	}
	
	pair<string ,int> u;
	int elap=0,a;
	
	while(!Q.empty())
	{
		u=Q.front();	Q.pop();
		a=min(u.second, q);
		u.second-=a;
		elap+=a;
		if(u.second>0)
		{
			Q.push(u);
		}
		else
		{
			cout<<u.first<<" "<<elap<<endl;
		}
	}
	return 0;
}
```

## vector

| 函数名      | 功能                     |
| ----------- | ------------------------ |
| size()      | 返回向量的元素数         |
| push_back() | 在向量末尾添加元素x      |
| pop_back()  | 删除向量的最后一个元素   |
| begin()     | 返回指向向量开头的迭代器 |
| end()       | 返回指向向量末尾的迭代器 |
| insert(p,x) | 在向量的位置p处插入元素x |
| erase(p)    | 删除向量中位置p的元素    |
| clear()     | 删除向量中的所有元素     |

## 双向链表List

| 函数名        | 功能                   |
| ------------- | ---------------------- |
| size()        | 返回表的元素数         |
| begin()       | 返回指向表开头的迭代器 |
| end()         | 返回指向表末尾的迭代器 |
| push_front(x) | 在表开头添加元素x      |
| push_back(x)  | 在表末尾添加元素x      |
| pop_front()   | 删除位于表开头的元素   |
| pop_back()    | 删除位于表末尾的元素   |
| insert(p, x)  | 在表的位置p处插入元素x |
| erase(p)      | 删除表中位置p的元素    |
| clear()       | 删除表中所有元素       |

### ALDS1_3_C: Doubly Linked List

```c++
#include<iostream>
#include<cstdlib>
#include<list>
#include<algorithm>
#include<string>

using namespace std;

int main()
{
	int q,x;
	string command;
	list<int> v;
	cin>>q;
	for(int i=0;i<q;i++)
	{
		cin>>command;
		if(command[0]=='i')
		{
			cin>>x;
			v.push_front(x);
		}
		else if(command[6]=='L')
		{
			v.pop_back();
		}
		else if(command[6]=='F')
		{
			v.pop_front();
		}
		else if(command[0]=='d')
		{
			cin>>x;
			for(list<int>::iterator it=v.begin();it!=v.end();it++)
			{
				if(*it == x)
				{
					v.erase(it);
					break;
				}
			}
		}
	}
	int i=0;
	for(list<int>::iterator it=v.begin(); it!=v.end();it++)
	{
		if(i++)	cout<<" ";
		cout<<*it;
	}
	cout<<endl;
	return 0;
}
```

## ALDS1_3_D: [Areas on the Cross-Section Diagram](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=ALDS1_3_D)

* 如果是“\”，则将表示该字符位置的整数i压入栈S1
* 如果是“/”，则送S1顶部取出与之对应的"\"的位置i，算出二者的距离并累加到总面积内
* “_”的作用只是将一对\/距离增加1，然而在代码中已经通过数学方法计算了，因此可以忽略掉"__"符号
* 新形成的面积=当前S2中的两个面积之和+新形成的i-j部分的面积，从S1中取出被引用的多个面积，再将新算出的面积压入S2。

```C++
#include<iostream>
#include<cstdlib>
#include<stack>
#include<algorithm>
#include<vector>

using namespace std;

int main()
{
	stack<int> S1;
	stack<pair<int ,int> > S2;
	char c;
	int sum=0;
	for(int i=0;cin>>c;i++)
	{
		if(c=='\\')	S1.push(i);
		else if(c=='/' && S1.size()>0)
		{
			int j=S1.top(); S1.pop();
			sum+=i-j;
			int a=i-j;
			while(S2.size()>0 && S2.top().first>j)
			{
				a+=S2.top().second;	S2.pop();
			}
			S2.push(make_pair(j, a));
		}
	}
	vector<int> ans;
	while(S2.size()>0)
	{
		ans.push_back(S2.top().second);
		S2.pop();
	}
	reverse(ans.begin(),ans.end());
	cout<<sum<<endl;
	cout<<ans.size();
	for(int i=0;i<ans.size();i++)
	{
		cout<<" "<<ans[i];
	}
	cout<<endl;
}
```

## 迭代器

### 基本运算符

| 符号   | 作用                                       |
| ------ | ------------------------------------------ |
| ++     | 让迭代器指向至下一元素                     |
| ==，!= | 判断两个迭代器是否指向同一位置并返回结果   |
| =      | 将右侧的值代入左侧迭代器所引用的元素的位置 |
| *      | 返回该位置的元素                           |

## lower_bound

返回一个指向第一个不小于指定值value的元素

## 排序sort

stable_sort较稳定

> ***\*sort的应用\****；
>
> 1、可以传入两个参数；
>
>    sort(a,a+N) ,其中a是数组，a+N表示对a[0]至a[N-1]的N个数进行排序(默认从小到大排序)；
>
> 2、传入三个参数；
>
>    sort(a,a+N,cmp),第三个参数是一个函数 ；
>
>    如果让函数从大到小排序，可以用如下算法实现；
>
>    bool cmp(int a,int b){return a>b};
>
>    sort(A,A+N,cmp);
>
> 而***\*stable_sort的用法\****与sort一致，**区别是stable_sort函数遇到两个数相等时，不对其交换顺序**；这个应用在数组里面不受影响，当函数参数传入的是结构体时，会发现两者之间的明显区别；

## ALDS1_6_D: Minimum Cost Sort

```c++
#include<iostream>
#include<cstdlib>
#include<algorithm>

using namespace std;

const int maxn=1000;
const int vmax=10000;

int n,a[maxn],s;
int b[maxn],t[vmax+1];

int solve()
{
	int ans=0;
	bool v[maxn];
	for(int i=0;i<n;i++)
	{
		b[i]=a[i];
		v[i]=false;
	}
	sort(b,b+n);
	for(int i=0;i<n;i++)
	{
		t[b[i]] = i;
	}
	for(int i=0;i<n;i++)
	{
		if(v[i])	continue;
		int cur=i,m=vmax,an=0,S=0;
		while(1)
		{
			v[cur]=true;
			an++;
			int V=a[cur];
			m=min(m,V);
			S+=V;
			cur=t[V];
			if(v[cur])	break;
		}
		ans+=min(S+(an - 2) *m, m+S+(an+1)*s);//2SOLUTIONS
	}
	return ans;
}

int main()
{
	cin>>n;
	s=vmax;
	for(int i=0;i<n;i++)
	{
		cin>>a[i];
		s=min(s,a[i]);
	}
	int ans=solve();
	cout<<ans<<endl;
	return 0;
}
```

# 动态规划DP

## LCS

```c++
#include<iostream>
#include<string>
#include<algorithm>
#include<cstring>

const int N=1000;
int c[N+5][N+5];

using namespace std;

int lcs(string X, string Y)
{
	int m=X.length();
	int n=Y.length();//也可.size()
	int maxl=0;
	X=' '+X;
	Y=' '+Y;
	for(int i=1;i<=m;i++)	c[i][0]=0;
	for(int j=1;j<=n;j++)	c[0][j]=0;
	for(int i=1;i<=m;i++)
	{
		for(int j=1;j<=n;j++)
		{
			if(X[i]==Y[j])
			{
				c[i][j]=c[i-1][j-1]+1;
			}
			else
			{
				c[i][j]=max(c[i-1][j], c[i][j-1]);
			}
			maxl=max(maxl, c[i][j]);
		}
	}
	return maxl;
}

int main()
{
	string s1,s2;
	int n;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>s1>>s2;
		cout<<lcs(s1,s2)<<endl;
	}
	return 0;
}
```



