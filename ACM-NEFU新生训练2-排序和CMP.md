---
title: ACM-NEFU新生训练2-排序和CMP
tags:
  - NEFUOJ
  - ACM
  - 排序
abbrlink: cd7c5106
date: 2020-12-22 20:15:10
---

# A.谁考了第k名-排序

### Description

```
在一次考试中，每个学生的成绩都不相同，现知道了每个学生的学号和成绩，求考第k名学生的学号和成绩。
```

### Input

```
第一行有两个整数，分别是学生的人数n（1≤n≤100），和求第k名学生的k（1≤k≤n）。
其后有n行数据，每行包括一个学号（整数）和一个成绩（浮点数），中间用一个空格分隔。
```

### Output

```
输出第k名学生的学号和成绩，中间用空格分隔。（注：请用%g输出成绩）
```

### Sample Input

```
5 3
90788001 67.8
90788002 90.3
90788003 61
90788004 68.4
90788005 73.9
```

### Sample Output

```
90788004 68.4
```

## 代码

```c++
#include <bits/stdc++.h>

using namespace std;

struct student
{
	int num;
	double grade;
}s[100];

int cmp(student s1,student s2)
{
	return s1.grade>s2.grade;
}

int main()
{
	int n,k;
	cin>>n>>k;
	for(int i=0;i<n;i++)
	{
		cin>>s[i].num>>s[i].grade;
	}
	sort(s,s+n,cmp);
	printf("%d %g",s[k-1].num,s[k-1].grade);
	return 0;
}
```

# B.奇数单增序列

### Description

```
给定一个长度为N（不大于500）的正整数序列，请将其中的所有奇数取出，并按升序输出
```

### Input

```
共2行：
第1行为 N；
第2行为 N 个正整数，其间用空格间隔。
```

### Output

```
增序输出的奇数序列，数据之间以逗号间隔。数据保证至少有一个奇数。
```

### Sample Input

```
10
1 3 2 6 5 4 9 8 7 10
```

### Sample Output

```
1,3,5,7,9
```

## 代码

```c++
#include <bits/stdc++.h>

using namespace std;

int main()
{
	int n,num[500],flag=1;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>num[i];
	}
	sort(num,num+n);
	for(int i=0;i<n;i++)
	{
		if(num[i]%2)
		{
			if(!flag)printf(",");
			printf("%d",num[i]);
			flag=0;
		}
	}
	return 0;
}
```

# C.成绩排序

### Description

```
给出班里某门课程的成绩单，请你按成绩从高到低对成绩单排序输出，如果有相同分数则名字字典序小的在前。
```

### Input

```
第一行为n (0 &lt; n &lt; 20)，表示班里的学生数目；
接下来的n行，每行为每个学生的名字和他的成绩, 中间用单个空格隔开。名字只包含字母且长度不超过20，成绩为一个不大于100的非负整数。
```

### Output

```
把成绩单按分数从高到低的顺序进行排序并输出，每行包含名字和分数两项，之间有一个空格。
```

### Sample Input

```
4
Kitty 80
Hanmeimei 90
Joey 92
Tim 28
```

### Sample Output

```
Joey 92
Hanmeimei 90 
Kitty 80
Tim 28
```

## 代码

```c++
#include<bits/stdc++.h>

using namespace std;

struct student
{
	char name[30];
	int grade;
}a[50];

int cmp(student s1,student s2)
{
	if(s1.grade!=s2.grade)	return s1.grade>s2.grade;
	else return strcmp(s1.name,s2.name)<0;
}

int main()
{
	int n;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>a[i].name>>a[i].grade;
	}
	sort(a,a+n,cmp);
	for(int i=0;i<n;i++)
	{
		cout<<a[i].name<<" "<<a[i].grade<<endl;
	}
	return 0;
}
```

# D.没必要的排序1

### Description

```
羽裳有n个数，她想知道前k大的数的和为多少
```

### Input

```
首先输入两个数n，k，代表有n个数，求前k大的和，接下来输入n个数，这n个数或是0或是1.
1&lt;=k&lt;=n&lt;=1000
```

### Output

```
输出一个数，为前k大的和
```

### Sample Input

```
5 3
0 0 1 0 1
```

### Sample Output

```
2
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	int n,k,cnt=0,tmp;
	while(cin>>n>>k)
	{
		cnt=0;
		for(int i=0;i<n;i++)
		{
			cin>>tmp;
			if(tmp)cnt++;
		}
		if(cnt<k)cout<<cnt<<endl;
		else cout<<k<<endl;
	}
	return 0;
}
```

# E.没必要的排序2

### Description

```
羽裳有n个数，她想知道前k大的数的和是多少
```

### Input

```
输入n，k代表有n个数，求前k大的和,之后输入n个数，第i个数为a[i]
1&lt;=n&lt;=10000000(1e7)
1&lt;=k&lt;1000
对任意的i
1&lt;=a[i]&lt;=100000(1e5)
```

### Output

```
输出一个数ans，ans是前k大数的和
```

### Sample Input

```
2 1
99999 1
```

### Sample Output

```
99999
```

### Hint

```
排序会超时
```

## 思路

hint都有了，必然桶排序

## Code

```c++
#include <bits/stdc++.h>

using namespace std;

int a[100010];
typedef long long ll;

int main()
{
    memset(a,0,sizeof(a));
    int n,k,tmp;
    ll ans=0;
    scanf("%d %d",&n,&k);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&tmp);
        a[tmp]++;
    }
    tmp=ans=0;
    for(int i=100000;i>=1;i--)
    {
        for(int j=a[i];j>0;j--)
        {
            ans+=i;
            k--;
            if(k==0)break;
        }
        if(k==0)break;
    }
    cout<<ans<<endl;
    return 0;
}
```

# F.老和尚的导员

### Description

```
等小和尚回来后，老和尚居然没有睡觉。老和尚表示他的导员的excel表格坏掉了（老和尚居然有导员？好吧……据说是方丈），而且老和尚的导员要老和尚将寺中所有和尚的期末考试成绩按降序排列来发奖学金（和尚还有奖学金？）
不用多说，为了继续睡午觉，这个任务理所当然的落在了小和尚身上。所有……你继续帮忙吧。
```

### Input

```
寺中主要考试科目有C语言，线性代数，高等数学和英语四个科目（怎么当和尚都这么累），输入的第一行是和尚的人数N（N&lt;=100)，第二行至第N+1行分别为C语言a[i]，线性代数b[i]，高等数学c[i]和英语的成绩d[i](0 &lt;= a[i],b[i],c[i],d[i] &lt;= 100)。
```

### Output

```
现需要你将和尚们的成绩以总成绩降序排列，输出数据的每行有两个数字，第一个数字为和尚的编号（输入时的第一个和尚成绩即为和尚1，第二个为和尚2），第二个数字为和尚的总成绩（如果总成绩相同，则按C语言的成绩排列，如在相同，则按线性代数输出编号，以此类推。）
```

### Sample Input

```
5
98 50 27 65
58 52 24 16
98 96 90 89
31 65 98 78
65 67 66 90
```

### Sample Output

```
3 373
5 288
4 272
1 240
2 150
```

## Code

```c++
#include <bits/stdc++.h>

using namespace std;

struct score
{
    int num;
    int c;
    int xxds;
    int gdsx;
    int yy;
    int sum;
}p[100];

int cmp(score h1, score h2)
{
    if(h1.sum!=h2.sum)return h1.sum>h2.sum;
    if(h1.c != h2.c)return h1.c > h2.c;
    if(h1.xxds!=h2.xxds)return h1.xxds>h2.xxds;
    if(h1.gdsx!=h2.gdsx)return h1.gdsx>h2.gdsx;
    if(h1.yy!=h2.yy)return h1.yy>h2.yy;
}

int main()
{
    int n;
    while(cin>>n)
    {
        for(int i=0;i<n;i++)
        {
            p[i].num=i+1;
            cin >> p[i].c >> p[i].xxds >> p[i].gdsx >> p[i].yy;
            p[i].sum= p[i].c + p[i].xxds + p[i].gdsx + p[i].yy;
        }
        sort(p,p+n,cmp);
        for(int i=0;i<n;i++)
        {
            cout<<p[i].num<<" "<<p[i].sum<<endl;
        }
    }
    return 0;
}
```

# G.健忘的老和尚

### Description

```
当小和尚排完名单后，老和尚突然一拍脑袋：“导员把每个人的人名都给我了，可我忘记告诉你了。”好吧……我们可怜的小和尚看来要费二遍事了（好像之前的任务都是你帮他做的，好吧，你真可怜）
```

### Input

```
输入数据为多组，输入的第一行为和尚的人数N，可以得到奖学金的人数M，和需要补考的人数O（在这里可以满足M+O&lt;=N，即得到奖学金的和尚一定不用参加补考）。之后的N行每行都有一个字符串（即为和尚的名字，长度小于100）和尚考试的总分a[i](0 &lt;= a[i] &lt;= 1000)。
```

### Output

```
前M行，每行是获得奖学金的小和尚的名字；
后O行，每行是补考的小和尚的名字；
由于老和尚觉得很对不起小和尚，所以他决定这次简单些，所以无论是奖励还是惩罚都按照总成绩从低到高输出和尚的名字即可。
```

### Sample Input

```
5 1 2
a 192 
aa 212
ab 351
bab 128
bbaa 654
```

### Sample Output

```
bbaa
bab
a
```

## Code

```c++
#include <bits/stdc++.h>

using namespace std;

struct score
{
    int grade;
    char name[110];
}p[10000];

int cmp(score h1, score h2)
{
    return h1.grade>h2.grade;
}

int main()
{
    int n,m,o;
    while(cin>>n>>m>>o)
    {
        for(int i=0;i<n;i++)
        {
        	getchar();
            cin>>p[i].name>>p[i].grade;
        }
        sort(p,p+n,cmp);
        for(int i=m-1;i>=0;i--)
        {
            cout<<p[i].name<<endl;
        }
        for(int i=n-1;i>=n-o;i--)
        {
            cout<<p[i].name<<endl;
        }
    }
    return 0;
}
```

# H.戏说三国

### Description

```
   东汉末年，宦官当权，民不聊生。灵帝中平元年，张角兄弟发动黄巾起义，官军闻风丧胆。为抵抗黄巾，幽州太守刘焉出榜招兵。榜文前，刘备、关羽、张飞三兄弟萍水相逢。三人都有为国效力之心，于是桃园结为异姓兄弟，开始了一段三国浪漫传奇……
  如果我问你三国中谁最聪明你一定会说是诸葛亮，我要问你谁武功最高，保不准你就要说关羽。是啊，我们的刘备对于优秀的手下向来十分满意。可是有一天，刘备在无意间在朋友圈里看到了梁山的宋江正在进行英雄排座次，最可气的是这条动态已经被转发评论了无数次了……这一下可气坏了刘备，他决定也来一次，蜀中文武百官大排名。
为了公平起见，诸葛亮帮他制定一条评分标准：每个官员有一个智育、德育、武育三个分数，分别以b%，a%，c%的比率计入加权总分，按总分降序排列，总分相同按智育折合后的分数降序，智育相同按德育，依次类推最终决定排名。（保证没有排名一样的两个人）
```

### Input

```
输入第一行t(1&lt;=t&lt;=1000)表示输入的组数。接下来每组第一行1个整数n（1&lt;=n&lt;=100000）三个实数a,b,c(a+b+c=100)接下来n行每行包括英雄的名字，字符串s（1&lt;=len&lt;=20,全为小写字母） 智育、德育、武育的分数（整数）。（1~100）
```

### Output

```
每组第一行输出是第几组输出Case #t：接下来输出n行每行包括英雄的名字，总分，智育、德育、武育折合后的分数（保留四位小数）。
```

### Sample Input

```
1
3 20.00  20.00  60.00
zhugeliang 90 80 0
zhangfei 0 0 100
guanyu 10 100 100
```

### Sample Output

```
Case #1:
guanyu 82.0000 2.0000 20.0000 60.0000
zhangfei 60.0000 0.0000 0.0000 60.0000
zhugeliang 34.0000 18.0000 16.0000 0.0000
```

## Code

```c++
#include <bits/stdc++.h>

using namespace std;

struct haohan
{
    double sum;
    double zy;
    double dy;
    double wy;
    char name[21];
}p[1010];

int cmp(haohan p1, haohan p2)
{
    return p1.sum>p2.sum;
}

int main()
{
    int t,n;
    double a,b,c;
    cin>>t;
    for(int i=0;i<t;i++)
    {
        cin>>n>>a>>b>>c;
        for(int j=0;j<n;j++)
        {
            cin>>p[j].name>>p[j].zy>>p[j].dy>>p[j].wy;
            p[j].zy*=b/100;
            p[j].dy*=a/100;
            p[j].wy*=c/100;
            p[j].sum=p[j].zy+p[j].dy+p[j].wy;
        }
        sort(p,p+n,cmp);
        printf("Case #%d:\n",i+1);
        for(int j=0;j<n;j++)
        {
            printf("%s %.4lf %.4lf %.4lf %.4lf\n",p[j].name,p[j].sum,p[j].zy,p[j].dy,p[j].wy);
        }
    }
    return 0;
}
```

# I.相约摩洛哥

### Description

```
2015年5月ACM全球总决赛在摩洛哥卡萨布兰卡举行，到时会有来自世界各地的acm大牛齐聚摩洛哥。
  
作为本次大赛的举办方负责人阿焜和阿祥被分配给了一个重要的任务，就是在比赛中统计大家的排名。
acm大赛排名规则（摘自百度百科）：
    最后的获胜者为正确解答题目最多且总用时最少的队伍。每道试题用时将从竞赛开始到试题解答被判定为正确为止，其间每一次提交运行结果被判错误的话将被加罚20分钟时间，未正确解答的试题不记时。例如：A、B两队都正确完成两道题目，其中A队提交这两题的时间分别是比赛开始后1:00和2:45，B队为1:20和2:00，但B队有一题提交了2次。这样A队的总用时为1:00+2:45=3:45而B队为1:20+2:00+0:20=3:40，所以B队以总用时少而获胜。
请你帮他俩完成这项任务吧！（保证没有成绩完全相同两支队伍）
```

### Input

```
多组输入，每组一个正整数n（1&lt;=n&lt;=100000），代表有n个队伍参赛。接下来的n行，每行分别有一个字符串（只包含小写英文字母）作为队伍名，长度1&lt;=len&lt;=10;ABC三道题的ac时间（以分钟为单位），时间为-1表示该题没有ac。接下来又有n行，表示每道题的提交次数,用3个正整数表示。
```

### Output

```
输出排行榜。n行，每行一个队伍的名字、解题数和总用时。
```

### Sample Input

```
3	
team 30 10 20
behappy 20 20 90
newbee 15 16 17
1 1 1
1 1 10
1 1 1
```

### Sample Output

```
newbee 3 48
team 3 60
behappy 3 310
```

## Code

```c++
#include <bits/stdc++.h>
using namespace std;

struct team
{
    char name[11];
    int ta,tb,tc;
    int actime;
    int num;
}t[100010];

int cmp(team t1,team t2)
{
    if(t1.num!=t2.num)return t1.num>t2.num;
    return t1.actime < t2.actime;
}

int main()
{
    int n,a,b,c;
    while(cin>>n)
    {
        for(int i=0;i<n;i++)
        {
            cin>>t[i].name>>t[i].ta>>t[i].tb>>t[i].tc;
        }
        for(int i=0;i<n;i++)
        {
            cin>>a>>b>>c;
            t[i].actime=0;
            t[i].num=0;
            if(t[i].ta!=-1)
            {
                t[i].ta+=20*(a-1);
                t[i].num++;
                t[i].actime+=t[i].ta;
            }
            if(t[i].tb!=-1)
            {
                t[i].tb+=20*(b-1);
                t[i].num++;
                t[i].actime+=t[i].tb;
            }
            if(t[i].tc!=-1)
            {
                t[i].tc+=20*(c-1);
                t[i].num++;
                t[i].actime+=t[i].tc;
            }
        }
        sort(t,t+n,cmp);
        for(int i=0;i<n;i++)
        {
            cout << t[i].name << " " << t[i].num << " " << t[i].actime << endl;
        }
    }
    return 0;
}
```

# J.结构体排序题一

### Description

```
现在给定一组二维平面上面的点的坐标,保证它的坐标是int类型的整数且大于等于0小于等于99.请你按照数据所给要求进行排序.
首先先以横坐标进行排序,若横坐标相同则按纵坐标排序,降序或升序将以0和1的形式表示,0表示降序,1表示升序.
比如,若数据开头给出0 1的要求,则表示先以横坐标降序排列,若横坐标相同则按纵坐标升序排列.
再比如,若数据开头给出1 1的要求,则表示先以横坐标升序排列,若横坐标相同则按纵坐标升序排列.
保证点的数量大于等于3小于等于99
```

### Input

```
第1行包含三个数字,第一个数字和第二个数字分别表示横坐标和纵坐标的排序要求,0表示降序,1表示升序,第三个数字n表示有几个点需要排序.
余下第2~n+1行每行各有两个类型为n个点的坐标,例如第2行第一个数字表示第一个点横坐标,第二个数字表示第一个点的纵坐标.
```

### Output

```
输出n行坐标,一行表示一个点,格式如下:
(x0,y0)
```

### Sample Input

```
0 0 4
1 1
1 2
2 1
2 2
1 1 4
2 2
2 1
1 2
1 1
```

### Sample Output

```
(2,2)
(2,1)
(1,2)
(1,1)
(1,1)
(1,2)
(2,1)
(2,2)
```

## Code

```c++
#include<bits/stdc++.h>

using namespace std;

struct node
{
    int x;
    int y;
}a[110];

int rey,rex;

int cmp(node t1,node t2)
{
    if(rex)
    {
        if(t1.x!=t2.x)  return t1.x<t2.x;
        if(rey) return t1.y<t2.y;
        else    return t1.y>t2.y;
    }
    else
    {
        if(t1.x!=t2.x)  return t1.x>t2.x;
        if(rey) return t1.y<t2.y;
        else    return t1.y>t2.y;
    }
}

int main()
{
    int n;
    while(cin>>rex>>rey>>n)
    {
        for(int i=0;i<n;i++)
        {
            cin>>a[i].x>>a[i].y;
        }
        sort(a,a+n,cmp);
        for(int i=0;i<n;i++)
        {
            printf("(%d,%d)\n",a[i].x,a[i].y);
        }
    }
    return 0;
}
```

# K.最强编码同学票数统计

### Description

```
为了找出林大2020新生中最擅长编写代码的同学，学校发起了一场投票。通过同学报名、前期遴选等环节，共提名了100名同学作为选举人进行评选，假设他们的编号从1到100。现在学院已经采集到了n名同学的投票结果，请你找出得票最多的程序员获得的票数(注：就是让你找相同数字的个数的最大值)。
```

### Input

```
输入：
第一行一个数n(1≤n≤100) , 代表有多少位同学投票。
第二行n个数:a1,a2⋯an,  ai(1≤ai≤100)代表第i位同学投票所选的林大最擅长编码的那个程序员的编号。输入数据有多组！
```

### Output

```
输出：
每组数据的输出占一行 , 输出得票最多的那个同学获得的票数（相同编号的个数的最大值）。
```

### Sample Input

```
输入样例1：
6
1 2 4 7 7 7
4
5 5 5 5
```

### Sample Output

```
输出样例1：
3
4
```

## Code

```c++
#include<bits/stdc++.h>

using namespace std;

int a[110];

int main()
{
	int n;
	while(cin>>n)
	{
		memset(a,0,sizeof(a));
		for(int i=1;i<=n;i++)
		{
			int tmp;
			cin>>tmp;
			a[tmp]++;
		}
		int ans=0;
		for(int i=1;i<=100;i++)
		{
			ans=max(ans, a[i]);
		}
		cout<<ans<<endl;
	}
	return 0;
}
```

