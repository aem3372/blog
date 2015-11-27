title: 2013江西理工大学软件学院ACM新生热身赛题解
tags:
  - ACM
id: 1125
categories:
  - 算法/数据结构
date: 2013-11-17 02:19:15
---

比赛情况：[http://acm.hdu.edu.cn/diy/contest_ranklist.php?cid=21515&amp;page=1](http://acm.hdu.edu.cn/diy/contest_ranklist.php?cid=21515&amp;page=1)

赛后重挂：[http://acm.hdu.edu.cn/diy/contest_show.php?cid=21575](http://acm.hdu.edu.cn/diy/contest_show.php?cid=21575)

&nbsp;

[toggle title="第一题题解"]

# Problem Description

Aem为了提高大家的编程水平，举办了这么一场比赛，但是出题是苦恼的，出难了没人能写。为了保证大家至少能解决一道题，于是有了这道题，你的任务是输出一个字符串 "CONTEST START" 和 一个幸运数字（幸运数字在输入中给出）。
提示：字符串和幸运数字直接有一个空格。

# Input

一个幸运数字

# Output

CONTEST START + 空格 + 幸运数字
注意：最后还有一个换行符

# Sample Input

13

# Sample Output

CONTEST START 13

# Author

chuyuaem

# 解题思路

这题真的没啥好说的，直接上标程。

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
int main()
{
	int LUCKY;
	scanf(&quot;%d&quot;,&amp;LUCKY);
	printf(&quot;CONTEST START %d\n&quot;,LUCKY);
	return 0;
}
[/code]

[/toggle]

[toggle title="第二题题解"]

# Problem Description

Aem的当年的录取通知书附赠了两张手机卡，一张是移动的，一张是电信的。后来我选择了移动的手机卡，因为大一军训充值移动话费，可以免费拿到军训服。

这张移动的手机卡，每个月赠送了300M省内流量，50M国内流量，100条短信。在学校使用手机上网时，会优先消耗省内流量，只有当省内流量用完才会消耗国内流量。

当其中一项使用到80%时，移动的系统会发来一条短信通知。
当其中一项用完时，移动的系统也会发来一条短信通知。

（假设Aem一直在学校）现在Aem知道自己每个月在学校用了多少流量，发了多少短信...但是他不知道自己收到了多少条移动通知短信...但是马虎的Aem总是算不出自己到底收到了多少移动短信，于是他找到了未来的编程高手----你，想借助你编程的力量解决这个问题。

# Input

第一行一个数字N，表示你要统计之前N个月移动发来的短信通知。
接下来N行（分别对应前N个月，前N-1个月，前N-2个月的情况）
每行2个整数m和n，分别表示这个月在学校使用的流量（Aem每个月使用的流量都是整数哦）和发出的短信。

输入数据范围：
0 &lt; N &lt; 1000
0 &lt;= m &lt;= 350
0 &lt;= n &lt;= 100

# Output

结果只有一行，这一行只有一个数字，表示在移动发来短信通知的数量。
提示：因为是一行，所以数字后面要加上换行符。

# Sample Input

2
100 10
300 90

# Sample Output

3

# Author

chuyuaem

# 解题思路

300M的省内流量，50M的国内流量。那么对于使用流量达到240M时会收到一条短信，到达300M时会收到一条短信，到达340M时会收到一条短信，到达350M时会收到一条短信。
100条短信。那么对于发送短信到达80条时会收到一条短信，到达100条时会收到一条短信。
不停判断即可。

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
int main()
{
	int N,m,n;
	int res = 0;
	scanf(&quot;%d&quot;,&amp;N);
	while(N--)
	{
		scanf(&quot;%d%d&quot;,&amp;m,&amp;n);
		if(m&gt;=240)
			++res;
		if(m&gt;=300)
			++res;
		if(m&gt;=340)
			++res;
		if(m&gt;=350)
			++res;
		if(n&gt;=80)
			++res;
		if(n&gt;=100)
			++res;
	}
	printf(&quot;%d\n&quot;,res);
	return 0;
}
[/code]

[/toggle]

[toggle title="第三题题解"]

# Problem Description

你的任务是计算一个数字K的位数。

# Input

第一行是一个整数N，表示后面会有N行K。
0 &lt;= K &lt; 2^32

# Output

对于输入的每个K，你需要在相应的行输出K的位数。
如第二个K，对应的输出在第二行。

# Sample Input

2
12
7474

# Sample Output

2
4

# Author

chuyuaem

# 解题思路

有好几种思路呢。
首先，可以利用不断除10，去处理。（这种情况要注意数据范围，首先范围不能小于unsigned int，其次还要注意对0的处理。
其次，你可以用字符串读入，直接看字符串长度。
当然，还有一些其他方法，这里就不多说。

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
int main()
{
	int N;
	unsigned int K;
	scanf(&quot;%d&quot;,&amp;N);
	while(N--)
	{
		int res = 0;
		scanf(&quot;%d&quot;,&amp;K);
		if(K==0)
		{
			printf(&quot;1\n&quot;);
			continue;
		}
		while(K)
		{
			++res;
			K = K/10;
		}
		printf(&quot;%d\n&quot;,res);
	}
	return 0;
}
[/code]

[/toggle]

[toggle title="第四题题解"]

# Problem Description

Aem的英语非常之差，差到都无法用英文命题。所以你们现在看到的是中文，虽然Aem经常告诉自己要好好学英语，但是除了高考前夕英语突飞猛进了一回外，英语一直是弱到不行。听闻Aem上学期英语挂科了（好不容易才补考通过了），貌似12月份还有英语四级要考，据说英语四级成绩太差，会连成绩单都拿不到的。为了拿到成绩单（Aem第一次参加英语四级都没敢奢望通过），他找到了一份单词表，这张单词表特别长，每个单词一行，Aem想知道这里究竟有多少行，但是单词表实在是太长了。于是他想请身为未来编程高手--你帮帮他。你的任务是计算出单词表中单词的个数。

# Input

若干个单词，每个单词一行。
一个单词仅仅由大小写字母构成。
（输入结束标志应该以EOF为准，当scanf函数的返回值为EOF时，表示已经没有数据可以读了）

# Output

输出只有一行，这一行有一个数字，表示单词个数（提示：数字后有换行符）

# Sample Input

I
like
to
programme
and
make
them
solve
technical
problems
for
me

# Sample Output

12

# Author

chuyuaem

# 解题思路

也是很多方法，首先可以就纯粹的统计单词，首先设置一个开关，开始开关（标志是否在一个单词内部）是关闭的。
不断读入字符。
如果读入一个字符，开关是关闭的，并且是一个有效的字母（单词的开始），那么数值加一，将开关打开。
如果读入一个字符，开关是关闭的，并且不是一个有效的字母，那么什么也不做。
如果读入一个字符，开关是打开的，并且是一个有效的字符，也什么都不错。
如果读入一个字符，开关是打开的，并且是一个无效的单词（单词的结束），将开关关闭。

其他思路大致就是，通过统计换行符，统计字符串个数来完成。

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
#include &lt;stdio.h&gt;
int main()
{
	int flag=0;
	int res = 0;
	char t;
	while(scanf(&quot;%c&quot;,&amp;t) != EOF)
	{
		if(flag == 0 &amp;&amp;
		   ((t&gt;='A' &amp;&amp; t&lt;='Z') || (t&gt;='a' &amp;&amp; t&lt;='z'))
		  )
		{
			++res;
			flag = 1;
		}
		if(flag != 0 &amp;&amp;
		  ! ((t&gt;='A' &amp;&amp; t&lt;='Z') || (t&gt;='a' &amp;&amp; t&lt;='z'))
		  )
		{
			flag = 0;
		}
	}
	printf(&quot;%d\n&quot;,res);
	return 0;
}

[/code]

[/toggle]

[toggle title="第五题题解"]

# Problem Description

传闻很多同学觉得C语言只能够写出一个黑框框的程序。虽然C语言可以画出更加i精美的图案，但是今天的我想说的是，C语言的就算在黑框框下也是可以画图的。

今天我们就来画一些直角三角形吧。

例如：
*
**
***
****

我们今天要画的三角形外形大致就是这样。
具体点就是，我们根据指定的值H画三角形。（上面那个三角形H值为4，即底边上有H个星号，高上也有H个星号）

# Input

第一行是一个数字N，表示接下来要画N个三角形。
接下来N行，每行一个H值（H值含义，见题目描述）。

# Output

一些三角形。

# Sample Input

2
3
5

# Sample Output

*
**
***
*
**
***
****
*****

# Author

chuyuaem

# 解题思路

利用循环控制好输出即可。

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
int main()
{
	int T;
	int h;
	int i,j;
	scanf(&quot;%d&quot;,&amp;T);
	while(T--)
	{
		scanf(&quot;%d&quot;,&amp;h);
		for(i=1; i&lt;=h; ++i)
		{
			for(j=1; j&lt;=i; ++j)
				printf(&quot;*&quot;);
			printf(&quot;\n&quot;);
		}
	}
	return 0;
}
[/code]

[/toggle]

[toggle title = "第六题题解"]

# Problem Description

话说，热身赛虽然是信心赛，但是到现在为止递推，递归，贪心，排序什么都没有。这样不太好吧，万一有人直接通关，那岂不是太无趣了。
（当然，正式比赛可能都会有哦，之前对编程算法了解较少的同学，希望在正式比赛前充分准备，掌握一些基本的递推递归贪心等思想。）
所以Aem设置了一个小小的门槛，就是这道题了。
据说终结这道题，将会获得终结者称号（- -）。

Aem正在玩一个解密类游戏，但是他遇到了一个难题，这里有一些数，想要进入到下一关的话，就需要从数K中删去要求的N个数字使剩下的数字组成的数最大(不改变原有顺序)，为了解决这个问题，Aem请你动用编程的力量为他解决这一难题。

# Input

第一行有一个数T，表示有T组测试数据。
接下来的T行，每一行有两个数K和N ( 0&lt;= K &lt;= 2^31 ,N小于K的位数)。

# Output

对每一组数据输出删除若干数字后剩下的数字组成的那个最大数。

# Sample Input

1
917845 3

# Sample Output

985

# Author

chuyuaem

# 解题思路

贪心策略。因为一串数字要组成最大数一定是降序排列的，那么在不能改变原有序列上的删除一个数，也是在尽可能保证高位上的降序排列。（删除N个数，其实也就是N次删除1个数。）

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
#include &lt;math.h&gt;
#include &lt;string.h&gt;

//删去1个数字，使结果最大
void del(char* str)
{
	int i;
	int len = strlen(str);
	for(i=0; i&lt;len-1; ++i)
	{
		if(str[i]&lt;str[i+1])
		{
			strcpy(str+i,str+i+1);
			return ;
		}
	}
	strcpy(str+i,str+i+1);
}

int main()
{
	int i,n,T;
	unsigned k;
	char arr[100];
	scanf(&quot;%d&quot;,&amp;T);
	while(T--)
	{
		scanf(&quot;%u%d&quot;,&amp;k,&amp;n);
		sprintf(arr,&quot;%u&quot;,k);
		for(i=0; i&lt;n; ++i)
		del(arr);
		printf(&quot;%s\n&quot;,arr);
	}
	return 0;
} [/code]

[/toggle]

[toggle title ="第七题题解"]

# Problem Description

当你完成了终结题，说明你对问题的分析已经略有小成了。
当然，终结题并不是说，比赛结束了，而是以你的水平，足以踏上新的征程，你来参加这场比赛应该也是打酱油的吧。（其实我想说，地球已经不适合你了），那么来试试这题吧。这是为至少竞赛入门水平的人准备的。（也许你的语法还不够熟悉，但是你的分析能力已经成熟了）

一本书的页面从自然数1开始顺序编码直到自然数n。书的页码按照n的位数编排，不足时补充前导数字0。例如，n是149时，第6页用数字006表示，而不是06或6。现在要求你对给定书的总页码n，计算出书的全部页码分别用到多少次数字0,1，2，...，9.

# Input

有若干组数据，每组数据一行。 （输入结束标志应该以EOF为准，当scanf函数的返回值为EOF时，表示已经没有数据可以读了）
每行一个数字n。（1≤n≤10^9）

# Output

每组输入数据对应10行。分别是0-9的使用次数。
输出完一组数据后，额外输出一个换行。

# Sample Input

11
1

# Sample Output

10
4
1
1
1
1
1
1
1
1

0
1
0
0
0
0
0
0
0
0

# Author

chuyuaem

# 解题思路

寻找数学规律即可

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

int res[10];

void calc(int num)
{
	int i;
	int yz = num;
	int mid = 1;
	int ws = 1;
	while (yz /= 10)
	{
		/*计算中间值*/
		mid *= 10;
		/*计算位数*/
		ws += 1;
	}
	/*去除 00000...这个数字*/
	res[0] -= ws;
	while (mid)
	{
		/* 除去最高位剩下的数*/
		int ys = num % mid;
		/* 最高位*/
		int zg = num / mid;
		/* 最高位为k时，对于最高位为0~k-1的数，恰有mid个（0~mid-1）*/
		for (i = 0; i &lt; zg; ++i)
		{
			res[i] += mid;
		}
		/* 最高位为k时，对最高位为k的数恰有ys+1个（0~ys）*/
		res[zg] += ys + 1;
		/* 最高位为k时，对于最高位为0~k-1的数，余数是00000...~99999...一共mid个*/
		/* 在00000...~99999..该每个数字出现(ws-1)*mid/10次*/
		for (i = 0; i &lt; 10; ++i)
		{
			res[i] += (mid/10)* zg* (ws - 1) ;
		}
		num = ys;
		--ws;
		mid /= 10;
	}
}

int main()
{
	int i;
	int num;
	while (scanf(&quot;%d&quot;,&amp;num) != EOF)
	{
		memset(res, 0, sizeof(res));
		calc(num);
		for (i = 0; i &lt; 10; ++i)
		{
			printf(&quot;%d\n&quot;,res[i]);
		}
		printf(&quot;\n&quot;);
	}
	return 0;
}
[/code]

[/toggle]