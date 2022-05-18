---
title: 【每日一题】LeetCode 179.最大数
tags:
  - leetcode
  - acwing
  - 算法
abbrlink: dc1af122
date: 2021-04-12 15:32:01
---

## [LeetCode 179.最大数](https://leetcode-cn.com/problems/largest-number/)

#### 思路

题意大概是给一个数组，然后数组内每个元素不能再拆分，进行排列后得到一个所有排列中数值最大的整数，这里思路比较明确，就是将其从大到小排序后（题目说明可以进行随意组合）逐项加入存储结果的字符串中即可，特别注意的是**在极端情况即数组内含有[0,0]下有可能会出现双重前导0**，对此要特别要进行一次处理。

#### C++代码

```c++
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [](int x,int y)
        {
            auto a =to_string(x), b=to_string(y);
            return a+b>b+a;
        });
        string res;
        for(auto& s: nums)  res+=to_string(s);
        int k=0;
        while(k+1<res.size() && res[k]=='0')    k++;//去除极端情况[0,0]时候出现前导0
        return res.substr(k);
    }
};
```

## [AcWing 1453.移掉K位数字](https://www.acwing.com/problem/content/1455/)

#### 思路

此题与上一题思路比较类似，不过本题求的是移掉指定个数的位之后（不改变原顺序）使得得到的数最小，同时也从数字变成了一个数字。本题需要考虑到的是位越高的数字对数的大小影响最大，所以按照贪心的思路，只需要让高位数字尽可能的小，则最后得到的数一定是所有方案中最小的，因此让其每次都与当前答案的最后一位比较即可，小的就说明当前这个数更适合做高位，即将当前的高位删除。最后还要判断一个特殊情况，**即当每一位都是相同数字时，则去掉任意位的数字即可**，最后依然是要消去前导0，处理后的字符串即为最终结果。
#### C++代码

```c++
#include<iostream>
#include<cstdio>

using namespace std;

int main()
{
    string num;
    int k;
    cin>>num>>k;
    
    string res="0";
    for(auto c:num)
    {
        while(k&&c<res.back())  res.pop_back(), k--;
        res+=c;
    }
    
    while(k--)  res.pop_back();//处理极端情况
    k=0;
    while(k+1 < res.size() && res[k]=='0')  k++;//去除前导0
    cout<<res.substr(k)<<endl;
    return 0;
}
```

