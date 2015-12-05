title: 'Gardon吃糖果'
id: 766
categories:
  - 算法/数据结构
date: 2013-04-05 01:02:57
---

## 题目

HOHO，终于从Speakless手上赢走了所有的糖果，是Gardon吃糖果时有个特殊的癖好，就是不喜欢将一样的糖果放在一起吃，喜欢先吃一种，下一次吃另一种，这样；可是Gardon不知道是否存在一种吃糖果的顺序使得他能把所有糖果都吃完？请你写个程序帮忙计算一下。

<!--more-->

## 输入

第一行有一个整数T，接下来T组数据，每组数据占2行，第一行是一个整数N（0<N<=1000000)，第二行是N个数，表示N种糖果的数目Mi(0<Mi<=1000000)。

## 输出

对于每组数据，输出一行，包含一个"Yes"或者"No"。

## 样例输入

2
3
4 1 1
5
5 4 3 2 1

## 样例输出

No
Yes

## Hint

Hint
Please use function scanf 

## Author

Gardon 

## Source

Gardon-DYGG Contest 2 

## 解题思路

这是一个组合数学的问题呢，吃糖果的顺序构成一个序列，这个序列是由我们来排的，不连续吃同一种糖其实就是序列中没有两个相邻的是同一种糖。那么可以设想将最多的一种糖（假设有max个）果先全部加入序列，然后将余下的糖果插入其中，如果将问题建立在这样一个数学模型上，那么显然只有余下的糖少于max-1个时才可能会出现不符合题意的序列。因此，我们只要找出最大值max，如果max<=sum-max+1就存在，否则不存在。

## 我的代码

```cpp
#include <iostream>
using namespace std;
int main()
{
     int T,n,m,a,max;
     int i,j;
     long long sum;
     cin >> T;
     while(T--)
     {
         sum=0; max=0;
         cin >> n;
         for (j=0;j<n;j++)
         {
             cin >> a;
             if (a>max) max=a;
             sum+=a;
         }
         if (sum-max+1<max) 
  cout << "No" << endl;
         else 
  cout << "Yes" << endl;
     }
    return 0;
}
```
