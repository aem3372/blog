title: 'Stone|2013-ACM-ICPC长春赛区网选-1006|HDU-4764'
tags:
  - '1006'
  - '2013'
  - '4764'
  - ACM
  - 网络选拔赛
  - 长春
id: 1089
categories:
  - 算法/数据结构
date: 2013-09-28 19:19:12
---

# 题目

Stone

# 时空限制

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)

# 题目描述

Tang and Jiang are good friends. To decide whose treat it is for dinner, they are playing a game. Specifically, Tang and Jiang will alternatively write numbers (integers) on a white board. Tang writes first, then Jiang, then again Tang, etc... Moreover, assuming that the number written in the previous round is X, the next person who plays should write a number Y such that 1 <= Y - X <= k. The person who writes a number no smaller than N first will lose the game. Note that in the first round, Tang can write a number only within range [1, k] (both inclusive). You can assume that Tang and Jiang will always be playing optimally, as they are both very smart students.

# Input

There are multiple test cases. For each test case, there will be one line of input having two integers N (0 < N <= 10^8) and k (0 < k <= 100). Input terminates when both N and k are zero.

# Output

For each case, print the winner's name in a single line.

# Sample Input

1 1
30 3
10 2
0 0

# Sample Output

Jiang
Tang
Jiang

# Source

2013 ACM/ICPC Asia Regional Changchun Online  

# Recommend

liuyiding

# 解题思路

这就是典型的线性棋游戏问题，采用逆推的思想可以轻易破解。首先数到N以及大于N的数是输的，因为两人都很聪明，所以当一人数到N-1时，那么另一人必然会输，我们称这个位置为必胜位；继续向前推，如果有人走到N-1-1至N-1-K范围内，另外一人必然会走到N-1，而自己必然会输，我们称这个位置为必输位...例如N=10，K=2时，（Y表示必胜位，N表示必输位）
<pre>
0  1  2  3  4  5  6  7  8  9  10  11
Y  N  N  Y  N  N  Y  N  N  Y  N   N  
</pre>
可以发现，0位置就是后报数的人的最终状态，因为这里是必胜位，而后报数的是Jiang，因此最终是Jiang获胜了。而必胜必输位是一个周期为K+1的循环，因此我们可以使用取模运算快速获取后报数人的结局。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
using namespace std;

//起始位置的输赢属性，这点是后手人的结局状态
bool check(int n,int k)
{
	return (n%(k+1) == 1);
}

int main()
{
	int N,K;
	while(cin &gt;&gt; N &gt;&gt; K, N)
	{
		if(check(N,K))
		{
			cout &lt;&lt; &quot;Jiang&quot; &lt;&lt; endl;
		}
		else
		{
			cout &lt;&lt; &quot;Tang&quot; &lt;&lt; endl;
		}
	}
	return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]