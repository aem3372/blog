title: 传统高精度算法系列(三)整数的高精度阶乘
tags:
  - 阶乘
  - 高精度
id: 762
categories:
  - 算法/数据结构
date: 2013-04-01 13:21:16
---

有了之前[高精度加法](http://www.aemiot.com/calc-high-int-sum.html)和[高精度减法](http://www.aemiot.com/calc-high-int-inc.html)的介绍，这里的高精度阶乘就不多说了，原理类似。

计算高精度阶乘实质是多次高精度乘单精度的乘法，同样按照竖向式运算模拟。

很容易写出如下的代码：

[code lang="c"]
#include &lt;stdio.h&gt;
#define MAX 5000
int arr[MAX] = {0};
int main(void)
{
    int n,i,j,k;
    scanf(&quot;%d&quot;,&amp;n);
    arr[0]=1;
    k=1; /*当前长度为1*/
    for(i=2; i&lt;=n; ++i)
    {
        /*对于有效部分乘上k*/
        int c = 0;
        for(j=0; j&lt;k; ++j)
        {
            int s = arr[j]*i + c;
            arr[j] = s % 10;
            c = s / 10;
        }
        /*更改有效长度*/
        while(c)
        {
            arr[k++] = c %10;
            c /= 10;
        }
    }
    for(i=k-1; i&gt;=0; --i)
        printf(&quot;%d&quot;,arr[i]);
    return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]