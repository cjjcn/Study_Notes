---
title: ACM-NEFU-2020大一寒假培训三（暴力）
tags:
  - ACM
  - NEFUOJ
  - 暴力
abbrlink: eac8bff4
date: 2020-12-24 16:39:28
---

# A.二倍的问题

### Description

```
给定2到15个不同的正整数，你的任务是计算这些数里面有多少个数对满足：数对中一个数是另一个数的两倍。比如给定1 4 3 2 9 7 18 22，得到的答案是3，因为2是1的两倍，4是2个两倍，18是9的两倍。 
```

### Input

```
输入包括n组测试数据。每组数据包括一行，给出2到15个两两不同且小于100的正整数。每一行最后一个数是0，表示这一行的结束后，这个数不属于那2到15个给定的正整数。
```

### Output

```
对每组输入数据，输出一行，给出有多少个数对满足其中一个数是另一个数的两倍。
```

### Sample Input

```
3
1 4 3 2 9 7 18 22 0
2 4 8 10 0
7 5 11 13 1 3 0
```

### Sample Output

```
3
2
0
```

## Code

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
	int vis[25],a[25];
	int t,x,p,ans;
	cin>>t;
	while(t--)
	{
		memset(vis,0,sizeof(vis));
		p=ans=0;
		while(cin>>x&&x)
		{
			p++;
			a[p]=x;
		}
		sort(a+1,a+p+1);
		for(int i=1;i<=p;i++)
		{
			if(vis[a[i]]==1)	ans++;
			vis[2*a[i]]=1;
		}
		cout<<ans<<endl;
	}
	return 0;
}
```

# B.大乐透

### Description

```
在小明曾经玩过的一种对号码的纸牌游戏(乐透)里,玩家必须从{1,2,……，49}中选择6个数。玩Lotto的一个流行策略是（虽然它并不增加你赢的机会）：就是从这49个数中，选出k（k&gt;6）个数组成一个子集S，然后只从S里拿出牌来玩几局游戏。例如，k=8，s={1,2,3,5,8,13,21,34}，那么有28场可能的游戏：[1,2,3,5,8,13],[1,2,3,5,8,21],[1,2,3,5,8,34],[1,2,3,5,13,21],……,[3,5,8,13,21,24]。
读取数字k和一组数S，输出由S中的数组成的所有可能的游戏。
```

### Input

```
输入数据有多组，每组一行，每行有多个整数，其中第一个整数为数字k，接下来有k个整数，即子集S。当k为0，输入结束。
```

### Output

```
输出由S中的数组成的所有可能的游戏。每种游戏一行。
```

### Sample Input

```
7 1 2 3 4 5 6 7
0
```

### Sample Output

```
1 2 3 4 5 6
1 2 3 4 5 7
1 2 3 4 6 7
1 2 3 5 6 7
1 2 4 5 6 7
1 3 4 5 6 7
2 3 4 5 6 7
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	int k,num[1000];
	while(1)
	{
		cin>>k;
		if(k==0)break;
		for(int i=0;i<k;i++)
		{
			cin>>num[i];
		}
		for(int i=0;i<k;i++)
		{
			for(int j=i+1;j<k;j++)
			{
				for(int l=j+1;l<k;l++)
				{
					for(int m=l+1;m<k;m++)
					{
						for(int n=m+1;n<k;n++)
						{
							for(int o=n+1;o<k;o++)
							{
								printf("%d %d %d %d %d %d\n",num[i],num[j],num[l],num[m],num[n],num[o]);
							}
						}
					}
				}
			}
		}
	}
	return 0;
}
```

# C.密码箱

### Description

```
小明的密码箱打不开了，小明的密码箱是传统的3位滚轮密码。小明完全不记得他的密码了，所以他从 000开始以升序开始尝试，他已经试到第abc位密码了，可是箱子还是没有打开，他希望你将之后所有可能尝试的密码输出，这样他就可以完全不去思考，让他波动密码盘更有效率
```

### Input

```
每行输入一个整数n(0 &lt; n &lt; 1000);n没有前缀0。
```

### Output

```
n之后所有可能尝试的密码;输出有前缀0的。
```

### Sample Input

```
989
```

### Sample Output

```
990
991
992
993
994
995
996
997
998
999
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	int n;
	while(cin>>n)
	{
		for(int i=n+1;i<1000;i++)
		{
			if(i<10)	cout<<"0";
			if(i<100)	cout<<"0";
			cout<<i<<endl;
		}
	}
	return 0;
}
```

# D.字符串统计

### Description

```
对于给定的一个字符串，统计其中数字字符出现的次数。
```

### Input

```
输入数据有多组，第一行是一个整数n，表示测试实例的个数，后面跟着n行，每行包括一个由字母和数字组成的字符串。
```

### Output

```
对于每个测试实例，输出该串中数值的个数，每个输出占一行。
```

### Sample Input

```
2
asdfasdf123123asdfasdf
asdf111111111asdfasdfasdf
```

### Sample Output

```
6
9
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	int n,ans;
	string s;
	cin>>n;
	while(n--)
	{
		ans=0;
		cin>>s;
		for(int i=0;i<s.size();i++)
		{
			if(s[i]>='0'&&s[i]<='9')	ans++;
		}
		cout<<ans<<endl;
	}
	return 0;
}
```

# E.丑数

### Description

```
只有质数2,3,5,7这几个作为因子的数叫做,丑数,比如前20个丑数是(从小到大来说) 1,2,3,4,5,6,7,8,9,10,12,14,15,16,18,20,21,24和25.
```

### Input

```
我们给你个n（1&lt;=m&lt;=5842）当输入n为0结束。
```

### Output

```
输出第n个丑数。每个数一行。
```

### Sample Input

```
1
2
3
4
11
```

### Sample Output

```
1
2
3
4
12
```

## Code

这题打表过的，就不出了...

# F.矩形

### Description

```
在测试超大规模集成电路时，对给定的一个设计，专家要检测元件是否相互遮盖。一个元件可视为一个矩形，假设每个矩形都是水平排列的（边与x轴或y轴平行），所以长方形由最小的和最大的x，y坐标表示。
编程计算完全被覆盖的矩形个数。
```

### Input

```
输入有多组长方形实例。对每组长方形，第一个数字是长方形的数量，然后是长方形的最小和最大x，y坐标（最小x，最大x，最小y，最大y）。
```

### Output

```
对每组输入数据，输出一行，是被完全覆盖的长方形数量。
```

### Sample Input

```
3
100 101 100 101
0 3 0 101
20 40 10 400
4
10 20 10 20
10 20 10 20
10 20 10 20
10 20 10 20
```

### Sample Output

```
0
4
```

## Code

```c++
#include <bits/stdc++.h>

using namespace std;

struct node
{
    int minx,maxx;
    int miny,maxy;
}s[10010];

int main()
{
    int n,ans,a[10010]={0};
    while(cin>>n)
    {
        ans=0;
        memset(a, 0, sizeof(a));
        for(int i=0;i<n;i++)
        {
            cin>>s[i].minx>>s[i].maxx>>s[i].miny>>s[i].maxy;
        }
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==j)continue;
                if(!a[j] && s[i].minx <= s[j].minx && s[i].maxx >= s[j].maxx && s[i].miny <= s[j].miny && s[i].maxy >= s[j].maxy)
                {
                    ans++;
                    a[j]=1;
                }
                if(!a[i] && s[j].minx <= s[i].minx && s[j].maxx >= s[i].maxx && s[j].miny <= s[i].miny && s[j].maxy >= s[i].maxy)
                {
                    ans++;
                    a[i]=1;
                }
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```

# G.抽奖

### Description

```
公司举办年会，为了活跃气氛，设置了摇奖环节。参加聚会的每位员工都有一张带有号码的抽奖券。现在，主持人依次公布 n 个不同的获奖号码，小谢看着自己抽奖券上的号码 num，无比紧张。请编写一个程序，如果小谢获奖了，请输出他中的是第几个号码；如果没有中奖，请输出 0。
```

### Input

```
第一行一个正整数 n，表示有 n 个获奖号码，2&lt;n≤100。
第二行包含 n 个正整数，之间用一个空格隔开，表示依次公布的 n 个获奖号码。
第三行一个正整数 num，表示小谢抽奖券上的号码。
1≤获奖号码，num&lt;10000。
```

### Output

```
一行一个整数，如果小谢中奖了，表示中奖的是第几个号码；如果没有中奖，则为 0。
```

### Sample Input

```
7
17 2 3 4 9555 6 1
3
```

### Sample Output

```
3
```

### Hint

```
暴力
单组输入
```

## Code

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
	int n,num[110],lotto,flag=0;
	int i;
	cin>>n;
	for(i=0;i<n;i++)
	{
		cin>>num[i];
	}
	cin >> lotto;
	for(i=0;i<n;i++)
	{
		if(num[i] == lotto)
		{
			cout<<i+1;
			break;
		}
	}
	if(i==n)    cout<<0;
	return 0;
}
```

# H.比身高

### Description

```
有 N 个人排成一排，假设他们的身高均为正整数，请找出其中符合以下条件的人：排在他前面且比他高的人数与排在他后面且比他高的人数相等。
```

### Input

```
第一行为一个正整数 N，1&lt;N&lt;1000，表示有多少个人。
下面 N 行，每行一个正整数，表示从前往后每个人的身高，假设每个人的身高≤10000。
```

### Output

```
一行一个整数，表示满足这个条件的人数。
```

### Sample Input

```
4
1
2
1
3
```

### Sample Output

```
2
```

### Hint

```
第 3、第 4 个人满足条件。
单组输入
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	int n,num[1010],l,r,ans=0;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>num[i];
	}
	for(int i=0;i<n;i++)
	{
		l=r=0;
		for(int j=i-1;j>=0;j--)
		{
			if(num[j]>num[i])	l++;
		}
		for(int j=i+1;j<n;j++)
		{
			if(num[j]>num[i])	r++;
		}
		if(l==r)	ans++;
	}
	cout<<ans;
	return 0;
}
```

# I.楼层编号

### Description

```
小林在 NOIP 比赛期间住在“新世界”酒店。和其他酒店不一样的是，这个酒店每天都有一个高能的数字 t，这个数字在楼层中是不会出现的，以 t=3 为例，则 3、13、31、33 等楼层是不存在的，楼层编号为 1，2，4，5，…所以实际上的 4 楼才是 3 楼。
已知小林预订了编号为 m 层的房间，并且当天高能数字是 t，现在他想知道房间所在的真实楼层是多少。
```

### Input

```
一行两个整数 m 和 t，1≤m≤100000，0≤t≤9，保证 m 对 t 合法。
```

### Output

```
一行一个整数，表示真实楼层。
```

### Sample Input

```
14 3
```

### Sample Output

```
12
```

### Hint

```
单组输入
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int t,b;

bool judge(int n)
{
    while(n>0)
    {
        b=n%10;
        n/=10;
        if(b==t)return 0;
    }
    return 1;
}

int main()
{
    int m;
    int now=0;
    cin >> m >> t;
    for(int i=1; i <= m; i++)
    {
        if(judge(i))	now++;
    }
    cout<<now;
    return 0;
}
```

# J.比例简化

### Description

```
在社交媒体上，经常会看到针对某一个观点同意与否的民意调查以及结果。例如，对某观点表示支持的有 1498 人，反对的有 902 人，那么其比例可以简单地记为1498∶902。
因该比例的数值太大，难以一眼看出它们的关系。若把比例记为 5∶3，虽然与真实结果有一定的误差，但依然能够较为准确地反映调查结果，同时也显得比较直观。
现给出支持人数 A 和反对人数 B，以及一个上限 L，请将 A 比 B 化简为 A′ 比 B′，要求在 A′和 B′ 均不大于 L，且 A′ 和 B′ 互质（两个整数的最大公约数为 1）的前提下，A′/B′≥ A/B 且 A′/B′-A/B 的值尽可能小。
```

### Input

```
一行三个整数 A，B，L，每两个整数之间用一个空格隔开，分别表示支持人数、反对人数以及上限。
```

### Output

```
一行两个整数 A′ 和 B′，中间用一个空格隔开，表示化简后的比例。
```

### Sample Input

```
1498 902 10
```

### Sample Output

```
5 3
```

### Hint

```
单组输入,1&lt;=A,B&lt;=1000000,1&lt;=L&lt;=100,A/B&lt;=L
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int a,b,l,tmp,a1,a2,t;
    double fs,minnum=10000000,r;
    cin>>a>>b>>l;
    tmp=__gcd(a,b);
    a/=tmp;
    b/=tmp;
    r= (double)a / b;
    if(a<l&&b<l)cout<<a<<" "<<b;
    else
    {
        a1=a;
        a2=b;
        for(int i=1;i<=l;i++)
        {
            for(int j=1;j<=l;j++)
            {
                if(__gcd(i,j)!=1)continue;
                fs=(double)i/j;
                if(fs < r)continue;
                if(fabs(fs - r) < minnum)
                {
                    minnum=fabs(fs - r);
                    a1=i;
                    a2=j;
                }
            }
        }
        cout << a1 << " " << a2;
    }
    return 0;
}
```

# K.奶牛碑文

### Description

```
小伟暑假期间到大草原旅游，在一块石头上发现了一些有趣的碑文。碑文似乎是一个神秘古老的语言，只包括三个大写字母 C、O 和 W。尽管小伟看不懂，但是令他高兴的是，C、O、W的顺序形式构成了一句他最喜欢的奶牛单词“COW”。现在，他想知道有多少次 COW 出现在文本中。
如果 COW 内穿插了其他字符，只要 COW 字符出现在正确的顺序，小伟也不介意。甚至，他也不介意出现不同的 COW 共享一些字母。例如，CWOW 出现了 1 次 COW，CCOW 算出现了2 次 COW，CCOOWW 算出现了 8 次 COW。
```

### Input

```
第 1 行为 1 个整数 N。
第 2 行为 N 个字符的字符串，每个字符是一个 C、O 或 W。
```

### Output

```
输出 COW 作为输入字符串的字串出现的次数（不一定是连续的）。
提示：答案会很大，建议用 64 位整数（long long）。
```

### Sample Input

```
6
COOWWW
```

### Sample Output

```
6
```

### Hint

```
对于 50% 的数据满足：N≤60。
对于 100% 的数据满足：N≤100000。
单组输入
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int n;
    long long c=0,co=0,cow=0;
    string s;
    cin >> n >> s;
    for(int i=0;i<n;i++)
    {
        if(s[i] == 'C')c++;
        if(s[i] == 'O')co+=c;
        if(s[i] == 'W')cow+=co;
    }
    cout<<cow<<endl;
    return 0;
}
```

