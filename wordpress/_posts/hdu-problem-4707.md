title: '直接BFS|Pet|HDU-4707'
tags:
  - '4707'
  - BFS
id: 1073
categories:
  - 算法/数据结构
date: 2013-09-25 10:40:22
---

# 题目

[Pet](http://acm.hdu.edu.cn/showproblem.php?pid=4707 "http://acm.hdu.edu.cn/showproblem.php?pid=4707")

# 题目描述

One day, Lin Ji wake up in the morning and found that his pethamster escaped. He searched in the room but didn’t find the hamster. He tried to use some cheese to trap the hamster. He put the cheese trap in his room and waited for three days. Nothing but cockroaches was caught. He got the map of the school and foundthat there is no cyclic path and every location in the school can be reached from his room. The trap’s manual mention that the pet will always come back if it still in somewhere nearer than distance D. Your task is to help Lin Ji to find out how many possible locations the hamster may found given the map of the school. Assume that the hamster is still hiding in somewhere in the school and distance between each adjacent locations is always one distance unit.

# Input

The input contains multiple test cases. Thefirst line is a positive integer T (0&lt;T&lt;=10), the number of test cases. For each test cases, the first line has two positive integer N (0&lt;N&lt;=100000) and D(0&lt;D&lt;N), separated by a single space. N is the number of locations in the school and D is the affective distance of the trap. The following N-1lines descripts the map, each has two integer x and y(0&lt;=x,y&lt;N), separated by a single space, meaning that x and y is adjacent in the map. Lin Ji’s room is always at location 0.

# Output

For each test case, outputin a single line the number of possible locations in the school the hamster may be found.

# Sample Input

1
10 2
0 1
0 2
0 3
1 4
1 5
2 6
3 7
4 8
6 9

# Sample Output

2

# 解题思路

一次BFS直接把那些距离在D内的找出来，用总数减一下就可以了

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;queue&gt;

using namespace std;

vector&lt;int&gt; arr[100010];

int dep[100010];

int bfs(int D)
{
    int nowData,next;
    int count = 1;
    dep[0] = 0;
    queue&lt;int&gt; DataBase;
    DataBase.push(0);
    while(!DataBase.empty())
    {
        nowData = DataBase.front();
        DataBase.pop();
        while(!arr[nowData].empty())
        {
            next = arr[nowData].back();
            dep[next] = dep[nowData] + 1;
            if(dep[next] &lt;= D)
            {
                ++count;
                DataBase.push(next);
            }
            arr[nowData].pop_back();
        }
    }
    return count;
}

int main()
{
    int T,N,D,t1,t2;
    cin &gt;&gt; T;
    while(T--)
    {
        cin &gt;&gt; N &gt;&gt; D;
        for(int i = 0;i &lt; N; i++)
	arr[i].clear();
        for(int i=0; i&lt;N-1; ++i)
        {
            cin &gt;&gt; t1 &gt;&gt; t2;
            arr[t1].push_back(t2);
        }
        cout &lt;&lt; N-bfs(D) &lt;&lt; endl;
    }
    return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]