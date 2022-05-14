---
title: acwing1497-树的遍历
tags:
  - ACM
  - 树
  - acwing
abbrlink: a9cc3b0f
date: 2021-03-27 12:24:52
---

## 题目

一个二叉树，树中每个节点的权值互不相同。

现在给出它的后序遍历和中序遍历，请你输出它的层序遍历。

#### 输入格式

第一行包含整数 NN，表示二叉树的节点数。

第二行包含 NN 个整数，表示二叉树的后序遍历。

第三行包含 NN 个整数，表示二叉树的中序遍历。

#### 输出格式

输出一行 NN 个整数，表示二叉树的层序遍历。

#### 数据范围

1≤N≤301≤N≤30

#### 输入样例：

```
7
2 3 1 5 7 6 4
1 2 3 4 5 6 7
```

#### 输出样例：

```
4 1 6 3 5 7 2
```

## 代码

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <unordered_map>
#include <queue>

using namespace std;

const int N = 40;

int n;
int postorder[N], inorder[N];
unordered_map<int, int> l, r, pos;

int build(int il, int ir, int pl, int pr)
{
    int root = postorder[pr];
    int k = pos[root];
    if (il < k) l[root] = build(il, k - 1, pl, pl + k - 1 - il);
    if (ir > k) r[root] = build(k + 1, ir, pl + k - 1 - il + 1, pr - 1);
    return root;
}

void bfs(int root)
{
    queue<int> q;
    q.push(root);
    while (q.size())
    {
        auto t = q.front();
        q.pop();
        cout << t << ' ';
        if (l.count(t)) q.push(l[t]);
        if (r.count(t)) q.push(r[t]);
    }
}


int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> postorder[i];
    for (int i = 0; i < n; i ++ )
    {
        cin >> inorder[i];
        pos[inorder[i]] = i;
    }
    int root = build(0, n - 1, 0, n - 1);
    bfs(root);
    return 0;
}
```

