---
title: ACM-NEFUOJ-汉诺塔问题
tags:
  - ACM
  - NEFUOJ
abbrlink: 2cb26a25
date: 2020-12-19 15:11:33
---

# P200汉诺塔

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
    int n,i;
    long long s[40];
    s[1]=2;
    for(i=2;i<=35;i++)
    s[i]=3*s[i-1]+2;
    while(cin>>n)
        cout<<s[n]<<endl;
    return 0;
} 
```



# P201又见汉诺塔

引用一波qky大佬的思路

> **在题目要求条件下，将n个盘子从左边移动到最右边的最快方法是：**
>
>   **（1）先将n-1个盘子移动到中间**
>
>   **（2）让最大的盘子直接移动到最右边（2步）**
>
>   **（3）最后再让n-1个盘子移动到最右边**
>
> **（1）和（3）最需步数应该是一样的，且因为只有最大的盘子才能放在其他盘子上面，所以计算（1）（3）的步骤数时不需考虑这一特殊要求；**
>
>  **我用near数组来储存（1）（3）所需的步骤，计算需要递归；n个盘子从左边移动到中间步骤如下：**
>
>   **（1）先让n-1个盘子移动到最右端**
>
>   **（2）让最大的盘子移动到中间**
>
>   **（3）让n-1个盘子从右端移动到中间**
>
>  **（1）的话和200题一样的；（3）的话就是递归；**

```c++
#include <bits/stdc++.h>

using namespace std;

long long han[21];

long long near[21];

void prepare(){
    han[1]=2;
    for(int i=2;i<=20;i++)
        han[i]=3*han[i-1]+2;
    near[1]=1;
    for(int i=2;i<=20;i++)
        near[i]=han[i-1]+near[i-1]+1;
}

long long fun(int n){
    if(n==1)
        return 2;
    else
        return 2*near[n-1]+2;
}

int main(){
    prepare();
    int T;
    cin>>T;
    while(T--){
        int n;
        cin>>n;
        cout<<fun(n)<<endl;
    }
    return 0;
}
```

