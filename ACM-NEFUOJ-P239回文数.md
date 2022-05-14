---
title: ACM-NEFUOJ-P239回文数
tags:
  - ACM
abbrlink: bf195a13
date: 2020-12-20 12:58:12
---

```c++
#include<bits/stdc++.h>

using namespace std;

int n,p[1000],len,p1[1000];
int f()
{
	int i;
	for(i=0;i<=len;i++)
		if(p[i]!=p[len-i]) return 0;
	return 1;
}
void f1()
{
	int i;
	for(i=0;i<=len;i++)  p1[i]=p[len-i];
	for(i=0;i<=len;i++)  p[i]+=p1[i];	 
	for(i=0;i<=len;i++)
	    p[i+1]+=p[i]/n, p[i]=p[i]%n; 
	if(p[len+1]>0) len++;
}
int main()
{
	string s;
	int i;
    while(cin>>n>>s)
	{
		memset(p,0,sizeof(p));
		len=s.size()-1;
        for(i=len;i>=0;i--)
		{
			if(s[i]>='0' && s[i]<='9') p[len-i]=s[i]-'0';
			if(s[i]>='A')  p[len-i]=s[i]-'A'+10;
		}
		if(f())  {cout<<'0'<<endl; continue;}
		int t=0;
		while(t<=30)
		{
			t++;
			f1();
			if(f()) { cout<<t<<endl; break; }
		}
		if(t>30)  cout<<"Impossible"<<endl;
	}
	return 0;
}
```

