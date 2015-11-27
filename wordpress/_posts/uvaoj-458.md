title: ' The Decoder|算法竞赛入门习题|UVaOJ-458'
tags:
  - '458'
  - ACM
  - UVaOJ
id: 1119
categories:
  - 算法/数据结构
date: 2013-11-11 23:11:33
---

# 题目

[The Decoder](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=94&page=show_problem&problem=399 "The Decoder")

# 时空限制

Time limit: 1.000 seconds

# 题目描述

Write a complete program that will correctly decode a set of characters into a valid message. Your program should read a given file of a simple coded set of characters and print the exact message that the characters contain. The code key for this simple coding is a one for one character substitution based upon a single arithmetic manipulation of the printable portion of the ASCII character set. 

# Input

For example: with the input file that contains: 

1JKJ'pz'{ol'{yhklthyr'vm'{ol'Jvu{yvs'Kh{h'Jvywvyh{pvu5
1PIT'pz'h'{yhklthyr'vm'{ol'Pu{lyuh{pvuhs'I|zpulzz'Thjopul'Jvywvyh{pvu5
1KLJ'pz'{ol'{yhklthyr'vm'{ol'Kpnp{hs'Lx|pwtlu{'Jvywvyh{pvu5

# Output

your program should print the message: 
*CDC is the trademark of the Control Data Corporation.
*IBM is a trademark of the International Business Machine Corporation.
*DEC is the trademark of the Digital Equipment Corporation.

Your program should accept all sets of characters that use the same encoding scheme and should print the actual message of each set of characters. 

# Sample Input

1JKJ'pz'{ol'{yhklthyr'vm'{ol'Jvu{yvs'Kh{h'Jvywvyh{pvu5
1PIT'pz'h'{yhklthyr'vm'{ol'Pu{lyuh{pvuhs'I|zpulzz'Thjopul'Jvywvyh{pvu5
1KLJ'pz'{ol'{yhklthyr'vm'{ol'Kpnp{hs'Lx|pwtlu{'Jvywvyh{pvu5

# Sample Output

*CDC is the trademark of the Control Data Corporation.
*IBM is a trademark of the International Business Machine Corporation.
*DEC is the trademark of the Digital Equipment Corporation.

# Soure

UVaOJ

# 解题思路

从样例可以看出就是ASCII值减去7.值得注意的是，用c++的时候注意流设置要关闭空白符跳过。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;string&gt;
#include &lt;iomanip&gt;
using namespace std;
int main()
{
    char t;
    while(cin &gt;&gt; noskipws, cin &gt;&gt; t)
	{
		cout &lt;&lt; (char)((t=='\n')? '\n':t-7);
	}
    return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]