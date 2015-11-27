title: '横向打印二叉树|历届试题|蓝桥杯'
tags:
  - 历届试题
  - 练习系统
  - 蓝桥杯
id: 1248
categories:
  - 算法/数据结构
date: 2014-03-01 21:33:43
---

# 题目

[横向打印二叉树](http://lx.lanqiao.org/problem.page?gpid=T34 "http://lx.lanqiao.org/problem.page?gpid=T34")

# 题目描述

二叉树可以用于排序。其原理很简单：对于一个排序二叉树添加新节点时，先与根节点比较，若小则交给左子树继续处理，否则交给右子树。
当遇到空子树时，则把该节点放入那个位置。
比如，10 8 5 7 12 4 的输入顺序，应该建成二叉树如下图所示，其中.表示空白。
<pre>
...|-12
10-|
...|-8-|
.......|...|-7
.......|-5-|
...........|-4
</pre>
本题目要求：根据已知的数字，建立排序二叉树，并在标准输出中横向打印该二叉树。

# Input

输入数据为一行空格分开的N个整数。 N<100，每个数字不超过10000。
输入数据中没有重复的数字。

# Output

输出该排序二叉树的横向表示。为了便于评卷程序比对空格的数目，请把空格用句点代替。

# Sample Input 1

10 5 20

# Sample Output 1

<pre>
...|-20
10-|
...|-5
</pre>

# Sample Input 2

5 10 20 8 4 7

# Sample Output 2

<pre>
.......|-20
..|-10-|
..|....|-8-|
..|........|-7
5-|
..|-4
</pre>

# 解题思路

二叉搜索树加遍历，输出格式控制略麻烦。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;cstdio&gt;
using namespace std;

char G[110][450]={0};
int gp = 0;
char buf[450];

int max_clen = 0;
struct T
{
	/* data */
	T(): val(0),ltree(NULL),rtree(NULL) {}
	int val,c,len,no;/*值，横向树数字开始位置，长度,图行号*/
	T *ltree,*rtree;
	//~T() { cout &lt;&lt; &quot;del &quot; &lt;&lt; val &lt;&lt; endl;}
};

T* make(T* node, int val, int c)
{
	if(node == NULL)
	{
		node = new T;
		node-&gt;c = c;
		node-&gt;len = sprintf(buf,&quot;%d&quot;,val);
		node-&gt;val = val;
		if(c+node-&gt;len &gt; max_clen)
			max_clen = c + node-&gt;len;
	}
	else
	{
		if(val &lt; node-&gt;val)
			node-&gt;ltree = make(node-&gt;ltree, val, c+3+node-&gt;len);
		else
			node-&gt;rtree = make(node-&gt;rtree, val, c+3+node-&gt;len);
	}
	return node;
}

void print(T* node)
{
	if(node != NULL)
		cout &lt;&lt; node-&gt;val &lt;&lt; endl;
	if(node-&gt;ltree != NULL)
		print(node-&gt;ltree);
	if(node-&gt;rtree != NULL)
		print(node-&gt;rtree);
}

void hprint(T* node)
{
	if(node-&gt;rtree != NULL)
		hprint(node-&gt;rtree);
	if(node != NULL)
	{
		//cout &lt;&lt; node-&gt;val &lt;&lt; &quot;-&quot;&lt;&lt; node-&gt;c &lt;&lt; endl;
		for(int i=0; i&lt;node-&gt;c; ++i)
			G[gp][i] = '.';	
		sprintf(G[gp]+node-&gt;c,&quot;%d&quot;,node-&gt;val);
		node-&gt;no = gp;
		++gp;
	}
	if(node-&gt;ltree != NULL)
		hprint(node-&gt;ltree);
}
void linkTree(T* node)
{
	if(node-&gt;rtree != NULL)
	{
		G[node-&gt;no][node-&gt;c+node-&gt;len] = '-';
		for(int i=node-&gt;rtree-&gt;no; i&lt;=node-&gt;no; ++i)
			G[i][node-&gt;c+node-&gt;len+1] = '|';
		G[node-&gt;rtree-&gt;no][node-&gt;c+node-&gt;len+2] = '-';
		linkTree(node-&gt;rtree);
	}
	if(node != NULL)
	{

	}
	if(node-&gt;ltree != NULL)
	{
		G[node-&gt;no][node-&gt;c+node-&gt;len] = '-';
		for(int i=node-&gt;no; i&lt;=node-&gt;ltree-&gt;no; ++i)
			G[i][node-&gt;c+node-&gt;len+1] = '|';
		G[node-&gt;ltree-&gt;no][node-&gt;c+node-&gt;len+2] = '-';
		linkTree(node-&gt;ltree);
	}
}

void printTree()
{
	for(int i=0; i&lt;gp; ++i)
	{
		for(int j=0; j&lt;max_clen; ++j)
		{
			if(G[i][j])
				cout &lt;&lt; G[i][j];
		}
		cout &lt;&lt; endl;
	}
}

T* clear(T* node)
{
	if(node-&gt;ltree != NULL)
		node-&gt;ltree = clear(node-&gt;ltree);
	if(node-&gt;rtree != NULL)
		node-&gt;rtree = clear(node-&gt;rtree);
	if(node != NULL)
		delete node;
	return NULL;
}

int main()
{
	T* root = NULL;
	int t;

	while(cin &gt;&gt; t)
	{
		root = make(root,t,0);
	}
	//cout &lt;&lt; &quot;------------&quot; &lt;&lt; endl;
	//print(root);
	//cout &lt;&lt; &quot;------------&quot; &lt;&lt; endl;
	hprint(root);
	linkTree(root);
	printTree();
	//cout &lt;&lt; &quot;------------&quot; &lt;&lt; endl;
	root = clear(root);
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]