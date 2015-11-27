title: 'Max Sum--DP解法|HDU-Problem-1003'
tags:
  - ACM
  - DP
  - 动态规划
  - 算法
id: 107
categories:
  - 算法/数据结构
date: 2012-12-24 13:59:39
---

不多说，先上题

题目地址 [http://acm.hdu.edu.cn/showproblem.php?pid=1003](http://acm.hdu.edu.cn/showproblem.php?pid=1003)

看完题目，第一感觉就是介于贪心和动态规划之间的题目，不过我果断选择了动态规划

不难发现 ，动态规划方程为 res[m+1] = max{ res[m]+arr[m+1],arr[m+1] }

方程可以解释为：以下标m+1为终点的所有区间，和最大的要么是以下标m为终点的所有区间的最大值加上下标为m+1的值，要么就是下标为m+1的值

利用这个方程很快就能找到最大值

再按照题目要求做适当修改，在适当的位置保存head和last

这样就能写出粗糙的代码，接下来贴上我写的代码，如下

&nbsp;
[code lang="cpp"]
#include&lt;stdio.h&gt;
#define MAX 111111
int main(void)
{
   int n,i;
   scanf(&quot;%d&quot;,&amp;n);
   for(i=0; i&lt;n; ++i)
   {
      int length,j,thead,head,last,max;
      int arr[MAX],res[MAX];
      scanf(&quot;%d&quot;,&amp;length);
      for(j=0; j&lt;length; ++j)
         scanf(&quot;%d&quot;,&amp;arr[j]);
      /* 动态规划开始
         动归方程  res[m+1] = max{res[m]+k,k}
         尾在找到找到当前最大值时确定
         头要随时保存*/
      max = res[0] = arr[0];
      head = thead = last =1;
      for(j=0; j&lt;length-1; ++j)
      {    
         if((res[j]+arr[j+1])&gt;=arr[j+1])
         {
            res[j+1] = res[j] + arr[j+1];
            if(res[j+1] &gt; max)
               max = res[j+1] , head = thead , last = j+1+1;
         }
         else
         {
            res[j+1] = arr[j+1];
            thead = j+1+1;
            if(res[j+1] &gt; max)
               max = res[j+1] , head = thead , last = j+1+1;
         }
      }
      printf(&quot;Case %d:\n&quot;,i+1);
      printf(&quot;%d %d %d\n&quot;,max,head,last);
      if(i != n-1) printf(&quot;\n&quot;);
   }
   return 0;
}
[/code]

使用测试数据验证后，能得到正确结果，接下来就着手优化了

优化一： 观察整个程序，发现一直在使用res[j] 和 res[j+1] 因此取消res数组，用 resnow 和 resnext 取代 每次循环后 resnow = resnext 优化后，空间从 1000K 降为652K，时间从51s 降为46ms

优化二： 将一次性读入数据并储存到数组中，改为读入一个处理一个，从而取消数组 优化后，空间从652K 降为224K，时间从46ms 降为 31ms

优化三: 将编译器从GCC换为VC，并开启默认优化，空间从 224K变为220K，时间从31ms 变为15ms

接下来附上优化后的代码,如下

&nbsp;
[code lang="cpp"]
#include&lt;stdio.h&gt;
#define MAX 111111
int main(void)
{
   int n,i;
   scanf(&quot;%d&quot;,&amp;n);
   for(i=0; i&lt;n; ++i)
   {
      int length,j,thead,head,last,max;
      int arr,resnext,resnow;
      scanf(&quot;%d&quot;,&amp;length);
      scanf(&quot;%d&quot;,&amp;arr);
      /* 动态规划开始
         动归方程  res[m+1] = max{res[m]+k,k}
         尾在找到找到当前最大值时确定
         头要随时保存*/
      max =  resnow = arr;
      head = thead = last =1;
      for(j=0; j&lt;length-1; ++j)
      {    
         scanf(&quot;%d&quot;,&amp;arr);
         if((resnow+arr)&gt;=arr)
         {
            resnext = resnow + arr;
            if(resnext &gt; max)
               max = resnext , head = thead , last = j+1+1;
         }
         else
         {
            resnext = arr;
            thead = j+1+1;
            if(resnext &gt; max)
               max = resnext , head = thead , last = j+1+1;
         }
         resnow =resnext;
      }
      printf(&quot;Case %d:\n&quot;,i+1);
      printf(&quot;%d %d %d\n&quot;,max,head,last);
      if(i != n-1) printf(&quot;\n&quot;);
   }
   return 0;
}
[/code]
&nbsp;

最后，大家有兴趣的话，也可以试试

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]