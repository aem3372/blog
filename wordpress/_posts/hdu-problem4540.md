title: '依然DP|威威猫系列故事——打地鼠|HDU-Problem-4540'
tags:
  - ACM
  - DP
id: 1039
categories:
  - 算法/数据结构
date: 2013-08-15 09:59:45
---

# 题目

[威威猫系列故事——打地鼠](http://acm.hdu.edu.cn/showproblem.php?pid=4540 "http://acm.hdu.edu.cn/showproblem.php?pid=4540")

# 题目描述

威威猫最近不务正业，每天沉迷于游戏“打地鼠”。
每当朋友们劝他别太着迷游戏，应该好好工作的时候，他总是说，我是威威猫，猫打老鼠就是我的工作！
无话可说...
我们知道，打地鼠是一款经典小游戏，规则很简单：每隔一个时间段就会从地下冒出一只或多只地鼠，玩游戏的人要做的就是打地鼠。
假设：
1、每一个时刻我们只能打一只地鼠，并且打完以后该时刻出现的所有地鼠都会立刻消失；
2、老鼠出现的位置在一条直线上，如果上一个时刻我们在x1位置打地鼠，下一个时刻我们在x2位置打地鼠，那么，此时我们消耗的能量为abs( x1 - x2 )；
3、打第一只地鼠无能量消耗。
现在，我们知道每个时刻所有冒出地面的地鼠位置，若在每个时刻都要打到一只地鼠，请计算最小需要消耗多少能量。

# Input

输入数据包含多组测试用例；
每组数据的第一行是2个正整数N和K（1 <= N <= 20, 1 <= K <= 10 )，表示有N个时刻，每个时刻有K只地鼠冒出地面；
接下来的N行，每行表示一个时刻K只地鼠出现的坐标（坐标均为正整数，且<=500）。

# Output

请计算并输出最小需要消耗的能量，每组数据输出一行。

# Sample Input

2 2
1 10
4 9
3 5
1 2 3 4 5
2 4 6 8 10
3 6 9 12 15

# Sample Output

1
1

# 解题思路

状态转移方程
DP[i][k] = min{for_each->x(DP[x][k-1])}
第k次选择打第i个位置所需最小能量是第k-1次打某个位置中的值加上这次所需能量得到的

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;cstdio&gt;
#include &lt;cmath&gt;

using namespace std;
int DP[510][30];
int t[510];
// DP[i][k] = min{for_each-&gt;x(DP[x][k-1])}
int main()
{
    int m,n;
    int i,j,w;
    while(cin &gt;&gt;m &gt;&gt; n)
    {
        for(i=0; i&lt;510; ++i)
            for(j=0; j&lt;30; ++j)
                DP[i][j] = 100000000;//拒绝标志
        for(i = 1; i&lt;=n; i++)//读入一行
        {
            int x;
            cin &gt;&gt;x;
            DP[x][0] = 0;
        }
        for(w=1; w&lt;m; ++w)
        {
            int x;
            for(i=1; i&lt;=n; i++)
            {
                cin &gt;&gt;x;
                DP[x][w] = DP[1][w-1]+ fabs(x-1);
                // DP[i][k] = min{for_each-&gt;x(DP[x][k-1])}
                for(j=2; j&lt;510; ++j)
                {
                    if(DP[j][w-1]+ fabs(x-j)&lt;DP[x][w])
                        DP[x][w] = DP[j][w-1]+ fabs(x-j);
                }
            }
        }
        int min = 100000000;
        for(i=1; i&lt;510; ++i)
            if(DP[i][m-1]&lt;min) min = DP[i][m-1];
        cout &lt;&lt; min &lt;&lt; endl;
    }
    return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]