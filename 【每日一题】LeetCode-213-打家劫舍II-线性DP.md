---
title: 【每日一题】LeetCode 213.打家劫舍II - 线性DP
tags:
  - leetcode
  - acwing
  - 每日一题
  - 算法
abbrlink: cdd03afb
date: 2021-04-15 18:12:19
---
## [LeetCode 213.打家劫舍II](https://leetcode-cn.com/problems/house-robber-ii/)
#### 思路
这道题有一道前置题目，即[LeetCode 198.打家劫舍](https://leetcode.com/problems/house-robber/description/)，这道前置题目没有环形，是一道常规的线性DP，在每一步都分别判定选和不选的状态即可，而到了本题内，因为是环形的，那么在起始点（终点）的地方就要注意一下，这里起点终点相互影响，所以我们要用分支结构控制一下每个分支的计算的房子，防止少判断
#### C++代码
```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        if(n==0)   return 0;
        if(n==1)    return nums[0];
        if(n==2)    return max(nums[0], nums[1]);

        int ans=0, f, g;
        f=nums[2]; g=0;
        for(int i=3;i<n;i++)
        {
            int lastf=f,lastg=g;
            f=lastg+nums[i];
            g=max(lastf, lastg);
        }
        ans=g+nums[0];

        f=nums[1];  g=0;
        for(int i=2; i<n;i++)
        {
            int lastf=f, lastg=g;
            f=lastg+nums[i];
            g=max(lastf, lastg);
        }    
        ans=max(ans, max(f,g));
        return ans;
    }
};
```
## [AcWing 1055.股票买卖](https://www.acwing.com/problem/content/1057/)
#### 思路I
这道题在蓝桥杯选拔校赛中出现过，贪心的思路也很简单，判断每天比前一天是涨还是跌就好了，如果明天股票会跌那么今天全部卖掉，如果明天股票涨那么就要大量买入，得到的结果一定是最优的。
#### C++代码I
```c++
#include<iostream>

using namespace std;

const int N=1e5+10;
const int INF=0x3f3f3f;

int n;
int a[N];

int main()
{
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>a[i];
    int res=0;
    for(int i=1;i<=n;i++)
        if(a[i]-a[i-1]>0)   res+=a[i]-a[i-1];
    cout<<res<<endl;
    return 0;
}
```
#### 思路II
这道题正解应该使用DP来实现的，分析下来一共有两个状态要推：
1. 当前处于`未持股状态`：
对应的转换有买入和不买入两种处理
2. 当前处于`持股状态`：
对应的转换状态就有卖出和不卖出
由此就可以推出状态转移方程。
#### C++代码II
```c++
#include <iostream>
using namespace std;
const int N = 1e5 + 10, INF = 0x3f3f3f;
int n;
int w[N];
int f[N][2];

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) scanf("%d", &w[i]);

    f[0][1] = -INF;
    for (int i = 1; i <= n; ++i) {
        f[i][0] = max(f[i - 1][0], f[i - 1][1] + w[i]);
        f[i][1] = max(f[i - 1][1], f[i - 1][0] - w[i]);
    }
    printf("%d\n", f[n][0]);
    return 0;
}
```