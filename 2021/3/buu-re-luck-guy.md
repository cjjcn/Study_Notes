---
title: buu-re-luck_guy
tags:
  - CTF
  - RE
  - buu
abbrlink: 6c2d8964
date: 2021-03-10 15:51:56
---

拿到后使用IDA64打开后，发现有main函数，进入后反编译为伪代码

```
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v4; // [rsp+14h] [rbp-Ch]
  unsigned __int64 v5; // [rsp+18h] [rbp-8h]

  v5 = __readfsqword(0x28u);
  welcome(*(_QWORD *)&argc, argv, envp);
  puts("_________________");
  puts("try to patch me and find flag");
  v4 = 0;
  puts("please input a lucky number");
  __isoc99_scanf("%d", &v4);
  patch_me(v4);
  puts("OK,see you again");
  return 0;
}
```



看到一个负责对比flag的patch_me函数，进入这个函数，看到如下

```
int __fastcall patch_me(int a1)
{
  int result; // eax

  if ( a1 % 2 == 1 )
    result = puts("just finished");
  else
    result = get_flag();
  return result;
}
```

很显然，get_flag就是获取flag的函数，进入这个函数

```
unsigned __int64 get_flag()
{
  unsigned int v0; // eax
  char v1; // al
  signed int i; // [rsp+4h] [rbp-3Ch]
  signed int j; // [rsp+8h] [rbp-38h]
  __int64 s; // [rsp+10h] [rbp-30h]
  char v6; // [rsp+18h] [rbp-28h]
  unsigned __int64 v7; // [rsp+38h] [rbp-8h]

  v7 = __readfsqword(0x28u);
  v0 = time(0LL);
  srand(v0);
  for ( i = 0; i <= 4; ++i )
  {
    switch ( rand() % 200 )
    {
      case 1:
        puts("OK, it's flag:");
        memset(&s, 0, 0x28uLL);
        strcat((char *)&s, f1);
        strcat((char *)&s, &f2);
        printf("%s", &s);
        break;
      case 2:
        printf("Solar not like you");
        break;
      case 3:
        printf("Solar want a girlfriend");
        break;
      case 4:
        v6 = 0;
        s = 9180147350284624745LL;
        strcat(&f2, (const char *)&s);
        break;
      case 5:
        for ( j = 0; j <= 7; ++j )
        {
          if ( j % 2 == 1 )
            v1 = *(&f2 + j) - 2;
          else
            v1 = *(&f2 + j) - 1;
          *(&f2 + j) = v1;
        }
        break;
      default:
        puts("emmm,you can't find flag 23333");
        break;
    }
  }
```

可以看到整个处理的case是在1-199之间随机生成的，处理后发现真正对串进行过处理的只有case1/4/5，其中字符串f1已经有值了，回溯过去内容为`GXY{do_not_`，f2的处理是对内容串进行了一系列操作，可以直接转化为C程序

```c
#include<iostream>

using namespace std;

int main()
{
	char f2[]={0x69, 0x63, 0x75, 0x67, 0x60, 0x6f, 0x66, 0x7f, 0};
	for(int i=0;i<8;i++)
	{
		if(i%2==1)
			f2[i]-=2;
		else
			f2[i]-=1;
	}
	char f1[]="GXY{do_not_";
	char flag[20];
	memset(flag, 0, 20);
	strcat(flag, f1);
	strcat(flag, f2);
	puts(flag);
	return 0;
}
```

运行后得到flag`GXY{do_not_hate_me}`

