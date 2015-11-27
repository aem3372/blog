title: '水题-输入输出练习|A Computer Graphics Problem|HDU-4716'
tags:
  - '4716'
  - 水题
  - 输入输出
id: 1067
categories:
  - 算法/数据结构
date: 2013-09-25 10:32:21
---

# 题目

[A Computer Graphics Problem](http://acm.hdu.edu.cn/showproblem.php?pid=4716 "http://acm.hdu.edu.cn/showproblem.php?pid=4716")

# 题目描述

In this problem we talk about the study of Computer Graphics. Of course, this is very, very hard.
We have designed a new mobile phone, your task is to write a interface to display battery powers.
Here we use '.' as empty grids.
When the battery is empty, the interface will look like this:
<pre>*------------*
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
*------------*</pre>
When the battery is 60% full, the interface will look like this:
<pre>*------------*
|............|
|............|
|............|
|............|
|------------|
|------------|
|------------|
|------------|
|------------|
|------------|
*------------*</pre>
Each line there are 14 characters.
Given the battery power the mobile phone left, say x%, your task is to output the corresponding interface. Here x will always be a multiple of 10, and never exceeds 100.

# Input

The first line has a number T (T &lt; 10) , indicating the number of test cases.
For each test case there is a single line with a number x. (0 &lt; x &lt; 100, x is a multiple of 10)

# Output

For test case X, output "Case #X:" at the first line. Then output the corresponding interface.
See sample output for more details.

# Sample Input

2
0
60

# Sample Output

Case #1:
<pre>*------------*
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
*------------*</pre>
Case #2:
<pre>*------------*
|............|
|............|
|............|
|............|
|------------|
|------------|
|------------|
|------------|
|------------|
|------------|
*------------*</pre>

# 解题思路

# 我的代码

[code lang="c"]
#include&lt;stdio.h&gt;
int main()
{
	int T,t,k,i;
	scanf(&quot;%d&quot;,&amp;T);
	for(i=1; i&lt;=T; ++i)
	{
		scanf(&quot;%d&quot;,&amp;t);
		printf(&quot;Case #%d:\n&quot;,i);
		printf(&quot;*------------*\n&quot;);
		for(k=0; k&lt;10-t/10; ++k)
			printf(&quot;|............|\n&quot;);
		for(k=0; k&lt;t/10; ++k)
			printf(&quot;|------------|\n&quot;);
		printf(&quot;*------------*\n&quot;);
	}
	return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]