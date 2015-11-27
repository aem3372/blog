title: 其实，英语题很简单 - ACM水题题解
tags:
  - ACM
  - 模拟
  - 算法
  - 英语
id: 410
categories:
  - 算法/数据结构
date: 2013-01-07 02:03:41
---

# 题目描述

xuanbin的英语一直拖着他的后腿，但是他最近找到了一种高效的学习方法。所以，他的英语功力呈直线往上飙。下面这题对于他来说，小菜一碟，现在试 试你的功力。
给你一段英文原文,由单词（每个单词长度小于等于20）和空格组成，以#字符结束，单词数小于等于300个。然后给出此段原文中的一个句子以#字符结
束，句子中最多包含30个单词（这个句子中挖去了一个单词用下划线代替），请你找出这个下划线上应填的单词并输出。（题目确保这个挖去的单词是一个
完整的单词，并且只有唯一的答案）。具体输入输出见样列。

# 样例输入

where is hero from #
where is _ from #

# 样例输出

hero

# 出题人

wenge&amp;xianbin

# 题解及代码

[note]
**<span style="color: #ff6600;">1.在英语考试前放出此题代码以求英语不挂。</span>**
** <span style="color: #ff6600;"> 2.因考试在即，日后写解析,见谅。</span>**
[/note]

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;string&gt;
#include &lt;vector&gt;
#include &lt;sstream&gt;
using namespace std;

vector&lt;string&gt; wordvec,question;
vector&lt;string&gt;::iterator first_seek(vector&lt;string&gt;::iterator itor)
{
	for(vector&lt;string&gt;::iterator i = wordvec.begin(); i != wordvec.end(); ++i)
		if( *i == *question.begin() )	return i;
		return wordvec.end();
}

vector&lt;string&gt;::iterator se_seek(vector&lt;string&gt;::iterator itor)
{
	for(vector&lt;string&gt;::iterator i = wordvec.begin(); i != wordvec.end(); ++i)
		if( *i == *(question.begin()+1) )	return i;
		return wordvec.end();
}

string seek(void)
{
	vector&lt;string&gt;::iterator first = wordvec.begin();
	if(*question.begin() != &quot;_&quot;)
		while((first = first_seek(first)) != wordvec.end())
		{
			vector&lt;string&gt;::iterator temp = first;
			string val;
			for(vector&lt;string&gt;::iterator itor = question.begin(); itor != question.end(); ++itor)
			{
				if(*itor == &quot;_&quot; &amp;&amp; itor != question.end()-1)
				{
					val = *temp;
					++temp;
					continue;
				}
				if(*itor == &quot;_&quot; &amp;&amp; itor == question.end()-1)
					return  *temp;
				if(*temp == *itor &amp;&amp; itor != question.end()-1)
				{
					++temp;
					continue;
				}
				if(*temp == *itor &amp;&amp; itor == question.end()-1)
					return val;
				if(*temp != *itor &amp;&amp; *itor != &quot;_&quot;)
					break;
			}
		}
	else
	{
		while((first = se_seek(first)) != wordvec.end())
		{
			vector&lt;string&gt;::iterator temp = first;
			string val;
			for(vector&lt;string&gt;::iterator itor = question.begin()+1; itor != question.end(); ++itor)
			{
				if(*temp == *itor &amp;&amp; itor != question.end()-1)
				{
					++temp;
					continue;
				}
				if(*temp == *itor &amp;&amp; itor == question.end()-1)
					return *(first-1);
				if(*temp != *itor)
					break;
			}
		}
	}
}
int main(void)
{
	string temp,str;

	getline(cin,str,'#');
	istringstream strm(str);
	while(strm &gt;&gt; temp)
		wordvec.push_back(temp);
	getline(cin,str,'#');
	strm.clear();
	strm.str(str);
	while(strm &gt;&gt; temp)
		question.push_back(temp);
	cout &lt;&lt; seek() &lt;&lt; endl;
	return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]