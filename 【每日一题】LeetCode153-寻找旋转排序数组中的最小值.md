---
title: 【每日一题】LeetCode153.寻找旋转排序数组中的最小值
tags:
  - leetcode
  - 每日一题
  - acwing
abbrlink: 76256c75
date: 2021-04-08 18:25:01
---
## LeetCode153.寻找旋转排序数组中的最小值
#### 思路
这道题的大概意思是将数组进行顺序（划重点）变换以后，要求最终的最小的元素，题目指出的数据量只有5000，那么直接线性搜索应该也可以过，但是这道题用二分的思路来考虑的话会更有趣一些。
先看看体面中的例子
> 数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

由题中给出的例子来看，*这个数组不论旋转多少次，都存一个一个中间点，这个中间点之前和之后的数据都是有序的*，而这个中间点在原始状态下是在整个数组的最前面的，这时候整个数组都是有序的(特判要点)，而在朴素情况下，这个中间点就是指的二分点，所以此题得解
#### C++代码
```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        if(nums.back() > nums[0])   return nums[0];
        int l=0,r=nums.size()-1;
        while(l<r)
        {
            int mid=(l+r)>>1;
            if(nums[mid]>=nums[0])  l=mid+1;
            else r=mid;
        }
        return nums[l];
    }
};
```
## AcWing68.0到n-1中缺失的数字
#### 思路
这里看过题解以后有个巧解思路，即在整个数组有序的前提下，缺失一组数之后在这之后的数与其序号的差就会增大1个，以此来作为二分的判断条件，此题即可得解
#### C++代码
```c++
class Solution {
public:
    int getMissingNumber(vector<int>& nums) {
        if(nums.empty())    return 0;
        int l=0,r=nums.size()-1;
        while(l<r)
        {
            int mid=(l+r)>>1;
            if(nums[mid]!=mid)  r=mid;
            else l=mid+1;
        }
        if(nums[r]==r)  r++;
        return r;
    }
};
```