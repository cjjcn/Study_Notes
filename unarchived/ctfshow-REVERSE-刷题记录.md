---
title: ctfshow-REVERSE-刷题记录
tags:
  - ctfshow
  - RE
  - 记录
abbrlink: 1bdb2719
date: 2022-03-03 16:42:41
---

# re2

打开初步分析为32位程序，无壳，拖入IDA32分析

![re2_main](https://image.0error.net/20220303165744.png)

找到关键行47-50，分析得出各函数内部的基本作用如图所示

首先进入分析输入后的处理![re2_401A70](https://image.0error.net/20220303165942.png)

可以很明显看出，是对输入的字符串进行处理后再操作，因此根据这个思路写出脚本，得到这里的密钥

```python
#!/usr/bin/python
#coding=utf-8

user = "DH~mqqvqxB^||zll@Jq~jkwpmvez{"

flag = ''
for x in range(0, 29):
    for y in range(256):
        if(ord(user[x])==y^0x1f):
            flag+=chr(y)

print(flag) #[Warnning]Access_Unauthorized
```

接着分析加密函数，对加密函数内各函数分析后可得出大致为RC4，因此只要根据给出的加密内容进行逆向解密即可

![re2_4014E0](https://image.0error.net/20220303170741.png)

但是这里加密后的是什么内容是什么不清楚，我盲猜是给出的压缩文件里面的文本，但是这里没有科学依据，后来看到一篇大师傅的WP，他的分析是程序在最初会对`enflag.txt`的内容进行`fopen() w`操作，这样会清楚里面的文本，照此来看里面的东西其实没有被用过，因此可以推断他就是要加密的原始文本，因此在同目录下创建`flag.txt`，放入文本，解密成功。

![re2_flag](https://image.0error.net/20220303171146.png)

# r2

打开后得到如下伪代码

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  size_t v3; // rax
  int v5; // [rsp+Ch] [rbp-134h] BYREF
  unsigned int i; // [rsp+10h] [rbp-130h]
  int v7; // [rsp+14h] [rbp-12Ch]
  int v8; // [rsp+18h] [rbp-128h]
  int v9; // [rsp+1Ch] [rbp-124h]
  int v10; // [rsp+20h] [rbp-120h]
  int v11; // [rsp+24h] [rbp-11Ch]
  int v12; // [rsp+28h] [rbp-118h]
  int v13; // [rsp+2Ch] [rbp-114h]
  int v14; // [rsp+30h] [rbp-110h]
  int v15; // [rsp+34h] [rbp-10Ch]
  unsigned __int64 v16; // [rsp+38h] [rbp-108h]
  int v17[8]; // [rsp+40h] [rbp-100h]
  char s[5]; // [rsp+60h] [rbp-E0h] BYREF
  char v19[107]; // [rsp+65h] [rbp-DBh] BYREF
  char dest[104]; // [rsp+D0h] [rbp-70h] BYREF
  unsigned __int64 v21; // [rsp+138h] [rbp-8h]

  v21 = __readfsqword(0x28u);
  v7 = 80;
  v8 = 64227;
  v9 = 226312059;
  v10 = -1540056586;
  v11 = 5;
  v12 = 16;
  v13 = 3833;
  v5 = 0;
  puts("plz input the key:");
  __isoc99_scanf("%s", s);	// 输入key
  v3 = strlen(s);	// 计算输入长度
  strncpy(dest, v19, v3 - 6);	// 将s的6~strlen(s)-1位复制到dest，输入s的长度不小于6
  dest[strlen(s) - 6] = 0;	// 最后1位置0
  __isoc99_sscanf(dest, "%x", &v5);	 // 读取dest中16进制数到v5
  v17[0] = v7;
  v17[1] = v8;
  v17[2] = v9;
  v17[3] = v10;
  v17[4] = (v11 << 12) + v12;
  v17[5] = v13;
  v17[6] = v5;
  v16 = 0LL;
  for ( i = 0; i <= 6; ++i )
  {
    for ( v16 += (unsigned int)v17[i]; v16 > 0xFFFF; v16 = v15 + (unsigned int)(unsigned __int16)v16 )// unsigned __int16只保留后4位
    {
      v14 = (unsigned __int16)v16;	// 没用到v14，这行可以忽略
      v15 = v16 >> 16;
    }
  }
  if ( v16 == 0xFFFF )
    puts("OK");
  else
    puts("Error");
  return 0;
}
```

从47行可见v16分别加上了v17的前6位，因此前6位是保持不变的，根据题意，v17[6]就是所要求的flag，所以这里可以写出如下程序

```c
#include<stdio.h>
int main() {
	int v15 = 0;
	unsigned __int64 v16 = 0LL;
	int v17[7] = {80, 64227, 226312059, -1540056586, 20496, 3833, 888};

	for (int i = 0 ; i <= 6; ++i ) {
		for ( v16 += (unsigned int)v17[i]; v16 > 0xFFFF; v16 = v15 + (unsigned int)(unsigned __int16)v16 )
			v15 = v16 >> 16;
	}

	if ( v16 == 0xFFFF )
		puts("OK");
	else
		puts("Error");

	return 0;
}
```

调试到即将与v17[6]相加的地方

![r2_debug](https://image.0error.net/img/2022/20220308150013.png)

v16先加上v17[6]，然后判断是否大于0xffff，小于等于0xffff即退出循环，由提示最小解，4位值，直接相减就可得出最终结果，最终得到flag即为`flag{1a9f}`

# 数学不及格_re3

用IDA打开，可以看到是一个类似于数学方程问题

![re3_main](https://image.0error.net/img/2022/20220315155940.png)

这里看到`v9`的值是`v4`经过函数`f`处理之后的值，再看一下函数f

![re3_f](https://image.0error.net/img/2022/20220315160104.png)

可以看出f函数是一个斐波那契的处理过程

这里的处理应当为四个判断等式联立求解，划掉v10-v12，得v4+3*v9;又因为v9为v4下标范围内的斐波拉契数列值，因此通过0<v4<200,有爆破可能性，直接遍历爆破v4与v9。（v4=58,v9=591286729879），后面依次求解v10-v12，并把它们转化为字符串，由此可写出python脚本

```python
def f(n):
    li=[]
    v9=0
    li.append(1)
    li.append(1)
    for i in range(2, n):
        li.append(li[i-1] + li[i-2])
        v9=li[i]
    return v9

for i in range(1, 199):
    v4 = i
    v9=f(v4)
    if(v4+3*v9==151381742876+117138004530+155894355749+1349446086540):
        print(v4, v9)
        break

v10 = hex(151381742876-v9)
v11 = hex(117138004530-v9)
v12 = hex(155894355749-v9)

s='666c61677b6e65776265655f686572657d'
flag=''
for i in range(0, len(s), 2):
    flag+=chr(int(s[i:i+2], 16))
print(flag)
```

可得出结果如下

![re3_result](https://image.0error.net/img/2022/20220315160345.png)

得到flag问题得解
