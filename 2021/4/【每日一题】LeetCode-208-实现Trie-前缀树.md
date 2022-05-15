---
title: 【每日一题】面试高频考点 - LeetCode 208.实现Trie(前缀树)
tags:
  - 每日一题
  - leetcode
  - acwing
  - 算法
abbrlink: 2c08cb6b
date: 2021-04-14 16:04:01
---

## [LeetCode 208. 实现Trie(前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

#### 思路

这是一道Trie树的标程题，按照Trie树的构建标准写即可，引用Wiki的一段

>在[计算机科学](https://zh.wikipedia.org/wiki/计算机科学)中，**trie**，又称**前缀树**或**字典树**，是一种有序[树](https://zh.wikipedia.org/wiki/树_(数据结构))，用于保存[关联数组](https://zh.wikipedia.org/wiki/关联数组)，其中的键通常是[字符串](https://zh.wikipedia.org/wiki/字符串)。与[二叉查找树](https://zh.wikipedia.org/wiki/二叉查找树)不同，键不是直接保存在节点中，而是由节点在树中的位置决定。一个节点的所有子孙都有相同的[前缀](https://zh.wikipedia.org/wiki/前缀)，也就是这个节点对应的字符串，而根节点对应[空字符串](https://zh.wikipedia.org/wiki/空字符串)。一般情况下，不是所有的节点都有对应的值，只有叶子节点和部分内部节点所对应的键才有相关的值。
>
>Trie这个术语来自于re**trie**val。根据[词源学](https://zh.wikipedia.org/wiki/词源学)，trie的发明者Edward Fredkin把它读作[/ˈtriː/](https://zh.wikipedia.org/wiki/Help:英語國際音標) "tree"。[[1\]](https://zh.wikipedia.org/wiki/Trie#cite_note-DADS-1)[[2\]](https://zh.wikipedia.org/wiki/Trie#cite_note-Liang1983-2)但是，其他作者把它读作[/ˈtraɪ/](https://zh.wikipedia.org/wiki/Help:英語國際音標) "try"。[[1\]](https://zh.wikipedia.org/wiki/Trie#cite_note-DADS-1)[[2\]](https://zh.wikipedia.org/wiki/Trie#cite_note-Liang1983-2)[[3\]](https://zh.wikipedia.org/wiki/Trie#cite_note-KnuthVol3-3)
>
>在图示中，键标注在节点中，值标注在节点之下。每一个完整的英文单词对应一个特定的整数。Trie可以看作是一个[确定有限状态自动机](https://zh.wikipedia.org/wiki/确定有限状态自动机)，尽管边上的符号一般是隐含在分支的顺序中的。
>
>键不需要被显式地保存在节点中。图示中标注出完整的单词，只是为了演示trie的原理。
>
>trie中的键通常是字符串，但也可以是其它的结构。trie的算法可以很容易地修改为处理其它结构的有序序列，比如一串数字或者形状的排列。比如，**bitwise trie**中的键是一串比特，可以用于表示整数或者内存地址。

##### Trie树的应用

trie树常用于搜索提示。如当输入一个网址，可以自动搜索出可能的选择。当没有完全匹配的搜索结果，可以返回前缀最相似的可能。[[4\]](https://zh.wikipedia.org/wiki/Trie#cite_note-4)

#### C++代码

```c++
class Trie {
public:
    int son[100010][26], ok[100010];
    int idx;
    /** Initialize your data structure here. */
    Trie() {
        memset(son, 0 ,sizeof(son));
        memset(ok,0,sizeof(ok));
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        int p=0;
        for(auto c:word)
        {
            int u=c-'a';
            if(!son[p][u])  son[p][u]=++idx;
            p=son[p][u];
        }
        ok[p]=1;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        int p=0;
        for(auto c:word)
        {
            int u=c-'a';
            if(!son[p][u])  return false;
            p=son[p][u];
        }
        return ok[p];
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        int p=0;
        for(auto c:prefix)
        {
            int u=c-'a';
            if(!son[p][u])  return false;
            p=son[p][u];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

## [AcWing 142.前缀统计](https://www.acwing.com/problem/content/144/)

#### 思路

Trie树的变种应用

#### C++代码

```c++
#include<iostream>


using namespace std;
const int N=1000010;

int n,m;
int son[N][26], cnt[N], idx;
char str[N];

void insert(char *str)
{
    int p=0;
    for(int i=0;str[i];i++)
    {
        int u=str[i]-'a';
        if(!son[p][u])  son[p][u]=++idx;
        p=son[p][u];
    }
    cnt[p]++;
}

int query(char *str)
{
    int p=0, res=0;
    for(int i=0;str[i];i++)
    {
        int u=str[i]-'a';
        if(!son[p][u])  break;
        p=son[p][u];
        res+=cnt[p];
    }
    return res;
}

int main()
{
    cin>>n>>m;
    while(n--)
    {
        cin>>str;
        insert(str);
    }
    while(m--)
    {
        cin>>str;
        cout<<query(str)<<endl;
    }
    return 0;
}
```

