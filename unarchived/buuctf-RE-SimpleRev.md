---
title: buuctf-RE-SimpleRev
tags:
  - CTF
  - RE
  - buu
abbrlink: 5276d88d
date: 2020-12-22 21:17:43
---

# IDA 打开

将main函数反编译为C代码

```c++
 1 int __cdecl __noreturn main(int argc, const char **argv, const char **envp)
 2 {
 3   int v3; // eax
 4   char v4; // [rsp+Fh] [rbp-1h]
 5 
 6   while ( 1 )
 7   {
 8     while ( 1 )
 9     {
10       printf("Welcome to CTF game!\nPlease input d/D to start or input q/Q to quit this program: ", argv, envp);
11       v4 = getchar();
12       if ( v4 != 'd' && v4 != 'D' )
13         break;
14       Decry();
15     }
16     if ( v4 == 'q' || v4 == 'Q' )
17       Exit();
18     puts("Input fault format!");
19     v3 = getchar();
20     putchar(v3);
21   }
22 }
```

# 代码分析

这道题的关键在于Decry()

> 1 unsigned __int64 Decry()
>  2 {
>  3   char v1; // [rsp+Fh] [rbp-51h]
>  4   int v2; // [rsp+10h] [rbp-50h]
>  5   int v3; // [rsp+14h] [rbp-4Ch]
>  6   int i; // [rsp+18h] [rbp-48h]
>  7   int v5; // [rsp+1Ch] [rbp-44h]
>  8   char src[8]; // [rsp+20h] [rbp-40h]
>  9   __int64 v7; // [rsp+28h] [rbp-38h]
> 10   int v8; // [rsp+30h] [rbp-30h]
> 11   __int64 v9; // [rsp+40h] [rbp-20h]
> 12   __int64 v10; // [rsp+48h] [rbp-18h]
> 13   int v11; // [rsp+50h] [rbp-10h]
> 14   unsigned __int64 v12; // [rsp+58h] [rbp-8h]
> 15 
> 16   v12 = __readfsqword(0x28u);
> 17   *(_QWORD *)src = 'SLCDN';
> 18   v7 = 0LL;
> 19   v8 = 0;
> 20   v9 = 'wodah';
> 21   v10 = 0LL;
> 22   v11 = 0;
> 23   text = join(key3, (const char *)&v9);         // text = 'killshadow'
> 24   strcpy(key, key1);
> 25   strcat(key, src);                             // key = 'ADSFKNDCLS'
> 26   v2 = 0;
> 27   v3 = 0;
> 28   getchar();
> 29   v5 = strlen(key);                             // v5 = 10
> 30   for ( i = 0; i < v5; ++i )
> 31   {
> 32     if ( key[v3 % v5] > 64 && key[v3 % v5] <= 90 )// key = 'adsfkndcls'
> 33       key[i] = key[v3 % v5] + 32;
> 34     ++v3;
> 35   }
> 36   printf("Please input your flag:", src);
> 37   while ( 1 )
> 38   {
> 39     v1 = getchar();
> 40     if ( v1 == 10 )
> 41       break;
> 42     if ( v1 == 32 )
> 43     {
> 44       ++v2;
> 45     }
> 46     else
> 47     {
> 48       if ( v1 <= 96 || v1 > 122 )
> 49       {
> 50         if ( v1 > 64 && v1 <= 90 )              // 大写字母
> 51           str2[v2] = (v1 - 39 - key[v3++ % v5] + 97) % 26 + 97;
> 52       }
> 53       else                                      // 小写字母
> 54       {
> 55         str2[v2] = (v1 - 39 - key[v3++ % v5] + 97) % 26 + 97;
> 56       }
> 57       if ( !(v3 % v5) )
> 58         putchar(' ');
> 59       ++v2;
> 60     }
> 61   }
> 62   if ( !strcmp(text, str2) )
> 63     puts("Congratulation!\n");
> 64   else
> 65     puts("Try again!\n");
> 66   return __readfsqword(0x28u) ^ v12;
> 67 }

转为C语言逆向解出结果

```c
#include<stdio.h> 
int main()
{
	char key[] = "adsfkndcls";
	char text[] = "killshadow";
	int i;
	int v3=10;//长度 
	for (int i = 0; i < 10; i++)
	{
		for (int j = 0; j < 128; j++)
		{
			if (j < 'A' || j > 'z' || j > 'Z' && j < 'a')
			{
				continue;
			}
			if ((j - 39 - key[v3 % 10] + 97) % 26 + 97 == text[i])
			{
				printf("%c",j);
				v3++;
				break;
			}
		}
	}
}
```

得到flag