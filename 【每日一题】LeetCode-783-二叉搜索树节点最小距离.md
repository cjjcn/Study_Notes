---
title: 【每日一题】LeetCode 783.二叉搜索树节点最小距离
tags:
  - leetcode
  - acwing
  - 树
abbrlink: dbf62d11
date: 2021-04-13 12:14:46
---

## [LeetCode 783.二叉搜索树节点最小距离](https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/)

#### 思路

今天的题都比较无脑，直接无脑dfs搜索最小差值，暴力搜索一遍得出的最后结果就是最小值

#### C++代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int ans, last;
    bool is_first;
    int minDiffInBST(TreeNode* root) {
        ans=INT_MAX, is_first=true;
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root)
    {
        if(!root)   return;
        dfs(root->left);
        if(is_first)
        {
            last=root->val;
            is_first=false;
        }
        else
        {
            ans=min(ans, root->val-last);
            last=root->val;
        }
        dfs(root->right);
    }
};
```

## [AcWing 71.二叉树的深度](https://www.acwing.com/problem/content/67/)

#### 思路

无脑递归

#### C++代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int treeDepth(TreeNode* root) {
        if(!root)   return 0;
        return max(treeDepth(root->left), treeDepth(root->right))+1;
    }
};
```





