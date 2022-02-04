---
title: test_code
tags:
  - dp
  - ms面试题
categories:
  - - 面试题
    - MS
abbrlink: 55689
date: 2022-01-30 13:32:07
---
## 三维dp
### dqwdwq
==dwqd==
![顾客购物程序的类图](http://hz-picbed-xxh.oss-cn-hangzhou.aliyuncs.com/img-bed/3-1Q113131610L7.gif)
$\sum\_{x}^{sd}{\frac{a}{b}+{a}\over{\alpha}}$
<!--more-->
$$
a*b^2 \quad (b \omega)
$$

`test for ms code `
```c++
/**
 * @Descripttion: 111 https://leetcode.com/discuss/interview-question/1488563/microsoft-on-campus-2021-india
 * @version: 
 * @Author: xxh
 * @Date: 2022-01-23
 */
#include<bits/stdc++.h>
using namespace std;
vector<vector<int>>dp;
int dd[100][2][200]={-1} ;//pos - -x/y - value
int dfs(vector<int>& h,int pos,int rx,int ry,int n){
    if(pos>=n) return 0;
    if(dp[pos][abs(rx-ry)]!=-1)return dp[pos][abs(rx-ry)];
    int v1=0,v2=0,v3=0;
    if(rx>=h[pos])v1 = 1+dfs(h,pos+1,rx-h[pos],ry,n);
    if(ry>=h[pos]) v2 = 1+dfs(h,pos+1,rx,ry-h[pos],n);
    v3=dfs(h,pos+1,rx,ry,n);
    dp[pos][abs(rx-ry)] = max(v1,max(v2,v3));
    return dp[pos][abs(rx-ry)];
}

int f(vector<int>& h,int pos,int x,int y,int n){
    dd[0][0][x]=dd[0][1][y]=0;
    for(int i=1;i<=n;i++){
        
        for(int j = x;j>=h[i-1];j--) //x
            for(int k=y;k>=0;k--){ //y
                if(dd[i-1][1][k]!=-1&&dd[i-1][0][j]!=-1) {
                    dd[i][0][j-h[i-1]]= max(dd[i-1][1][k]+dd[i-1][0][j]+1,dd[i][0][j-h[i-1]]);
                    cout<<i<<" "<<j<<" "<<dd[i][0][j-h[i-1]]<<endl;
                }
            }
            
        for(int j = y;j>=h[i-1];j--)
            for(int k=x;k>=0;k--){
                if(dd[i-1][0][k]!=-1&&dd[i-1][1][j]!=-1) dd[i][1][j-h[i-1]] = max(dd[i-1][0][k]+dd[i-1][1][j]+1,dd[i][1][j-h[i-1]]);

            }
        
    }
    int mx =0;
    for(int i = 0;i<100;i++){
        //cout<<dd[n-1][0][i];
        mx = max(mx,dd[n-1][0][i]);
        }
    int my = 0;
    for(int i = 0;i<100;i++){
        //cout<<dd[n-1][1][i];
        my = max(my,dd[n-1][1][i]);
        }
    return max(mx,my);

}
int main()
{
    vector<int>h={2,3,1,2,6,5,4};
    int x = 8,y=9;
    int n = h.size();
    dp.resize(n,vector<int>(max(x,y)+1,-1));
    memset(dd,0xff,sizeof(dd));
    cout<<f(h,0,x,y,n);
    return 0;
}
```
