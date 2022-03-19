## [AcWing 1813. 方块游戏](https://www.acwing.com/problem/content/1815/)

### 思路

枚举，题目说明不管哪一面向上都可以，所以就两面加起来取各个字母的最大值，最后N对字母的最大值相加就是答案

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
    int n;
    vector<int> ans(26);
    cin>>n;
    string s1,s2;
    for(int i=0;i<n;i++)
    {
        cin>>s1>>s2;
        vector<int> c1(26), c2(26);
        for(char c: s1) c1[c-'a']++;
        for(char c: s2) c2[c-'a']++;
        for(int j=0;j<26;j++)
            ans[j] += max(c1[j], c2[j]);
    }
    for(int i=0;i<26;i++)
        cout<<ans[i]<<endl;
    return 0;
}
```

## [AcWing 1801. 蹄子剪刀布](https://www.acwing.com/problem/content/1791/)

### 思路

枚举，根据题目叙述可知，相邻两个手势之间具有胜负关系，所以我们只需要将三个手势分别编号为0,1,2（原输入编号为1-3，需要进行处理），所以可以通过`(x+1)%3`获得x的下一个相邻手势，然后用两个ans分别记录两种情况下的胜负关系，然后输出较大值即可

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
    int n;
    cin>>n;
    int ans1=0, ans2=0;
    for(int i=0;i<n;i++)
    {
        int x,y;
        cin>>x>>y;
        x--;
        y--;
        if((x+1)%3==y)  ans1++;
        if(x==(y+1)%3)  ans2++;
    }
    cout<<max(ans1, ans2)<<endl;
    return 0;
}
```



## [AcWing 1789. 牛为什么过马路II](acwing.com/problem/content/1791/)

### 思路

枚举26个字母，计算字母之间只出现一次的字母，这里借鉴大佬的思路，可以使用位运算，出现一次就是1，其余即为0，最后使用`bitset.count()`函数返回1的个数

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
    string s;
    cin>>s;
    int ans=0;
    for(int i=0;i<26;i++)
    {
        bitset<32> bits;
        for(char c:s)
        {
            bits[c-'A']=bits[c-'A']^1;
            if(c-'A'==i && bits[i]==1)
            {
                bits=0;
                bits[i]=1;
            }
            if(c-'A'==i && bits[i]==0)
            {
                ans+=bits.count();
            }
        }
    }
    cout<<ans/2<<endl;
    return 0;
}
```

## [AcWing 1776. 牛的基因组学](https://www.acwing.com/problem/content/description/1778/)

### 思路

依旧是枚举的思路，按照每个位置依次进行枚举。每次枚举的时候，首先把有斑点的牛都遍历一遍，把所有出现过的基因都记录为一个库序列；其次再对没有斑点的奶牛进行遍历，如果出现了在有斑点的基因库序列中没有的基因，则退出循环（条件不成立），如果第二个循环正常退出，则说明符合条件，累加结果即可。

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

int n,m,ans;
const int MAXN = 110;


string s1[MAXN],s2[MAXN];

int main()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)   cin>>s1[i];
    for(int i=1;i<=n;i++)   cin>>s2[i];
    for(int i=0;i<m;i++)
    {
        int flag[30], x=0;
        memset(flag, 0, sizeof(flag));
        for(int j=1;j<=n;j++)   flag[s1[j][i] - 'A'] = 1;
        for(int j=1;j<=n;j++)
            if(flag[s2[j][i] - 'A'] == 1)
            {
                x=1;
                break;
            }
        if(x==0)    ans++;
    }
    cout<<ans<<endl;
    return 0;
}
```

## [AcWing 1762. 牛的洗牌](https://www.acwing.com/problem/content/1764/)

### 思路

题目给出排列后的顺序序列，要求还原出排列前的，因此我们可以按照给定的序列进行逆转即可，例如本来是奶牛从i位置去到j位置，现在逆向以后便成为了j位置的奶牛去i位置，假设用数组`a`保存序列，`b`序列记录最终位置，`c`序列记录三次逆序以后的位置。
由此可得，原来位于i位置的奶牛经过一次洗牌舞后会到`a[i]`位置，推理可得三次逆序后会到的位置序号为`a[a[a[i]]]`，因此编写程序对已知序列按照上述逻辑逆序即可。

```c++
#include<bits/stdc++.h>

using namespace std;

typedef long long ll;

int main()
{
    int n;
    cin>>n;
    vector<int> a(n+1), b(n+1), c(n+1);
    for(int i=1;i<=n;i++)
    {
        int j;
        cin>>j;
        a[j]=i;
    }
    for(int i=1;i<=n;i++)
        cin>>b[i];
    for(int i=1;i<=n;i++)
        c[a[a[a[i]]]]=b[i];
    for(int i=1;i<=n;i++)
        cout<<c[i]<<endl;
    return 0;
}
```

## [AcWing 1750. 救生员](https://www.acwing.com/problem/content/1752/)

### 思路

本题主要分析方法也还是使用枚举的方法，逐个奶牛进行解雇后对区间覆盖长度（有效工作时间区间段）进行判断。

使用`last`变量记录上一个奶牛工作结束时间，如果当前奶牛开始的时间大于`last`，则其有效工作时间即为`[start, end]`；若结束时间大于`last`，则`[last, end]`为其有效工作时间。最后根据枚举结果输出最长覆盖长度即可。

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

typedef long long ll;

int main()
{
    int n;
    cin>>n;
    vector<pair<int, int>> time(n);
    for(int i=0;i<n;i++)
        cin>>time[i].first>>time[i].second;
    sort(time.begin(), time.end());
    int res=0;
    for(int i=0;i<n;i++)
    {
        int last=-1, sum=0;
        for(int j=0;j<n;j++)
        {
            if(j==i)    continue;
            int start = time[j].first, end=time[j].second;
            if(start > last)
            {
                sum+=end-start;
                last=end;
            }
            else if(last<end)
            {
                sum+=end-last;
                last=end;
            }
        }
        res=max(res, sum);
    }
    cout<<res<<endl;
    return 0;
}
```

## [AcWing 1460. 我在哪？](https://www.acwing.com/problem/content/1462/)

### 思路

枚举，利用`unordered_set`从短到长依次遍历尝试不同长度的串，如果能找到相同存在的则说明不可以唯一判定，即跳出循环，否则就可以认为当前长度可以唯一判断，直接输出即可。

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
    int n;
    int i, j;
    string s;
    cin>>n>>s;
    for(i=1;i<=n;i++)
    {
        unordered_set<string> S;
        for(j=0;j<=n-i;j++)
        {
            string tmp = s.substr(j, i);
            if(S.count(tmp))    
            {
                break;
            }
            S.insert(tmp);
        }
        if(j==n-i+1)
        {
            cout<<i<<endl;
            break;
        }
    }
    return 0;
}
```

## [AcWing 1443. 拍照](https://www.acwing.com/problem/content/1445/)

### 思路

这里依然是使用枚举的方法，我们先假定已知了`a[1]`（由于要求字典序从小到大，这里通过从小到大枚举实现），然后便可以依照b与a的关系推出后续的各个数据。

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

const int MAXN = 1010;

int a[MAXN], b[MAXN];
int flag[MAXN];

int main()
{
    int n;
    cin>>n;
    for(int i=1;i<n;i++)
        cin>>b[i];
    for(int i=1;i<b[1];i++)
    {
        memset(flag, 0 ,sizeof(flag));
        bool res = true;
        a[1]=i;
        flag[i]=1;
        for(int j=2;j<=n;j++)
        {
            int t=b[j-1]-a[j-1];
            if(!flag[t] && t>=1 && t<=n)
            {
                a[j]=t;
                flag[t]=1;
            }
            else
            {
                res=false;
                break;
            }
        }
        if(res)
        {
            for(int i=1;i<=n;i++)
                cout<<a[i]<<" ";
            break;
        }
    }
    return 0;
}
```

## [AcWing 1672. 疯狂的科学](https://www.acwing.com/problem/content/1674/)

### 思路

枚举，分段找到连续的相反的一段，算作一次变换即可。

### 代码

```c++
#include<iostream>
#include<cstdio>
#include<cstring>

using namespace std;

string a,b;
int n, res;

int main()
{
    cin>>n>>a>>b;
    for(int i=0;i<n;i++)
    {
        if(a[i] == b[i])    continue;
        int j=i;
        while(a[j]!=b[j])   j++;
        res++;
        i=j-1;
    }
    cout<<res<<endl;
    return 0;
}
```

## [AcWing 1660. 社交距离II](https://www.acwing.com/problem/content/1662/)

### 思路

枚举，双指针找最小范围

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

vector<pair<int, int>> a;

int main()
{
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        int x,s;
        cin>>x>>s;
        a.push_back({x, s});
    }
    sort(a.begin(), a.end());
    int r=1e6;
    int s=a[0].second;//牛的生病状态
    for(int i=1;i<n;i++)
    {
        if(s!=a[i].second)
        {
            r=min(r, a[i].first-a[i-1].first);
        }
        s=a[i].second;
    }
    r--;
    
    int cnt=0;
    int x=-1;
    for(int i=0;i<n;i++)
    {
        if(x==-1 && a[i].second)    
        {
            x=a[i].first;
            cnt++;
        }
        else if(x!=-1)
        {
            if(a[i].first-x<=r)
            {
                x=a[i].first;
                continue;
            }
            if(a[i].second)
            {
                cnt++;
                x=a[i].first;
            }
        }
    }
    cout<<cnt<<endl;
    return 0;
}
```

## [AcWing 3347. 菊花链](https://www.acwing.com/problem/content/3350/)

### 代码

```c++
#include<bits/stdc++.h>

using namespace std;

const int N = 110;

int n;
int p[N];

int main()
{
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>p[i];
    int res=0;
    for(int i=0;i<n;i++)
    {
        unordered_set<int> H;
        for(int j=i, s=0;j<n;j++)
        {
            s+=p[j];
            H.insert(p[j]);
            int cnt = j-i+1;
            if(s%cnt == 0 && H.count(s/cnt))    res++;
        }
    }
    cout<<res<<endl;
    return 0;
}
```

# [3745. 牛的学术圈 I - AcWing题库](https://www.acwing.com/problem/content/3748/)

## 思路

记录一下引用次数的分布，然后遍历判断一下在何处引用之后能够得到最合适的`ans`即可

## 代码

```c++
#include<bits/stdc++.h>

using namespace std;

map<int, int> cnt;

int main()
{
    int n,l,x;
    cin>>n>>l;
    for(int i=0;i<n;i++)
    {
        cin>>x;
        cnt[x]++;
    }
    int ans;
    x=n;
    for(ans=0; ans<n; ans++)
    {
        x-=cnt[ans];
        if(x+min(cnt[ans], l) <= ans)   break;
    }
    cout<<ans<<endl;
    return 0;
}
```

# [1442. 单词处理器 - AcWing题库](https://www.acwing.com/problem/content/1444/)

## 思路

模拟题，按照题目给出的思路编程即可，需要注意控制括号的输出范围，即在开头，结尾均不输出

## 代码

```c++
#include<bits/stdc++.h>

using namespace std;

int main()
{
    int n,k;
    string s;
    cin>>n>>k;
    int cnt=0;
    for(int i=0;i<n;i++)
    {
        cin>>s;
        if(cnt + s.size() > k)
        {
            cout<<endl;
            cnt=0;
            cout<<s;
        }
        else if(cnt!=0)   cout<<" "<<s;
        else    cout<<s;
        cnt+=s.size();
    }
    return 0;
}
```

