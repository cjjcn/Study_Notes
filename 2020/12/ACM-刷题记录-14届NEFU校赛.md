---
title: ACM-刷题记录-14届NEFU校赛
tags:
  - ACM
abbrlink: 77f448c2
date: 2020-12-03 16:34:50
---

# P2031凯撒密码

```c++
#include<bits/stdc++.h>

using namespace std;

int main(){
	string s;
	int d;
	while(cin>>s)
	{
		cin>>d;
		int len=s.length();
		for(int i=0;i<len;i++)
		{
			s[i]=s[i]-'a'-d;
			if(s[i]<0)	s[i]=26+s[i];
			s[i]=s[i]%26+'a';
			printf("%c", s[i]);
		}
		cout<<endl;
	}
	return 0;
}
```
<!--more-->

# P2033神秘数字

```c++
#include<bits/stdc++.h>

using namespace std;

int main(){
	long long n,m,ans=0;
	cin>>n>>m;
	//cout<<pow(10,n)<<endl<<pow(10,n+1)<<endl;
	for(int i=pow(10,n-1);i<pow(10,n);i++)
	{
		int num=i,sum=0;
		while(num)
		{
			sum+=num%10;
			num/=10;
		}
		if(sum==m)
		{
			//cout<<i<<endl;
			ans+=i;
		}
	}
	cout<<ans<<endl;
	return 0;
}
```

# P100 快来找一找

```c++
#include<iostream>
#include<cstdio>

using namespace std;

int main(){
	int m,maxnum=0;
	while(cin>>m)
	{
		int tmp;
		for(int i=0;i<m;i++)
		{
			cin>>tmp;
			maxnum=max(maxnum,tmp);
		}
		cout<<maxnum<<endl;
		maxnum=0;
	}
}
```

# P2034 第几个质数

老夫还是不用筛

```c++
#include<bits/stdc++.h>

using namespace std;

int prime[1000+5];

int isprime(int x)
{
	if(prime[x]!=-1)	return prime[x];
	for(int i=2;i<=sqrt(x);i++)
	{
		if(x%i==0)	return prime[x]=0;
	}
	return prime[x]=1;
}

int main(){
	int n,number=1;
	memset(prime, -1, sizeof(prime));
	prime[1]=0;	prime[2]=1;
	while(cin>>n)
	{
		if(!isprime(n))
		{
			cout<<"NO"<<endl;
			continue;
		}
		for(int i=1;i<n;i++)
		{
			if(isprime(i))
			{
				number++;
			}
		}
		cout<<number<<endl;
		number=1;
	}
	return 0;
}
```

# P2039 why的概率论

```c++
#include<bits/stdc++.h>

using namespace std;

int main(){
	int t,x,y;
	cin>>t;
	while(t--)
	{
		cin>>x>>y;
		if(!x)	cout<<"0 / 1"<<endl;
		else
		{
			y+=x;
			x*=4;
			cout<<x/__gcd(x,y)<<" / "<<y/__gcd(x,y)<<endl;
		}
	}
	return 0;
}
```

# P2065 小x爱料理

```c++
#include<bits/stdc++.h>

using namespace std;

int main(){
	int t;
	cin>>t;
	while(t--)
	{
		int u,v,w,a,b,c;
		int n1,n2,n3,ans;
		cin>>u>>v>>w>>a>>b>>c;
		n1=u/a;
		n2=v/b;
		n3=w/c;
		ans=min(n1, min(n2, n3));
		cout<<ans<<endl;
	}
	return 0;
}
```

# 库特的鸽鸽们

```c
#include<bits/stdc++.h>

using namespace std;


const int MAXN=2e5+10;
int n,q,a[MAXN],c[MAXN],p[MAXN],maxn[MAXN];
int main(){
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>a[i];
	}
	cin>>q;
	for(int i=1;i<=q;i++)
	{
		int op,x,y;
		cin>>op;
		if(op==1)
		{
			cin>>x>>y;
			c[x]=y;
			p[x]=i;
		}
		else cin>>maxn[i];
	}
	for(int i=q-1;i>=1;i--)
	{
		maxn[i]=max(maxn[i], maxn[i+1]);
	}
	for(int i=1;i<=n;i++)
	{
		if(p[i])	cout<<max(c[i], maxn[p[i]])<<" ";
		else cout<<max(maxn[1], a[i])<<" ";
	}
		//if(p[n])	cout<<max(c[n], maxn[p[n]]);
		//else cout<<max(maxn[1], a[n]);
	return 0;
}
```

