title: 传统高精度算法系列(一)整数的高精度加法
tags:
  - 加法
  - 算法
  - 高精度
id: 353
categories:
  - 算法/数据结构
date: 2012-12-30 20:28:21
---

大家都知道，C语言的基本数据类型提供的可计算范围非常有限的
那么我们需要处理大数据时怎么办呢
其实有一种方法可以很轻松的解决这个问题
那就是**高精度算法**。

首先，我们介绍整数的高精度加法

算法的思路：
**模拟我们在小学所学的笔算**
（对于不足位用0补齐，如9818+13，我们认为是9818+0013）
假设我们要计算的两数分别是 A1 A2 A3 A4 、 B1 B2 B3 B4
那么我们笔算时，会列出这样的式子

A1 A2 A3 A4
+ B1  B2  B3 B4
---------------------
C0  C1 C2 C3 C4

那么我们在计算机中也同样可以完成这个过程
为了方便进位，我们在计算前，将两个数反向储存至数组中
即我们是对  B4 B3 B2 B1 、A4 A3 A2 A1 进行计算

初始化时 dv = 0
其中有恒等式   Cn  = （An + Bn + dv）mod  10  （dv为进位结果）
根据这个等式我们只需要循环执行以下三步，就能得到大致结果

1. **temp = An + Bn +dv**
2\. **Cn = temp mod 10**
3\. **dv = temp div 10**

循环完成后，我们还需要进行最后一步
因为两个最高位相加仍可能产生进位
所以，我们在这里还要额外做一次对 dv 的判断
如果 dv 值为 1 的话， Cn+1 应该要进位，即 Cn+1 = 1

该算法用C语言描述如下：

[code lang="cpp"]
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#define MAX 1001
int main(void)
{
	int arr1[MAX]={0},arr2[MAX]={0};
	int length,i,length1,length2,t,dv = 0;
	char str[MAX];
	/*读入数据，并进行预处理(计算出数字位数，并方向存放)*/
	scanf(&quot;%s&quot;,str);
	length1 = strlen(str);
	for(i=0; i&lt;length1; ++i)
		arr1[i] = str[length1-1-i] - '0';
	scanf(&quot;%s&quot;,str);
	length2 = strlen(str);
	for(i=0; i&lt;length2; ++i)
		arr2[i] = str[length2-1-i] - '0';
	length = (length1&gt;length2)?length1:length2;
	/*算法核心内容*/
	for(i=0; i&lt;length; ++i)
	{
		t = arr1[i] + arr2[i] +dv;
		arr1[i] = t % 10;
		dv = t / 10;
	}
	if(dv != 0)	arr1[length++] = dv;
	/*结束*/
	for(i=0; i&lt;length; ++i)
		printf(&quot;%d&quot;,arr1[length-1-i]);
	return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]