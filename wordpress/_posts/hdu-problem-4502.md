title: 'DP解法|吉哥系列故事——临时工计划|Problem - 4502'
tags:
  - '4502'
  - DP
id: 1025
categories:
  - 算法/数据结构
date: 2013-08-08 11:28:25
---

# 题目

[吉哥系列故事——临时工计划](http://acm.hdu.edu.cn/showproblem.php?pid=4502 "http://acm.hdu.edu.cn/showproblem.php?pid=4502")

# 题目描述

　　俗话说一分钱难倒英雄汉，高中几年下来，吉哥已经深深明白了这个道理，因此，新年开始存储一年的个人资金已经成了习惯，不过自从大学之后他不好意思再向大人要压岁钱了，只能把唯一的希望放到自己身上。可是由于时间段的特殊性和自己能力的因素，只能找到些零零碎碎的工作，吉哥想知道怎么安排自己的假期才能获得最多的工资。
　　已知吉哥一共有m天的假期，每天的编号从1到m，一共有n份可以做的工作，每份工作都知道起始时间s，终止时间e和对应的工资c，每份工作的起始和终止时间以天为单位(即天数编号)，每份工作必须从起始时间做到终止时间才能得到总工资c，且不能存在时间重叠的工作。比如，第1天起始第2天结束的工作不能和第2天起始，第4天结束的工作一起被选定，因为第2天吉哥只能在一个地方工作。
　　现在，吉哥想知道怎么安排才能在假期的m天内获得最大的工资数（第m+1天吉哥必须返回学校，m天以后起始或终止的工作是不能完成的）。

# Input

第一行是数据的组数T；每组数据的第一行是2个正整数：假期时间m和可做的工作数n；接下来n行分别有3个正整数描述对应的n个工作的起始时间s，终止时间e，总工资c。

[数据范围]
1 <= T <= 1000
9 < m <= 100
0 < n <= 1000
s <= 100, e <= 100, s <= e
c <= 10000

# Output

对于每组数据，输出吉哥可获得的最高工资数。

# Sample Input

1
10 5
1 5 100
3 10 10
5 10 100
1 4 2
6 12 266

# Sample Output

102

# 解题思路

状态转移方程
MAX[Time] = { MAX[Time-1],for_each(MAX[ list[Time][k].begin-1 ]+list[Time][k].cost) }
某一天能获得的最大收入，要么是前一天的最大收入，要么是在这一天结束的某个任务获得的收入加上这个任务开始前获得的最大收入

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
#define SMAX 1005
#define EMAX 1005

/*risk对象拥有开始时间，和结束时能获得的收入*/
struct risk{
    int begin,cost;
};

struct risk list[SMAX][EMAX];
int list_count[SMAX];
/*list[i] 是在第i天完成的一个任务组*/
/*list_count[i] 是在第i天结束的任务数量*/

int DP_MAX[SMAX];

int check(int s,int e,int c,int m)
{
    if(e&gt;m) return 0; /*不能及时返回学校*/
    return 1;
}

void clear(void)
{
    int i;
    for(i=0; i&lt;SMAX; ++i) 
        list_count[i] = 0;
}

int main(void)
{
    int T,m,n,s,e,c;
    int i,j;
    struct risk temp;
    scanf(&quot;%d&quot;,&amp;T);
    while(T--)
    {
        clear();
        scanf(&quot;%d%d&quot;,&amp;m,&amp;n);
        /*读入数据，并预处理*/
        for(i=0; i&lt;n; ++i)
        {
            scanf(&quot;%d%d%d&quot;,&amp;s,&amp;e,&amp;c);
            if(check(s,e,c,m))
            {
                temp.begin = s; temp.cost = c;
                list[e][list_count[e]++] = temp;
            }
        }
        DP_MAX[0] = 0; /*没有时间，当然没有收入*/
        for(i=1; i&lt;=m; ++i)/*假期时间*/
        {
            DP_MAX[i] = DP_MAX[i-1];
            for(j=0; j&lt;list_count[i]; ++j)
            {
                int now_cost = 
                    DP_MAX[ list[i][j].begin - 1 ] + list[i][j].cost;
                if( now_cost &gt; DP_MAX[i])
                    DP_MAX[i] = now_cost;
            }
        }
            printf(&quot;%d\n&quot;,DP_MAX[m]);
    }
    return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]