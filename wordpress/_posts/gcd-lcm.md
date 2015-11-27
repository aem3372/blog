title: '易于编程的最大公约数算法和最小公倍数算法|HDU-Problem-1019'
tags:
  - 最大公约数
  - 最小公倍数
  - 欧几里得算法
id: 662
categories:
  - 算法/数据结构
date: 2013-02-24 00:09:04
---

题目：杭电Problem-1019      [Least Common Multiple](http://acm.hdu.edu.cn/showproblem.php?pid=1019)

题目简述：计算一串数字的最小公倍数

思路：求一串数字的最小公倍数化为多次求2个数字求最小公倍数。求最小公倍数可以通过最大公约数得出，其中最大公约数可以使用欧几里得算法求出。

[toggle title="最大公约数算法(欧几里得算法)"]
假设要求A，B 两个数字（A>B）的最大公约数，GCD(A,B)=GCD(C,D)，其中C = B,D = A mod B。多次利用公式可以将化为GCD(N,0)的状态，那么N 就是我们要求的最大公约数。
例如，我们要求1286和78的最大公约数。
GCD(1286,78)
=GCD(78,1286 mod 78) = GCD(78,38)
=GCD(38,78 mod 38) = GCD(38,2)
=GCD(2,38 mod 2) = GCD(2,0)
由此知道最大公约数为2。
[/toggle]

[toggle title="最小公倍数算法(利用最大公约数的算法)"]
假设要求A，B两个数字的最小公倍数，只需求出最大公约数，然后用两数之积除以最大公约数就可得到最小公倍数。
例如，我们要求1286和78的最小公倍数，利用之前的方法可以知道最大公约数是2。那么最小公倍数就是1286 * 78 / 2 = 50154。
[/toggle]

利用上述算法，可以很容易写出此题的AC代码。
附上我的代码：
[code lang="cpp"]
#include &lt;stdio.h&gt;
typedef long long NUM;

NUM ancalc(NUM t1,NUM t2)
{
    int t;
    if(t1&gt;t2) t=t1,t1=t2,t2=t;
    while(t2!=0)
    {
        t=t2;
        t2 = t1%t2;
        t1 = t;
    }
    return t1;
}

NUM calc(NUM t1,NUM t2)
{
    int t;
    return t1*t2/ancalc(t1,t2);
}
int main(void)
{
    int i,j,n,l,t1,t2;
    scanf(&quot;%d&quot;,&amp;n);
    for(i=0;i&lt;n;++i)
    {
        scanf(&quot;%d&quot;,&amp;l);
        scanf(&quot;%d&quot;,&amp;t1);
        for(j=0;j&lt;l-1;++j)
        {
            scanf(&quot;%d&quot;,&amp;t2);
            t1 = calc(t1,t2);
        }
        printf(&quot;%d\n&quot;,t1);
    }
    return 0;
}
[/code]
[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]