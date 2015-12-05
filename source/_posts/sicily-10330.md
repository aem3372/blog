title: 'Cutting Sausages'
tags:
  - GCD
  - SOJ
id: 1303
categories:
  - 算法/数据结构
date: 2014-03-11 13:50:07
---

## 题目

[Cutting Sausages](http://soj.me/10330 "Cutting Sausages")

## 题目大意

有n根一样的香肠要切成若干段，现在要分给m个人，使每个人得到的一样多，至少切几刀？

## 解题思路

<!--
首先可以知道，N等于M时，一刀都不用切；N大于M时，当N是M的倍数时，一刀都不用切；当N大于M时，可以每人给(N div M)根,将N缩小到比M小；当N小于M时，如果M是N的整数倍，那么每根香肠分成M/N根就可以了，即需要切M/N-1刀，如果M不是N的整数倍，那么我们可以多切一些，即把每根香肠分成(M div N)+1根，即需要切M/N刀，将M=M%N（这个不明白，求大神指点），继续迭代就能得到答案。
-->

首先，将所有香肠串在一起，可以知道至多切m-1刀（可以看成是两把刻度不同等长的尺子，求上门有多少个刻度对上了）。
假设每根长l，那么总长就是n\*l，每人得到n\*l/m。
设，切q根后正好得到了p个人的份，q\*l=p\*n\*l/m，化简后得到p\*n=q\*m。
关于p和q有若干组解，这里要求最小的一组，可以发现p\*n=q\*m=LCM(n,m)。
接着可以得到q=n/GCD(n,m)，q的整数倍都可能是一个解，将公式化简为n/q=GCD(n,m)。
那么就知道已经有GCD(n,m)-1处是已经断开的（刻度对上的）。

<!--more-->

## 我的代码

```cpp
#include<iostream>

using namespace std;
//n>m
int gcd(int n,int m)
{
    if(m == 0)
        return n;
    return gcd(m,n%m);
}
int main()
{
    int n,m;
    cin >> n >> m;
    if(n>m)
        cout << m - gcd(n,m) << endl;
    else
        cout << m - gcd(m,n) << endl;    
}
```
