title: '横向打印二叉树'
tags:
  - 蓝桥杯
id: 1248
categories:
  - 算法/数据结构
date: 2014-03-01 21:33:43
---

## 题目

[横向打印二叉树](http://lx.lanqiao.org/problem.page?gpid=T34 "http://lx.lanqiao.org/problem.page?gpid=T34")

## 题目描述

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

<!--more-->

## Input

输入数据为一行空格分开的N个整数。 N<100，每个数字不超过10000。
输入数据中没有重复的数字。

## Output

输出该排序二叉树的横向表示。为了便于评卷程序比对空格的数目，请把空格用句点代替。

## Sample Input 1

10 5 20

## Sample Output 1

<pre>
...|-20
10-|
...|-5
</pre>

## Sample Input 2

5 10 20 8 4 7

## Sample Output 2

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

```cpp
#include <iostream>
#include <cstdio>
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
	//~T() { cout << "del " << val << endl;}
};

T* make(T* node, int val, int c)
{
	if(node == NULL)
	{
		node = new T;
		node->c = c;
		node->len = sprintf(buf,"%d",val);
		node->val = val;
		if(c+node->len > max_clen)
			max_clen = c + node->len;
	}
	else
	{
		if(val < node->val)
			node->ltree = make(node->ltree, val, c+3+node->len);
		else
			node->rtree = make(node->rtree, val, c+3+node->len);
	}
	return node;
}

void print(T* node)
{
	if(node != NULL)
		cout << node->val << endl;
	if(node->ltree != NULL)
		print(node->ltree);
	if(node->rtree != NULL)
		print(node->rtree);
}

void hprint(T* node)
{
	if(node->rtree != NULL)
		hprint(node->rtree);
	if(node != NULL)
	{
		//cout << node->val << "-"<< node->c << endl;
		for(int i=0; i<node->c; ++i)
			G[gp][i] = '.';	
		sprintf(G[gp]+node->c,"%d",node->val);
		node->no = gp;
		++gp;
	}
	if(node->ltree != NULL)
		hprint(node->ltree);
}
void linkTree(T* node)
{
	if(node->rtree != NULL)
	{
		G[node->no][node->c+node->len] = '-';
		for(int i=node->rtree->no; i<=node->no; ++i)
			G[i][node->c+node->len+1] = '|';
		G[node->rtree->no][node->c+node->len+2] = '-';
		linkTree(node->rtree);
	}
	if(node != NULL)
	{

	}
	if(node->ltree != NULL)
	{
		G[node->no][node->c+node->len] = '-';
		for(int i=node->no; i<=node->ltree->no; ++i)
			G[i][node->c+node->len+1] = '|';
		G[node->ltree->no][node->c+node->len+2] = '-';
		linkTree(node->ltree);
	}
}

void printTree()
{
	for(int i=0; i<gp; ++i)
	{
		for(int j=0; j<max_clen; ++j)
		{
			if(G[i][j])
				cout << G[i][j];
		}
		cout << endl;
	}
}

T* clear(T* node)
{
	if(node->ltree != NULL)
		node->ltree = clear(node->ltree);
	if(node->rtree != NULL)
		node->rtree = clear(node->rtree);
	if(node != NULL)
		delete node;
	return NULL;
}

int main()
{
	T* root = NULL;
	int t;

	while(cin >> t)
	{
		root = make(root,t,0);
	}
	//cout << "------------" << endl;
	//print(root);
	//cout << "------------" << endl;
	hprint(root);
	linkTree(root);
	printTree();
	//cout << "------------" << endl;
	root = clear(root);
}
```