---
title: buu-re-findit
tags:
  - CTF
  - RE
  - buu
abbrlink: 3d2fcb0e
date: 2021-03-10 20:34:37
---

拿到手是一个apk文件，利用Android Killer打开后查看mainActivity里面的相关信息，可以看到一串十六进制数字:`0x70,0x76,0x6b,0x71,0x7b,0x6d,0x31,0x36,0x34,0x36,0x37,0x35,0x32,0x36,0x32,0x30,0x33,0x33,0x6c,0x34,0x6d,0x34,0x39,0x6c,0x6e,0x70,0x37,0x70,0x39,0x6d,0x6e,0x6b,0x32 ,0x38,0x6b,0x37,0x35,0x7d`。写了一个C语言的小程序来把这串数字转换为字符串

```c
#include<iostream>
#include<vector>

using namespace std;

int main()
{
	vector<int> flag={0x70,0x76,0x6b,0x71,0x7b,0x6d,0x31,0x36,0x34,0x36,0x37,0x35,0x32,0x36,0x32,0x30,0x33,0x33,0x6c,0x34,0x6d,0x34,0x39,0x6c,0x6e,0x70,0x37,0x70,0x39,0x6d,0x6e,0x6b,0x32 ,0x38,0x6b,0x37,0x35,0x7d};
	char s[100];
	for(int i =0; i<flag.size();i++)
	{
		s[i]=char(flag[i]);
	}
	cout<<s<<endl;
	return 0;
}
```

得到结果`pvkq{m164675262033l4m49lnp7p9mnk28k75}`，看符号判断应该是被加密过，使用凯撒解密得到结果`flag{c164675262033b4c49bdf7f9cda28a75}`