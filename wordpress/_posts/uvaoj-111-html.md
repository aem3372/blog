title: '动规|LCS|History Grading|UVaOJ-111'
tags:
  - ACM
  - UVaOJ
id: 1359
categories:
  - 算法/数据结构
date: 2014-04-28 18:29:59
---

# 题目

[History Grading
](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=114&page=show_problem&problem=47 "http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=114&page=show_problem&problem=47")

# 题目大意

历史考试中，要学生对一系列历史事件按发生事件排序。以学生填写的答案和标准答案相同的最多项目计算得分。(理论是为了避免一错全错的情况)

# 解题思路

又见LCS。注意输入数据的内容。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
using namespace std;
int n;
int a[25],r[25];
int dp[25][25];
int main()
{
	cin &gt;&gt; n;
	for(int i=1; i&lt;=n; ++i)
	{
		int t;
		cin &gt;&gt; t;
		a[t] = i;
	}
	while(1)
	{
		for(int i=1; i&lt;=n; ++i)
		{
			int t;
			cin &gt;&gt; t;
			r[t] = i;
		}
		if(!cin)
			break;
		//LCS
		//f[i,j] = f[i-1,j-1]+1 where a[i]==r[j]
		//		   max{f[i-1,j],f[i,j-1]} where a[i]!=r[j]
		for(int i=1; i&lt;=n; ++i)
			for(int j=1; j&lt;=n; ++j)
			{
				if(a[i] == r[j])
					dp[i][j] = dp[i-1][j-1] + 1;
				else if(dp[i-1][j]&gt;dp[i][j-1])
					dp[i][j] = dp[i-1][j];
				else
					dp[i][j] = dp[i][j-1];
			}
		cout &lt;&lt; dp[n][n] &lt;&lt; endl;
	}
	return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]