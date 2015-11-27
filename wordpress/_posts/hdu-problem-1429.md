title: '状态压缩下的BFS|HDU-Problem-1429'
tags:
  - ACM
  - BFS
  - HDU-Problem-1429
  - 状态压缩
id: 1011
categories:
  - 算法/数据结构
date: 2013-07-10 18:25:57
---

# 题目

[胜利大逃亡(续)](http://acm.hdu.edu.cn/showproblem.php?pid=1429 "http://acm.hdu.edu.cn/showproblem.php?pid=1429")

# 题目描述

Ignatius再次被魔王抓走了(搞不懂他咋这么讨魔王喜欢)……

这次魔王汲取了上次的教训，把Ignatius关在一个n*m的地牢里，并在地牢的某些地方安装了带锁的门，钥匙藏在地牢另外的某些地方。刚开始Ignatius被关在(sx,sy)的位置，离开地牢的门在(ex,ey)的位置。Ignatius每分钟只能从一个坐标走到相邻四个坐标中的其中一个。魔王每t分钟回地牢视察一次，若发现Ignatius不在原位置便把他拎回去。经过若干次的尝试，Ignatius已画出整个地牢的地图。现在请你帮他计算能否再次成功逃亡。只要在魔王下次视察之前走到出口就算离开地牢，如果魔王回来的时候刚好走到出口或还未到出口都算逃亡失败。

# Input

每组测试数据的第一行有三个整数n,m,t(2<=n,m<=20,t>0)。接下来的n行m列为地牢的地图，其中包括:

. 代表路
* 代表墙
@ 代表Ignatius的起始位置
^ 代表地牢的出口
A-J 代表带锁的门,对应的钥匙分别为a-j
a-j 代表钥匙，对应的门分别为A-J

每组测试数据之间有一个空行。

# Output

针对每组测试数据，如果可以成功逃亡，请输出需要多少分钟才能离开，如果不能则输出-1。

# Sample Input

4 5 17
@A.B.
a*.*.
*..*^
c..b*

4 5 16
@A.B.
a*.*.
*..*^
c..b*

# Sample Output

16
-1

# 出题人

LL

# 解题思路

因为要找小于t的步数的结果，所以很容易想到BFS，但是t值可能很大，不判重的话，很容易爆内存，所以引入哈希表来完成判重，这里的重复并不是简单的坐标，而是坐标加上钥匙，因为钥匙数量不多，可以用位压缩，将钥匙使用一个short int存放，使用它的低10位，因此一共至多有20*20*1024种结点，之后添加结点前先判重就可以通过BFS顺利AC。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;queue&gt;
#include &lt;deque&gt;
using namespace std;

class Data
{
public:
	Data():x(0),y(0),key(0) {}
	Data(int x,int y):x(x),y(y),key(0) {}
	~Data() {}

	/* data */
	unsigned short key; /*a-j的钥匙*/
	unsigned char x,y;      /*坐标*/
	unsigned short vaule;
};

/*状态压缩*/
bool hash[25][25][2500];
/*xy拓展方向*/
int index[][2] = {{0,-1},{0,1},{-1,0},{1,0}};

int main(int argc, char const *argv[])
{
	int n,m,t;
	char arr[25][25];
	queue&lt;Data&gt; DataBase;
	Data now;
	int vaule = 0;
	int i,j,z;
	while(cin &gt;&gt; n &gt;&gt; m &gt;&gt; t)
	{
		vaule = 0;
		/*清空hash表*/
		for(i = 0; i &lt; 25; ++i)
			for(j = 0; j &lt; 25; ++j)
				for(z = 0; z &lt; 1050; ++z)
					hash[i][j][z] = false;
		/*清空数据库*/
		while(!DataBase.empty()) DataBase.pop();
		/*读入数据*/
		for (i = 0; i &lt; n; ++i)
		{
			for (j = 0; j &lt; m; ++j)
			{
				Data temp;
				cin &gt;&gt; arr[i][j];
				if(arr[i][j]=='@')
				{
					temp.x = i;
					temp.y = j;
					temp.vaule = 0;
					hash[temp.x][temp.y][temp.key] = true; 
					DataBase.push(temp);
				}
			}
		}
		while(vaule &lt;= t)
		{
			if(DataBase.empty())
			{
				cout &lt;&lt; -1 &lt;&lt; endl;
				break;
			}
			now = DataBase.front();
			DataBase.pop();
			vaule = now.vaule;
			if(now.vaule == t)
			{
				cout &lt;&lt; -1 &lt;&lt; endl;
				break;
			}
			/*添加接下来可以走的地方*/
			for(z=0; z&lt;4; ++z)
			{
				char next =
					arr[now.x+index[z][0]][now.y+index[z][1]];
				if(now.y+index[z][1]&gt;=0 
					&amp;&amp; now.y+index[z][1]&lt; m
					&amp;&amp;now.x+index[z][0]&gt;=0 
					&amp;&amp; now.x+index[z][0]&lt; n	
					&amp;&amp; next != '*')
				{
					if(next&gt;='A' &amp;&amp; next&lt;='J')
					{
						if(now.key &amp; (1&lt;&lt;(next-'A')) ) /*有钥匙*/
						{
							Data temp(now);
							temp.x = now.x+index[z][0];
							temp.y = now.y+index[z][1];
							temp.vaule = now.vaule + 1;
							if(next&gt;='a' &amp;&amp; next&lt;='j')/*捡到钥匙*/
								temp.key |= (1 &lt;&lt; (next-'a'));    
							if(!hash[temp.x][temp.y][temp.key])
							{
								hash[temp.x][temp.y][temp.key]=1;
								DataBase.push(temp);
							}
						}
					}
					else
					{
						Data temp(now);
						temp.x = now.x+index[z][0];
						temp.y = now.y+index[z][1];
						temp.vaule = now.vaule + 1;
						if(next&gt;='a' &amp;&amp; next&lt;='j')/*捡到钥匙*/
							temp.key |= (1 &lt;&lt; (next-'a'));    
						if(!hash[temp.x][temp.y][temp.key])
						{
							hash[temp.x][temp.y][temp.key]=1;
							DataBase.push(temp);
						}
					}
				}
			}
			/*判断是否为出口*/
			if(arr[now.x][now.y] == '^')
			{
				cout &lt;&lt; now.vaule &lt;&lt; endl;
				break;
			}
		}
	}
	return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]