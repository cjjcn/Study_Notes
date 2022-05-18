---
title: ACM-NEFU15届校赛-大一组
tags:
  - ACM
  - NEFU
abbrlink: ba287589
date: 2020-12-09 17:30:22
---

# A. 三角形面积

```c
#include <bits/stdc++.h>

using namespace std;

int main()
{
    double a,b,c;
    double ans,p,tmp;
    cin>>a>>b>>c;
    p=(a+b+c)*0.5;
    tmp=p*(p-a)*(p-b)*(p-c);
    ans=sqrt(tmp);
    printf("%.1lf", ans);
    return 0;
}
```

<!--more-->

# B. 最大质因子

> ***唯一分解定理***
>
> 唯一分解定理又称为算数基本定理，基本内容是：
>
> 每个大于1的自然数，要么本身就是质数，要么可以写为2个或以上的质数的积，而且这些质因子按大小排列之后，写法仅有一种方式。
>
> 用另一种方法表示就是：
>
> 对于任何一个大于1的正整数,都存在一个标准的分解式: N=p1^a1 * p2^a2*···*pn^an;（其中一系列an为指数，pn为质数）
>
> 此定理表明：任何一个大于 1 的正整数都可以表示为素数的积。

然而这道题纯暴力就可解...

```c
#include <bits/stdc++.h>

using namespace std;

const int maxn=1e5+10;

bool judge(int a)
{
    int flag=1;
    for (int i=2;i*2<=a;i++)
     if (a%i==0) {flag=0; break; }
    if (a==1) return 0;
     else return flag;
}

int main()
{
    int a;
   while(cin>>a)
   {
       for (int i=a;i>=1;i--)
       {
           if (a%i==0)
            if (judge(i))

           {
               cout<<i<<endl;
               break;
           }
       }
   }

    return 0;

}
```



# C.杨辉三角

模板例题

```c
#include<bits/stdc++.h>

using namespace std;

int a[21][21];

int main()
{
	memset(a, 0, sizeof(a));
	a[1][1]=1;
	a[2][1]=a[2][2]=1;
	int n;
	cin>>n;
	for(int i=3;i<=n;i++)//行 
	{
		for(int j=1;j<=i;j++)
		{
			if(j==1 || j==i)	
			{
				a[i][j]=1;continue;
			}
			a[i][j]=a[i-1][j-1]+a[i-1][j];
		}
	}
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=i;j++)
		{
			cout<<a[i][j]<<" ";
		}
		cout<<endl;
	}
	return 0;
}
```

# D."nefu"的数目

```c
#include<iostream>
#include<cstdio>
#include<cstring>

using namespace std;

string s;

int scount(int p)
{
	int sum=0;
	int len=s.length();
	for(int i=p+1;i<len;i++)
	{
		if(s[i]=='e')
		{
			for(int j=i+1;j<len;j++)
			{
				if(s[j]=='f')
				{
					for(int k=j+1;k<len;k++)
					{
						if(s[k]=='u')	sum++;
						//cout<<sum<<endl;
					}
				}
			}
		}
	}
	return sum;
}

int main()
{
	int ans=0,flag=0;
	cin>>s;
	int len=s.length();
	//cout<<len;
	for(int i=0;i<len;i++)
	{
		if(s[i]=='n')
		{
		    ans+=scount(i);
		}
            //cout<<ans<<endl;
	}
	cout<<ans<<endl;
	return 0;
}
```



# E. 最少修改次数(1)

```c
#include <bits/stdc++.h>

using namespace std;

const int maxn=2e5+10;

int main()
{
 string s,t;
   while(cin>>s)
   {
       cin>>t;
       int ct=0;
       int nums=s.size(),numt=t.size();
       int min=1111;
       for (int i=0;i<=nums-numt;i++)
       {
           int j=0;
           ct=0;
           for (int k=i;k<=i+numt-1;k++)
            {
                if (s[k]!=t[j]) ct++;
                j++;
            }

            if (ct<min) min=ct;
       }

       cout<<min<<endl;
   }
    return 0;
}
```



# F.字典序

```c
#include <bits/stdc++.h>

using namespace std;

const int maxn=2e5+10;

int main()
{
	int n;
  while(cin>>n)
  {
     string s1,s,max="0";
    for (int i=1;i<=n;i++)
     {
       int m=i;
       s.clear();	s1.clear();
       while(m!=0)
        {
         s+=m%8+'0';
         m/=8;
        }
       for (int j=s.size()-1;j>=0;j--)
       {
       	    s1+=s[j];
	   }
        if (s1>max) max=s1;
     }
    cout<<max<<endl;
  }
	return 0;
}
```

# G.最小差值

```c
#include<bits/stdc++.h>

using namespace std;

const int maxn=2e5+10;
int a[maxn];

int main()
{
	int n,tot=0;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>a[i];
		tot+=a[i];
	}
	long long sum=0,ans=999999;
	for(int i=0;i<n;i++)
	{
		long long com;
		sum+=a[i];
		com=tot-sum;
		ans=min(ans, abs(com-sum));
	}
	cout<<ans;
	return 0;
}
```

# H.染色方案（待补）

# I.最大正方形

```c
#include<iostream>

#include<algorithm>

using namespace std;

int main()
{
	int a[1000+5];
	int n;
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>a[i];
	}
	sort(a+1, a+1+n);
	int ans=0;
	for(int i=n;i>=1;i--)
	{
		if(a[i]>=ans+1)
		{
			ans++;
		}
		else	break;
	}
	cout<<ans;
	return 0;
}
```

# J.最大值

___注意：此题用C++输入输出会超时___

```c
#include<iostream>
#include<cstdio>

using namespace std;

const int MAXN=2e5+10;
int a[MAXN];
int times=0;

int main()
{
	int n;
	while(cin>>n)
	{
		int maxn=0,next=0;
		for(int i=0;i<n;i++)
		{
			scanf("%d", &a[i]);
			maxn=max(maxn, a[i]);	
		}
		for(int i=0;i<n;i++)
		{
			if(a[i]==maxn)
			{
				times++;
				continue;
			}
			next=max(next, a[i]);
		}
		for(int i=0;i<n;i++)
		{
			if(a[i]>=maxn && times<=1)
			{
				printf("%d\n", next);
			}
			else
			{
				printf("%d\n", maxn);
			}
		}
	}
	return 0;
}
```



# K.循环排列(待补)

# L.库特与围棋(待补)

