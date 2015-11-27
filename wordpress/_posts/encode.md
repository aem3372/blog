title: '最简单的压缩方式|HDU-Problem-1020'
tags:
  - 压缩
  - 解压缩
id: 668
categories:
  - 算法/数据结构
date: 2013-02-24 13:02:04
---

题目：杭电Problem-1020 [Encoding](http://acm.hdu.edu.cn/showproblem.php?pid=1020)

题目简述：最简单的压缩方式。将多个字符换成个数+字符的方式并输出。

思路：一次获取一个字符的连续且相同，多次处理即可。

此题非常简单，因此就直接上代码了。

[code lang="cpp"]
#include &lt;stdio.h&gt;
#include &lt;ctype.h&gt;
#define MAX 10000

/*返回字符串第一个字符连续且相同的字符个数(包括第一个字符)*/
int ReX(char arr[])
{
    int n=0;
    char befo = arr[n++],t = befo;
    while(t==befo)
        t=arr[n++];
    return n-1;
}

int main(void)
{
    char arr[MAX],str[MAX];
    int i,n,e,x;
    char *p = str;
    scanf(&quot;%d&quot;,&amp;n);
    for(i=0; i&lt;n; ++i)
    {
        e = 0;
        scanf(&quot;%s&quot;,arr);
        while(arr[e])
        {
        x = ReX(arr+e);
        if(x==1) p += sprintf(p,&quot;%c&quot;,arr[e]);
        else p += sprintf(p,&quot;%d%c&quot;,x,arr[e]);
        e += x;
        }
        p += sprintf(p,&quot;\n&quot;);
    }
    p += sprintf(p,&quot;&#92;&#48;&quot;);
    printf(&quot;%s&quot;,str);
    return 0;
}

/*既然写了压缩，干脆解压缩一起写了*/
/*用于逆处理的函数
char* ScanOne(char arr[])
{
    char t = getchar();
    int i,n=1; //默认为一个字符
    if( isdigit(t) )
    {
        n = t - '0';
        t = getchar();
    }
    for(i=0; i&lt;n; ++i)
    {
        arr[i]=t;
    }
    return arr+n;
}
*/

/*逆处理过程
    char *p = str;
    scanf(&quot;%d&quot;,&amp;n);
    for(i=0; i&lt;n; ++i)
    {
        scanf(&quot;%s&quot;,arr);
        p = ScanOne(p);
    }
    printf(&quot;%s&quot;,str);
*/
[/code]
[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]