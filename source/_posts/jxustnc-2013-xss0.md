title: 书的页码
id: 1125
categories:
  - 算法/数据结构
date: 2013-11-17 02:19:15
---

## 题目

一本书的页面从自然数1开始顺序编码直到自然数n。书的页码按照n的位数编排，不足时补充前导数字0。例如，n是149时，第6页用数字006表示，而不是06或6。现在要求你对给定书的总页码n，计算出书的全部页码分别用到多少次数字0,1，2，...，9.

<!--more-->

## 输入格式

有若干组数据，每组数据一行。 （输入结束标志应该以EOF为准，当scanf函数的返回值为EOF时，表示已经没有数据可以读了）
每行一个数字n。（1≤n≤10^9）

## 输出格式

每组输入数据对应10行。分别是0-9的使用次数。
输出完一组数据后，额外输出一个换行。

## 我的代码

```cpp
#include <stdio.h>
#include <string.h>

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
		for (i = 0; i < zg; ++i)
		{
			res[i] += mid;
		}
		/* 最高位为k时，对最高位为k的数恰有ys+1个（0~ys）*/
		res[zg] += ys + 1;
		/* 最高位为k时，对于最高位为0~k-1的数，余数是00000...~99999...一共mid个*/
		/* 在00000...~99999..该每个数字出现(ws-1)*mid/10次*/
		for (i = 0; i < 10; ++i)
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
	while (scanf("%d",&num) != EOF)
	{
		memset(res, 0, sizeof(res));
		calc(num);
		for (i = 0; i < 10; ++i)
		{
			printf("%d\n",res[i]);
		}
		printf("\n");
	}
	return 0;
}
```