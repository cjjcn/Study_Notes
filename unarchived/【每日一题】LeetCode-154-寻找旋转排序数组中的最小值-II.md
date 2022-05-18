---
title: 【每日一题】LeetCode 154.寻找旋转排序数组中的最小值 II
tags:
  - leetcode
  - acwing
  - 每日一题
abbrlink: b04e1ac
date: 2021-04-09 10:12:49
---

## [LeetCode 154.寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

#### 思路

这道题和前两天的每日一题解法基本一致，就是无论怎样旋转，他在一个分界点前后分别都是单调递增的，而这个分界点就是整个数组的最小值，如图

![img](https://www.acwing.com/media/article/image/2018/05/31/1_92f5550a64-2.png)

其中，最右侧的平行部分的数与最左边平行数相同，这部分不满足二分性质，因此需要去掉（或者加一条相同数取较小序号应该也可以执行，可以自行尝试下）因此，只需要通过二分法找到这个最小分界点即可，为了避免对边界情况的处理，可以在数组长度较小的时候直接线性扫描出最小值（这里选择为5，可以自行调整）

#### C++代码

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l=0, r=nums.size()-1;
        while(l<r && nums[r]==nums[l])  r--;
        if(r-l+1<5)
        {
            int res=INT_MAX;
            for(int x : nums)   res=min(res,x);
            return res;
        }
        if(nums[0]<=nums[r])    return nums[0];
        else
        {
            while(l<r)
            {
                int mid=(l+r+1)/2;
                if(nums[mid]>=nums[0])  l=mid;
                else r=mid-1;
            }
            return nums[r+1];
        }
    }
};
```

## [AcWing 69.数组中数值和下标相等的元素](https://www.acwing.com/problem/content/65/)

#### 思路

> 性质:nums[i]-i >= nums[i-1]-(i-1)

由以上思路即可进行二分，由于题目只要求找到任意一个，所以不需要加任何条件限制，直接解出结果然后再单独对二分点进行判断即可

#### C++代码

```c++
class Solution {
public:
    int getNumberSameAsIndex(vector<int>& nums) {
        int l=0, r=nums.size()-1;
        while(l<r)
        {
            int mid=(l+r)>>1;
            if(nums[mid]-mid>=0)    r=mid;
            else l=mid+1;
        }
        if(nums[r]-r==0)    return r;
        return -1;
    }
};
```

