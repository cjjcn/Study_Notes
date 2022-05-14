---
title: ACM-CodeForces-#685(Div.2)
tags:
  - ACM
  - CF
abbrlink: e1e96252
date: 2020-11-22 10:46:08
---

好久没见过CF有这么水的contest了，蒟蒻赶紧~~找找自信~~

<!--more-->

# A. Subtract or Divide

```c++
#include<iostream>
 
using namespace std;
 
int main(){
	int T,n;
	cin>>T;
	while(T--)
	{
		cin>>n;
		if(n<=3)	n--;
		else	n=2+(n&1);
		cout<<n<<endl;
	}
	return 0;
}
```



# B. Non-Substring Subsequence

```c++
#include<iostream>
#include<cstring>
 
using namespace std;
 
int main(){
	int T;
	cin>>T;
	while(T--)
	{
		int n,q;
		string s;
		cin>>n>>q>>s;
		while(q--)
		{
			int l,r;
			bool good=false;
			cin>>l>>r;
			l--;
			r--;
			for(int i=0;i<l && good==false; i++)
				if(s[i]==s[l])	good=true;
			for(int i=r+1;i<n && good==false;i++)
				if(s[i]==s[r])	good=true;
			if(good==true)	cout<<"YES"<<endl;
			else	cout<<"NO"<<endl;
		}
	}
	return 0;
}
```

# C. String Equality

这里有必要记录两个很有意思的小知识点，都是C11标准里面新增的，array数组和auto类型变量，auto类型可以根据你赋值的数据自动分配数据类型，用起来十分方便

> array<T,N> 模板定义了一种相当于标准数组的容器类型。它是一个有 N 个 T 类型元素的固定序列。除了需要指定元素的类型和个数之外，它和常规数组没有太大的差别。显然，不能增加或删除元素。
>
> 模板实例的元素被内部存储在标准数组中。和标准数组相比，array 容器的额外幵销很小，但提供了两个优点：如果使用 at()，当用一个非法的索引访问数组元素时，能够被检测到，因为容器知道它有多少个元素，这也就意味着数组容器可以作为参数传给函数，而不再需要单独去指定数组元素的个数。
>
> 使用 array 容器类型时，需要在源文件中包含头文件 array。

```c++
#include<iostream>
#include<cstring>
#include<algorithm>
#include<array>
 
using namespace std;
 
int main(){
	int T;
	cin>>T;
	while(T--)
	{
		int n,k;
		string s1,s2;
		array<int, 27> have{}, need{};
		cin>>n>>k>>s1>>s2;
		for(auto& c: s1)
			have[c-'a']++;
		for(auto& c: s2)
			need[c-'a']++;
		bool good = true;
		for(int i=0;i<26;i++)
		{
			if(have[i]<need[i] || (have[i] -= need[i]) % k)
				good=false;
			have[i+1]+=have[i];
		}
		if(good)	cout<<"Yes"<<endl;
		else	cout<<"No"<<endl;
	}
	return 0;
}
```

# D.Circle Game

逻辑题

```c++
#include <bits/stdc++.h>
using namespace std;
 
int main() 
{
    int T;
    cin >> T;
    while(T--) {
        int n, k;
        cin >> n >> k;
        int x = 0, y = 0;
        for(;;) 
		{
            if(x <= y && pow(x+k, 2) + pow(y, 2) <= pow(n, 2))
                x += k;
            else if(x > y && pow(y+k, 2) + pow(x, 2) <= pow(n,2))
                y += k;
            else	break;
        }
        if(x == y)
            cout << "Utkarsh" << endl;
        else
            cout << "Ashish" << endl;
    }
    return 0;
}
```

