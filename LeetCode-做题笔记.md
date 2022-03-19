---
title: LeetCode - 做题笔记
tags:
  - leetcode
  - 每日一题
abbrlink: '2e627859'
date: 2022-03-03 15:03:41
---

这篇博客集中整理在LeetCode的刷题记录，方便查阅

# [258. 各位相加 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/add-digits/)

## 代码

```c++
class Solution {
public:
    int addDigits(int num) {
        int ans=999999;
        while(ans >= 10)
        {
            ans=0;
            while(num!=0)
            {
                ans = ans + num%10;
                num/=10;
            }
            num=ans;
        }
        return ans;
    }
};
```

# [401. 二进制手表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-watch/)

## 题目

二进制手表顶部有 4 个 LED 代表 小时（0-11），底部的 6 个 LED 代表 分钟（0-59）。每个 LED 代表一个 0 或 1，最低位在右侧。

例如，下面的二进制手表读取 "3:25" 。

给你一个整数 turnedOn ，表示当前亮着的 LED 的数量，返回二进制手表可以表示的所有可能时间。你可以 按任意顺序 返回答案。

小时不会以零开头：

例如，"01:00" 是无效的时间，正确的写法应该是 "1:00" 。
分钟必须由两位数组成，可能会以零开头：

例如，"10:2" 是无效的时间，正确的写法应该是 "10:02" 。


示例 1：

输入：turnedOn = 1
输出：["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]
示例 2：

输入：turnedOn = 9
输出：[]


提示：

0 <= turnedOn <= 10

## 思路

思路比较直接，就是枚举出这个二进制数的每一位，然后分别判定是否符合时间单位的标准，如果是的话就保存下来输出即可，主要还是要学习二进制位运算的相关操作。

## C++代码

```c++
class Solution {
public:
    vector<string> readBinaryWatch(int num) {
        vector<string> res;
        char str[10];
        for(int i=0;i<1<<10;i++)//枚举每一位
        {
            int s=0;
            for(int j=0;j<10;j++)
            {
                if(i>>j & 1)//如果i的第j位是1
                    s++;
            }
            if(s==num)
            {
                int a=i>>6, b=i&63;//63=各个位均为1,a取了i的小时(最高的四位)
                if(a<12 && b<60)
                {
                    sprintf(str, "%d:%02d", a, b);
                    res.push_back(str);
                }
            }
        }
        return res;
    }
};
```

# [2104. 子数组范围和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/sum-of-subarray-ranges/)

## 思路

枚举计算出每个子数组之间最大值与最小值的差值并加在最终结果中即可

## 代码

```c++
class Solution {
public:
    long long subArrayRanges(vector<int>& nums) {
        int n = nums.size();
        long long res=0;
        for(int i=0;i<n;i++)
        {
            int minVal=INT_MAX, maxVal=INT_MIN;
            for(int j=i;j<n;j++)
            {
                minVal = min(minVal, nums[j]);
                maxVal = max(maxVal, nums[j]);
                res+=maxVal - minVal;
            }
        }
        return res;
    }
};
```

# [521. 最长特殊序列 Ⅰ - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/longest-uncommon-subsequence-i/)

## 思路

究竟贪心题，如果两者长度相等，那么一定没有特殊串，如果长度不等，那么长的一串一定不会不特殊（短的再怎么删也不会变长）

## 代码

```c++
class Solution {
public:
    int findLUSlength(string a, string b) {
        if(a==b)    return -1;
        return max(a.size(), b.size());
    }
};
```

# [504. 七进制数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/base-7/)

## 代码

```c++
class Solution {
public:
    string convertToBase7(int num) {
        if(!num)    return "0";
        bool is_neg = num < 0;
        num = abs(num);
        string res;
        while(num)
            res+= to_string(num%7), num/=7;
        reverse(res.begin(), res.end());
        if(is_neg)  
            res = '-'+res;
        return res;
    }
};
```

# [LeetCode 2049. 统计最高分的节点数目](https://leetcode-cn.com/problems/count-nodes-with-the-highest-score/)

## 思路

在一棵树中，当把一个节点和与它相连的所有边删除，剩余部分最多是三棵非空子树，即原节点的左子树，右子树，以及把以这个节点为根结点的子树移除所形成的子树（除根结点外均有），最少数量则无此类特性。而题目中提到各节点的分数为这些子树的**节点**个数的乘积。我们可以先用 `parents` 数组统计出每个节点的子节点，然后使用深度优先搜索来计算以每个节点为根结点的子树的大小，同时计算每个节点的大小，作为深度优先搜索的返回值，最后统计最大分数出现的次数。在实现上，统计最大分数出现的次数可以放到深度优先搜索中完成，从而节省一部分空间。

## 代码

```c++
class Solution {
public:
    long long n;
    long long maxScore=0;
    long long cnt=0;
    vector<vector<int>> children;

    int dfs(int node)
    {
        long long size = n-1;
        long long score = 1;
        for(int c:children[node])
        {
            int t = dfs(c);
            score *= t;
            size -= t;//算出节点大小，让size减去他

        }
        if(node != 0)
        {
            score *= size;
        }
        if(score == maxScore)
            cnt++;
        else if(score > maxScore)
        {
            maxScore = score;
            cnt=1;
        }
        return n-size;
    }

    int countHighestScoreNodes(vector<int>& parents) {
        this->n = parents.size();
        this->children = vector<vector<int>> (n);
        for(int i=0;i<n;i++)
        {
            int p = parents[i];
            if(p!=-1)
            {
                children[p].emplace_back(i);//记录p节点的儿子节点
            }
        }
        dfs(0);
        return cnt;
    }
};
```

# [LeetCode 599. 两个列表的最小索引总和](https://leetcode-cn.com/problems/minimum-index-sum-of-two-lists/)

## 思路

利用哈希表存储后进行字符串索引查找，同时通过`map`记录每个哈希值的索引数值大小

## 代码

```c++
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        unordered_map<string, int> hash;
        for(int i=0;i<list1.size();i++)
            hash[list1[i]] = i;
        int sum = INT_MAX;
        vector<string> res;
        for(int i=0;i<list2.size();i++)
        {
            string& s = list2[i];
            if(hash.count(s))
            {
                int k = hash[s] + i;
                if(k < sum)
                {
                    sum = k;
                    res = {s};
                }
                else if(k==sum)
                {
                    res.push_back(s);
                }
            }
        }
        return res;
    }
};
```

# [2044. 统计按位或能得到最大值的子集数目 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/count-number-of-maximum-bitwise-or-subsets/)

## 思路

爆搜思路，对于任意一位 xx 而言，都有选和不选两种选择，分别对应了`dfs(u + 1, val | nums[x])`和`dfs(u + 1, val)`两条搜索路径，在搜索所有状态过程中，使用全局变量`max`和`ans`来记录最大得分以及取得最大得分的状态数量。

## 代码

```c++
class Solution {
public:
    vector<int> nums;
    int ans=0, max=0;
    int countMaxOrSubsets(vector<int>& _nums) {
        nums = _nums;
        dfs(0,0);
        return ans;
    }

    void dfs(int u, int val)
    {
        if(u == nums.size())
        {
            if(val > max)   
            {
                max = val;
                ans = 1;
            }
            else if(val == max) ans++;
            return;
        }
        dfs(u+1, val | nums[u]);
        dfs(u+1, val);
    }
};
```

# [720. 词典中最长的单词 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/longest-word-in-dictionary/)

## 思路

先将输入的`words`进行排序，规则为先按长度升序排序，在长度相同情况下再按照字典序降序排序，这样就可以保证我们在后面遍历每个单词时，比该单词短的全部~~子单词~~已全部遍历过，且每次遇到符合要求的单词一定是最长且字典序最小的单词，可以直接将其更新为答案。

## 代码

```c++
class Solution {
public:
    string longestWord(vector<string>& words) {
        sort(words.begin(), words.end(), [](const string &a, const string &b){
            return a.size()!=b.size()?a.size()<b.size():a>b;
        });
        string longest;
        unordered_set<string> candidates = {""};
        for(auto &word : words)
        {
            if(candidates.count(word.substr(0, word.size() - 1)))
            {
                longest = word;
                candidates.emplace(word);
            }
        }
        return longest;
    }
};
```

# [AcWing 606. 根据二叉树创建字符串](https://leetcode-cn.com/problems/construct-string-from-binary-tree/)
## 思路
递归方式遍历，如果确实存在左儿子右儿子则带上括号递归，如果不存在则直接返回当前节点数值即可，思路很简单
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
class Solution {
public:
    string tree2str(TreeNode* root) {
        if(root == nullptr) return "";
        if(root->left == nullptr && root->right == nullptr)
        {
            return to_string(root->val);
        }
        else if(root->right == nullptr)
        {
            return to_string(root->val) + "(" + tree2str(root->left) + ")";
        }
        else
        {
            return to_string(root->val) + "(" + tree2str(root->left) + ")("+ tree2str(root->right) + ")";
        }
    }
};
```
