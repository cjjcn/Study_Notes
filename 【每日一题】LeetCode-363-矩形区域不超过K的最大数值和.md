---
title: 【每日一题】LeetCode 363.矩形区域不超过K的最大数值和
tags:
  - leetcode
  - acwing
  - 每日一题
  - 算法
abbrlink: 95003eb
date: 2021-04-22 19:00:28
---
## [LeetCode 363. 矩形区域不超过K的最大数值和](https://leetcode-cn.com/problems/max-sum-of-rectangle-no-larger-than-k/)
#### 思路
将问题转化为一维上的问题，枚举`lo`和`hi`表示当前处理数据的列区间，对于每一个列区间，可以看做一个一维问题。
一维问题可以用前缀和配合二分的方式在 `O(mlog⁡m)` 的时间解决。维护一个有序集合，集合中初始放入 0。每次获取当前位置的前缀和，在集合中二分查找第一个大于等于 `sum - k` 的数字，如果能找到，则更新答案。然后将当前位置的前缀和放入有序集合中。
由此推出的列区间共有n^2个。每个一维问题解决时间为`O(mlogm)`，故总时间复杂度为`O(n^2mlog(m))`
#### C++代码
```c++
class Solution {
public:
    vector<vector<int>> s;

    int get(int x1,int y1,int x2,int y2)
    {
        return s[x2][y2]-s[x1-1][y2]-s[x2][y1-1]+s[x1-1][y1-1];
    }
    int maxSumSubmatrix(vector<vector<int>>& matrix, int k) {
        int n=matrix.size(), m=matrix[0].size();
        s=vector<vector<int>>(n+1, vector<int>(m+1));
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++)
                s[i][j]=s[i-1][j]+s[i][j-1]-s[i-1][j-1]+matrix[i-1][j-1];
        int res=INT_MIN;
        for(int i=1;i<=m;i++)
        {
            for(int j=i;j<=m;j++)
            {
                set<int> S;
                S.insert(0);
                for(int p=1;p<=n;p++)
                {
                    int si=get(1,i,p,j);
                    auto it=S.lower_bound(si-k);
                    if(it!=S.end()) res=max(res, si-*it);
                    S.insert(si);
                }
            }
        }
        return res;
    }
};
```

## [AcWing 3412.邻域均值](https://www.acwing.com/problem/content/3415/)

#### 思路

使用前缀和，得出最后的`s[N][N]`然后进行计算，后面主要返回整体的平均值即可，每次都选出最适合的区域并保存。

#### C++代码

```c++
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

const int N=610;

int n,L,r,t;
int s[N][N];

int get_sum(int x1,int y1,int x2,int y2)
{
    return s[x2][y2]-s[x1-1][y2]-s[x2][y1-1]+s[x1-1][y1-1];
}

int get_cnt(int x1,int y1,int x2,int y2)
{
    return (x2-x1+1)*(y2-y1+1);
}

int main()
{
    cin>>n>>L>>r>>t;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
        {
            int x;
            cin>>x;
            s[i][j]=s[i-1][j]+s[i][j-1]-s[i-1][j-1]+x;
        }
    int res=0;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
        {
            int x1=max(1, i-r), y1=max(1, j-r);
            int x2=min(n, i+r), y2=min(n, j+r);
            if(get_sum(x1,y1,x2,y2)<=t * get_cnt(x1,y1,x2,y2))
                res++;
        }
    cout<<res<<endl;
    return 0;
}
```

