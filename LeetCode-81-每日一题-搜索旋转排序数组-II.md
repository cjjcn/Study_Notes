---
title: LeetCode.81-每日一题-搜索旋转排序数组 II
tags:
  - leetcode
  - 每日一题
abbrlink: 2cf5b000
date: 2021-04-07 15:04:50
---

## [LeetCode81 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

本题分析到的最坏情况下的最好复杂度的解法均为线性解法，所以就拿线性解法做了，时间复杂度是O(n)

#### C++代码

```c++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        for(auto &v:nums)
        {
            if(v==target)
                return true;
        }
        return false;
    }
};
```

## [AcWing22 旋转数组的最小数字](https://www.acwing.com/problem/content/20/)

除了最后水平的一段（黑色水平那段）之外，其余部分满足二分性质：竖直虚线左边的数满足nums[i]≥nums[0]；而竖直虚线右边的数不满足这个条件。
分界点就是整个数组的最小值。

所以我们先将最后水平的一段删除即可。

这里还有一段y总的特殊情况的tip

>另外，不要忘记处理数组完全单调的特殊情况：
>
>当我们删除最后水平的一段之后，如果剩下的最后一个数大于等于第一个数，则说明数组完全单调。



#### C++代码

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n=nums.size()-1;
        if(n<0) return -1;
        while(n>0 && nums[n] == nums[0])    n--;
        if(nums[n]>=nums[0])    return nums[0];
        int l=0, r=n;
        while(l<r)
        {
            int mid=l+r>>1;
            if(nums[mid] < nums[0]) r=mid;
            else l=mid+1;
        }
        return nums[r];
    }
};
```

