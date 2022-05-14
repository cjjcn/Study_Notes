---
title: CodeForces-Round#684-Div.2
tags:
  - ACM
  - CF
abbrlink: 676fcf4a
date: 2020-11-18 11:52:18
---

# A. Buy the String

<!--more-->

```c++
#include<iostream>
#include<cstdio>

using namespace std;

char c[1005];

int main(){
    int T;
    int sum_1=0,sum_0=0,ans;
    cin>>T;
    while(T--)
    {
        int n,c0,c1,h;
        cin>>n>>c0>>c1>>h;
        for(int i=0;i<n;i++)
        {
            cin>>c[i];
            if(c[i]=='1')   sum_1++;
            else    sum_0++;
        }
        //cout<<sum_0<<endl<<sum_1<<endl;
        ans=c0*sum_0+c1*sum_1;
        /*if(c0==c1)
            ans=c0*sum_0+c1*sum_1;
        if(c0>c1)
            ans=min((c1*(sum_1+sum_0))+sum_0*h, ans);
        if(c0<c1)
            ans=min(c0*(sum_1+sum_0)+sum_1*h, ans);*/
        for(int i=1;i<=sum_0;i++)
        {
            int now=(i*h)+((sum_1+i)*c1)+(sum_0-i)*c0;
            ans=min(ans, now);
        }
        for(int i=1;i<=sum_1;i++)
        {
            int now=(i*h)+((sum_0+i)*c0)+(sum_1-i)*c1;
            ans=min(ans, now);
        }
        cout<<ans<<endl;
        ans=0;  sum_1=0;    sum_0=0;
    }
}
```



# B. Sum of Medians

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<vector>

using namespace std;

char c[1005];

int main(){
    int T;
    int sum_1=0,sum_0=0,ans;
    cin>>T;
    while(T--)
    {
        int n,k;
        cin>>n>>k;
        int m=n*k;
        vector<int>a(m);
        for(int i=0;i<m;i++)
        { 
        	cin>>a[i];
         }   
        long long tot=0;
        if(n==2)
        {
            for(int i=0;i<m;i+=2)
                tot+=a[i];
            cout<<tot<<endl;
            continue;
        }
        int inter=(n+1)/2;
        int left=n-inter;
        left+=1;
        int x=1,in=0;
        for(int i=m-1;i>=0;i--)
        {
            if(x%left==0)
            {
                tot+=a[i];
                in++;
            }
            x+=1;
            if(in>=k)   break;
        }
        cout<<tot<<endl;
    }
    return 0;
}
```



# C1. Binary Table (Easy Version)

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<vector>
 
        using namespace std;
 
        void case4(vector<vector<int>>&b , vector<string>& a , int i , int j){
            a[i][j]=a[i+1][j]=a[i][j+1]=a[i+1][j+1]='0';
            vector<int>c;
            c.push_back(i);
            c.push_back(j);
            c.push_back(i+1);
            c.push_back(j);
            c.push_back(i+1);
            c.push_back(j+1);
            b.push_back(c);
            c.clear();
            c.push_back(i);
            c.push_back(j+1);
            c.push_back(i+1);
            c.push_back(j);
            c.push_back(i+1);
            c.push_back(j+1);
            b.push_back(c);
            c.clear();
            c.push_back(i);
            c.push_back(j);
            c.push_back(i);
            c.push_back(j+1);
            c.push_back(i+1);
            c.push_back(j);
            b.push_back(c);
            c.clear();
            c.push_back(i);
            c.push_back(j);
            c.push_back(i);
            c.push_back(j+1);
            c.push_back(i+1);
            c.push_back(j+1);
            b.push_back(c);
        }
 
        void case1(vector<vector<int>>&b , vector<string>& a , int i , int j){
            vector<int>c;
            if(a[i][j]=='1'){
                c.push_back(i);
                c.push_back(j+1);
                c.push_back(i+1);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j+1);
                b.push_back(c);
            }
            if(a[i][j+1]=='1'){
                c.push_back(i);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j+1);
                b.push_back(c);
            }
            if(a[i+1][j]=='1'){
                c.push_back(i);
                c.push_back(j);
                c.push_back(i);
                c.push_back(j+1);
                c.push_back(i+1);
                c.push_back(j+1);
                b.push_back(c);
            }
            if(a[i+1][j+1]=='1'){
                c.push_back(i);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j);
                c.push_back(i);
                c.push_back(j+1);
                b.push_back(c);
            }
        }
 
        void case2(vector<vector<int>>&b , vector<string>& a , int i , int j){
            vector<int>c;
            if(a[i][j]=='1' && a[i][j+1]=='1'){
                c.push_back(i);
                c.push_back(j);
                c.push_back(i);
                c.push_back(j+1);
                c.push_back(i+1);
                c.push_back(j+1);
                b.push_back(c);
                a[i][j]=a[i][j+1]='0';
                a[i+1][j+1]='1';
            }
            if(a[i][j]=='1' && a[i+1][j]=='1'){
                c.push_back(i);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j+1);
                b.push_back(c);
                a[i][j]=a[i+1][j]='0';
                a[i+1][j+1]='1';
            }
            if(a[i][j]=='1' && a[i+1][j+1]=='1'){
                c.push_back(i);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j+1);
                c.push_back(i+1);
                c.push_back(j);
                b.push_back(c);
                a[i][j]=a[i+1][j+1]='0';
                a[i+1][j]='1';
            }
            if(a[i][j+1]=='1' && a[i+1][j]=='1'){
                c.push_back(i);
                c.push_back(j+1);
                c.push_back(i+1);
                c.push_back(j);
                c.push_back(i);
                c.push_back(j);
                b.push_back(c);
                a[i][j+1]=a[i+1][j]='0';
                a[i][j]='1';
            }
            if(a[i][j+1]=='1' && a[i+1][j+1]=='1'){
                c.push_back(i);
                c.push_back(j+1);
                c.push_back(i+1);
                c.push_back(j+1);
                c.push_back(i);
                c.push_back(j);
                b.push_back(c);
                a[i][j+1]=a[i+1][j+1]='0';
                a[i][j]='1';
            }
            if(a[i+1][j]=='1' && a[i+1][j+1]=='1'){
                c.push_back(i+1);
                c.push_back(j);
                c.push_back(i+1);
                c.push_back(j+1);
                c.push_back(i);
                c.push_back(j);
                b.push_back(c);
                a[i+1][j]=a[i+1][j+1]='0';
                a[i][j]='1';
            }
        }
 
        void solve(){
            int n,m;
            cin>>n>>m;
            vector<string>a(n);
            for(int i=0;i<n;i++){
                cin>>a[i];
            }
            vector<vector<int>>b;
            for(int i=0;i<n-1;i++){
                for(int j=0;j<m-1;j++){
                    int total=0;
                    total=(a[i][j]-'0')+(a[i][j+1]-'0')+(a[i+1][j]-'0')+(a[i+1][j+1]-'0');
                    if(total==0)
                        continue;
                    else if(total==3){
                        vector<int>c;
                        if(a[i][j]=='1'){
                            a[i][j]='0';
                            c.push_back(i);
                            c.push_back(j);
                        }
                        if(a[i][j+1]=='1'){
                            a[i][j+1]='0';
                            c.push_back(i);
                            c.push_back(j+1);
                        }
                        if(a[i+1][j]=='1'){
                            a[i+1][j]='0';
                            c.push_back(i+1);
                            c.push_back(j);
                        }
                        if(a[i+1][j+1]=='1'){
                            a[i+1][j+1]='0';
                            c.push_back(i+1);
                            c.push_back(j+1);
                        }
                        b.push_back(c);
                    }
                    else if(total==4){
                        case4(b,a,i,j);
                    }
                    else if(total==1){
                        case1(b,a,i,j);
                        case4(b,a,i,j);
                    }
                    else{
                        case2(b,a,i,j);
                        case1(b,a,i,j);
                        case4(b,a,i,j);
                    }
                }
            }
            cout<<(int)b.size()<<"\n";
            for(int i=0;i<(int)b.size();i++){
                for(int j=0;j<6;j++)
                    cout<<b[i][j]+1<<" ";
                cout<<"\n";
            }
        }
 
        int main(){
            int T;
            cin>>T;
            while(T--)
            {
                solve();
            }
            return  0;
        }
```

PS.这个模拟把人写傻了