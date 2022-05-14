---
title: 数据结构-KMP算法的研究
tags:
  - 数据结构
  - KMP
abbrlink: '625e4078'
date: 2021-10-25 11:46:20
---

# KMP是什么

>在[计算机科学](https://zh.wikipedia.org/wiki/计算机科学)中，**Knuth-Morris-Pratt[字符串查找算法](https://zh.wikipedia.org/wiki/字符串查找算法)**（简称为**KMP算法**）可在一个[字符串](https://zh.wikipedia.org/wiki/字符串)`S`内查找一个词`W`的出现位置。一个词在不匹配时本身就包含足够的信息来确定下一个匹配可能的开始位置，此算法利用这一特性以避免重新检查先前配对的[字符](https://zh.wikipedia.org/wiki/字符)。
>
>这个算法由[高德纳](https://zh.wikipedia.org/wiki/高德纳)和[沃恩·普拉特](https://zh.wikipedia.org/w/index.php?title=沃恩·普拉特&action=edit&redlink=1)在1974年构思，同年[詹姆斯·H·莫里斯](https://zh.wikipedia.org/w/index.php?title=詹姆斯·H·莫里斯&action=edit&redlink=1)也独立地设计出该算法，最终三人于1977年联合发表。
>
>——wiki

# KMP匹配的原理是什么

在两个串匹配的过程中，当前一段匹配都相符合的时候，下一个元素匹配失败（两个串的指针分别到了i和j+1），这时候如果直接按照暴力做法返回到串头来重新进行匹配会增加时间复杂度，并未能利用到在两个指针前面一段已经匹配好的串。KMP的作用就是将最大的相同前缀后缀**长度**保存下来，以用来节省匹配时间，如果下一个元素匹配失败，那么可以将最大的相同前缀移到原来后缀的位置，那么相当于这一部分前缀就省去了匹配的过程，极大的降低了复杂度。

# next数组保存什么

next数组既可以理解为保存了当前下标下的相同最大前缀的最后一个元素的下标，也可以理解为保存了在当前下标之前的最大前后缀相同子串的长度，如图所示

![next数组作用图示](http://image.0error.net/202202101149737.png)

# KMP模板

```c++
#include<iostream>

using namespace std;

const int N=100010, M=1000010;

int n,m;
int ne[N];
char s[M], p[N];

int main()
{
    cin>>n>>p+1>>m>>s+1;
    //KMP计算next
    for(int i=2,j=0;i<=n;i++)//next[1]无需计算，必为0
    {
        while(j && p[i]!=p[j+1])    j=ne[j];
        if(p[i]==p[j+1])    j++;
        ne[i]=j;
    }
    //KMP匹配
    for(int i=1, j=0;i<=m;i++)
    {
        while(j && s[i]!=p[j+1])    j=ne[j];
        if(s[i]==p[j+1])    j++;
        if(j==n)
        {
            printf("%d ", i-n);
            j=ne[j];
        }
    }
    return 0;
}
```



