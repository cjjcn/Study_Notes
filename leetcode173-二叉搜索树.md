---
title: leetcode173-二叉搜索树
tags:
  - leetcode
  - 树
abbrlink: 8f7221e8
date: 2021-03-31 20:20:54
---

## 题目

实现一个二叉搜索树迭代器类BSTIterator ，表示一个按中序遍历二叉搜索树（BST）的迭代器：
BSTIterator(TreeNode root) 初始化 BSTIterator 类的一个对象。BST 的根节点 root 会作为构造函数的一部分给出。指针应初始化为一个不存在于 BST 中的数字，且该数字小于 BST 中的任何元素。
boolean hasNext() 如果向指针右侧遍历存在数字，则返回 true ；否则返回 false 。
int next()将指针向右移动，然后返回指针处的数字。
注意，指针初始化为一个不存在于 BST 中的数字，所以对 next() 的首次调用将返回 BST 中的最小元素。

你可以假设 next() 调用总是有效的，也就是说，当调用 next() 时，BST 的中序遍历中至少存在一个下一个数字。

![img](https://i.loli.net/2021/03/31/QEB5IZwK3kiNu6o.png)

示例：

输入
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
输出
[null, 3, 7, true, 9, true, 15, true, 20, false]

解释
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // 返回 3
bSTIterator.next();    // 返回 7
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 9
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 15
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 20
bSTIterator.hasNext(); // 返回 False


提示：

树中节点的数目在范围 [1, 105] 内
0 <= Node.val <= 106
最多调用 105 次 hasNext 和 next 操作


进阶：

你可以设计一个满足下述条件的解决方案吗？next() 和 hasNext() 操作均摊时间复杂度为 O(1) ，并使用 O(h) 内存。其中 h 是树的高度。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-search-tree-iterator

## 思路

正好遇到这道题，梳理一下二叉树中序遍历的基本思路，即从先将所有的左子树从根节点开始全部压入一个栈中，然后进行中序遍历时先将当前节点输出，然后检查该节点是否有右子树，如果有的话就把右子树压入栈中，按照树结构建立的原理（即先左后右）此节点也必有左子树，只要把左子树输出后继续取出栈顶元素操作的步骤即可，这样遍历出来的树结构就是中序遍历后树结构

![img](https://i.loli.net/2021/03/31/QpqIVmg7b6ZRFPl.png)

## 代码

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
class BSTIterator {
public:
    stack<TreeNode*> stk;
    BSTIterator(TreeNode* root) {
        while(root){
            stk.push(root);
            root=root->left;
        }
    }
    
    int next() {
        auto root=stk.top();
        stk.pop();
        int val=root->val;
        root=root->right;
        while(root){
            stk.push(root);
            root=root->left;
        }
        return val;
    }
    
    bool hasNext() {
        return stk.size();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

