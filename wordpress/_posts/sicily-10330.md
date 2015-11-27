title: '最大公约数|Cutting Sausages|Sicily-10330'
tags:
  - '10330'
  - ACM
  - sicily
id: 1303
categories:
  - 算法/数据结构
date: 2014-03-11 13:50:07
---

# 题目

[Cutting Sausages](http://soj.me/10330 "http://soj.me/10330")

# 题目描述

Mirko has given up on the difficult coach job and switched to food tasting instead. Having skipped breakfast like a professional connoisseur, he is visiting a Croatian cured meat festival. The most renowned cook at the festival, Marijan Bajs, has prepared N equal sausages which need to be distributed to M tasters such that each taster gets a precisely equal amount. He will use his trusted knife to cut them into pieces. 
In order to elegantly divide the sausages, the number of cuts splitting individual sausages must be as small as possible. For instance, if there are two sausages and six tasters (the first test case below), it is sufficient to split each sausage into three equal parts, making a total of four cuts. On the other hand, if there are three sausages and four tasters (the second test case below), one possibility is cutting off three quarters of each sausage. Those larger parts will each go to one of the tasrers, while the fourth taster will get the three smaller pieces (quarters) left over.
Mirko wants to try the famous sausages, so he volunteered to help Bajs. Help them calculate the minimum total number of cuts needed to carry out the desired division. 

# Input

The first and only line of input contains two positive integers, N and M (1 ≤ N, M ≤ 100), the number of sausages and tasters, respectively.

# Output

The first and only line of output must contain the required minimum number of cuts.

# Sample Input 1

样例1：
2 6
样例2：
3 4
样例3：
6 2

# Sample Output 1

样例1：
4
样例2：
3
样例3：
0

# Source

COCI 2013.9/2014年每周一赛第一场

# 题目大意

有n根一样的香肠要切成若干段，现在要分给m个人，使每个人得到的一样多，至少切几刀？

# 解题思路

<!--
首先可以知道，N等于M时，一刀都不用切；N大于M时，当N是M的倍数时，一刀都不用切；当N大于M时，可以每人给(N div M)根,将N缩小到比M小；当N小于M时，如果M是N的整数倍，那么每根香肠分成M/N根就可以了，即需要切M/N-1刀，如果M不是N的整数倍，那么我们可以多切一些，即把每根香肠分成(M div N)+1根，即需要切M/N刀，将M=M%N（这个不明白，求大神指点），继续迭代就能得到答案。
-->
[![西西里岛10330解题示意图](http://www.aemiot.com/wp-content/uploads/2014/03/sicily-10330.gif)](http://www.aemiot.com/wp-content/uploads/2014/03/sicily-10330.gif)

首先，将所有香肠串在一起，可以知道至多切m-1刀（可以看成是两把刻度不同等长的尺子，求上门有多少个刻度对上了）。
假设每根长l，那么总长就是n*l，每人得到n*l/m。
设，切q根后正好得到了p个人的份，q*l=p*n*l/m，化简后得到p*n=q*m。
关于p和q有若干组解，这里要求最小的一组，可以发现p*n=q*m=LCM(n,m)。
接着可以得到q=n/GCD(n,m)，q的整数倍都可能是一个解，将公式化简为n/q=GCD(n,m)。
那么就知道已经有GCD(n,m)-1处是已经断开的（刻度对上的）。

# 我的代码

[code lang="cpp"]
#include&lt;iostream&gt;

using namespace std;
//n&gt;m
int gcd(int n,int m)
{
    if(m == 0)
        return n;
    return gcd(m,n%m);
}
int main()
{
    int n,m;
    cin &gt;&gt; n &gt;&gt; m;
    if(n&gt;m)
        cout &lt;&lt; m - gcd(n,m) &lt;&lt; endl;
    else
        cout &lt;&lt; m - gcd(m,n) &lt;&lt; endl;    
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]