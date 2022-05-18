---
title: ACM-学习记录-素数筛
tags: ACM
abbrlink: 91338f8a
date: 2020-11-20 20:09:57
---

# 前言

近期发现我NEFU低年级组校赛题目只有模拟+数论，恰恰都是我最不会做的，数论方面反反复复用到的就是素数筛，特在此记录一下，闲来无事自己翻阅当作复习复习，以免被到时候~~一道题都做不出来~~菜到巨佬们。

# 代码

查找2-N的所有素数，如下

```c++
//线性筛
void init() {
  phi[1] = 1;
  for (int i = 2; i < MAXN; ++i) {
    if (!vis[i]) {
      phi[i] = i - 1;
      pri[cnt++] = i;
    }
    for (int j = 0; j < cnt; ++j) {
      if (1ll * i * pri[j] >= MAXN) break;
      vis[i * pri[j]] = 1;
      if (i % pri[j]) {
        phi[i * pri[j]] = phi[i] * (pri[j] - 1);
      } else {
        // i % pri[j] == 0
        // 换言之，i 之前被 pri[j] 筛过了
        // 由于 pri 里面质数是从小到大的，所以 i 乘上其他的质数的结果一定也是
        // pri[j] 的倍数 它们都被筛过了，就不需要再筛了，所以这里直接 break
        // 掉就好了
        phi[i * pri[j]] = phi[i] * pri[j];
        break;
      }
    }
  }
}
```



数论确实是想杀了我- -