---
title: Caser-Pass-Tool1-C++
tags: CTF
abbrlink: 992fe380
date: 2020-10-12 10:48:51
---
刷pico遇到一个凯撒密码加密题，顺手写了个解密工具，但是暂时没有解决到边界字母的升降档问题
<!--more-->
```C++
#include<iostream>

using namespace std;

int main(){
	char s[100];
	int n,k,s2[100];
	cin>>n>>k;
	for(int i=0;i<n;i++)
	{
		cin>>s[i];
		s2[i]=(int)s[i];
	}
	for(int i=0;i<n;i++)
	{
		s2[i]+=k;
		cout<<(char)s2[i];
	}
	return 0;
} 
```
P.S C++大法好