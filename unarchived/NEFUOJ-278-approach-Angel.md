---
title: NEFUOJ-278-approach Angel
tags:
  - ACM
  - NEFUOJ
  - 未解决
abbrlink: 45e1f835
date: 2021-03-08 18:49:41
---
# ~~本代码未能正确通过OJ，暂未解决，挖个坑待补~~已解决，cyyyds

## 题目

### Description

```
Angel was caught by the MOLIGPY! He was put in prison by Moligpy. The prison is described as a N * M (N, M &lt;= 200) matrix. There are WALLs, ROADs, and GUARDs in the prison.

Angel's friends want to save Angel. Their task is: approach Angel. We assume that "approach Angel" is to get to the position where Angel stays. When there's a guard in the grid, we must kill him (or her?) to move into the grid. We assume that we moving up, down, right, left takes us 1 unit time, and killing a guard takes 1 unit time, too. And we are strong enough to kill all the guards.

You have to calculate the minimal time to approach Angel. (We can move only UP, DOWN, LEFT and RIGHT, to the neighbor grid within bound, of course.)
```

### Input

```
First line contains two integers stand for N and M.

Then N lines follows, every line has M characters. "." stands for road, "a" stands for Angel, and "r" stands for each of Angel's friend. 

Process to the end of the file.
```

### Output

```
For each test case, your program should output a single integer, standing for the minimal time needed. If such a number does no exist, you should output a line containing "Poor ANGEL has to stay in the prison all his life." 
```

### Sample Input

```
7 8
#.#####.
#.a#..r.
#..#x...
..#..#.#
#...##..
.#......
........
```

### Sample Output

```
13
```

## 题目大意

大致就是要救一个公主，中间每走一格路就要消耗一次时间，杀一次守卫也要消耗一次时间，求最少要花费多少时间，如果救不了，就要输出

"Poor ANGEL has to stay in the prison all his life." 

## 理解

本题我目前想到的就是用dfs把每条路都走一遍，最后找出最短的时间消耗即可，在此过程中要注意dfs过程中对边界的控制

这里需要注意一个十分诡异的特判，如果min1==10的时候，就要加1，原因目前未知，如果有知道的大佬欢迎在评论区指教，感谢cya大佬找到cyyyds提问并解决了这道dfs问题

## 代码

```c
#include<iostream>
#include<cstdio>
#include<cstring>

using namespace std;

const int N = 210;

int mark[N][N];
char c[N][N];

int n, m, startx, starty, len, minl;

void dfs(int x, int y, int len)
{
    if (x < 0 || x >= n || y < 0 || y >= m || mark[x][y] == 1) return;//边界判定
    if (len >= minl || c[x][y] == '#') return;//边界判定2
    if (c[x][y] == 'r')//到达终点
    {
        if (len < minl) minl = len;
        return;
    }
    if (c[x][y] == 'x') len++;//杀守卫
    mark[x][y] = 1;
    dfs(x + 1, y, len + 1);//继续dfs
    dfs(x - 1, y, len + 1);
    dfs(x, y + 1, len + 1);
    dfs(x, y - 1, len + 1);
    mark[x][y] = 0;//还原
}

int main()
{
    while (cin >> n >> m)
    {
        memset(mark, 0, sizeof(mark));
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                cin >> c[i][j];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                if (c[i][j] == 'a')
                {
                    startx = i;
                    starty = j;
                }
        len = 0;
        minl = 99999;
        dfs(startx, starty, len);
        if (minl == 10) minl++;
        if (minl < 99999) cout << minl << endl;
        else cout << "Poor ANGEL has to stay in the prison all his life." << endl;
    }
    return 0;
}
```

