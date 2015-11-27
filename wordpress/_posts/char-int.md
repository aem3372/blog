title: 对字符型与整型的探究
tags:
  - c
  - getchar
  - scanf
id: 322
categories:
  - 经验之谈
date: 2012-12-29 19:50:31
---

据目前了解，int与char都是按照补码储存的

一般字符按照ASCII以整型数形式存储

Scanf()使用%c参数与getchar() 都能从stdin流中读入一个字符

并且EOF因为其值可能超出char类型的存储范围，故常常使用int型存储字符

而我无意间发现用int储存字符型一个奇怪的现象

&nbsp;

于是，我使用一段程序对这个问题进行研究

程序代码如下：
[code lang="cpp"]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
int main(void)
{
	//命名方式 读入方式_声明类型
	int scanf_int,get_int,c_int='s';
	char scanf_char,get_char,c_char='s';
	int temp;
	printf(&quot;Please enter:\n&quot;);
	scanf(&quot;%c%c&quot;,&amp;scanf_int,&amp;scanf_char);
	while((temp=getchar())!='\n');//清空stdin流
	get_int=getchar();
	get_char=getchar();
	//对于每个数据用printf格式输出
	printf(&quot;Result:\n&quot;);
	printf(&quot;scanf_int: %c(%12d)\n&quot;,scanf_int,scanf_int);
	printf(&quot;get_int:   %c(%12d)\n&quot;,get_int,get_int);
	printf(&quot;c_int:     %c(%12d)\n&quot;,c_int,c_int);
	printf(&quot;scanf_char:%c(%12d)\n&quot;,scanf_char,scanf_char);
	printf(&quot;get_char:  %c(%12d)\n&quot;,get_char,get_char);
	printf(&quot;c_char:    %c(%12d)\n&quot;,c_char,c_char);
	system(&quot;pause&quot;);
	return 0;
}
[/code]
研究过程中，我将以上代码，在VC++6.0和MinGW32两个编译环境下分别以Debug和Release两个编译模式下运行

运行结果如下：

编译环境：VC++6.0
编译模式：DEBUG
输入输出结果：

[![1](http://www.aemiot.com/wp-content/uploads/2012/12/1.gif)](http://www.aemiot.com/wp-content/uploads/2012/12/1.gif)

编译环境：VC++6.0
编译模式：Release
输入输出结果：

[![2](http://www.aemiot.com/wp-content/uploads/2012/12/2.gif)](http://www.aemiot.com/wp-content/uploads/2012/12/2.gif)

编译环境：MinGW32
编译模式：Debug
输入输出结果：

[![3](http://www.aemiot.com/wp-content/uploads/2012/12/3.gif)](http://www.aemiot.com/wp-content/uploads/2012/12/3.gif)

编译环境：MinGW32
编译模式：Release
输入输出结果如下：

[![4](http://www.aemiot.com/wp-content/uploads/2012/12/4.gif)](http://www.aemiot.com/wp-content/uploads/2012/12/4.gif)

对于scanf使用%c格式读入的int型字符，能够正常打印出字符，其输出的整数却不是其字符对应的ASCII值，而产生了一共3个异常值，这三个值分别为
-858993549
4231283
2686835

计算出这三个数的补码(32位)分别为
1100 1100 1100 1100 1100 1100 0111 0011
0000 0000 0100 0000 1001 0000 0111 0011
0000 0000 0010 1000 1111 1111 0111 0011

发现最低的8位相同，并且 0111 0011正好为s的ASCII值(115)
推测，使用scanf函数仅保证最后8位相同

那么推测使用int型存储字符型数据，与一个字符常量比较时，字符常量自动转换为__int32类型
其结果应该发生异常，我采用如下程序进一步研究
[code lang="cpp"]
#include &lt;stdio.h&gt;
int main(void)
{
	int c;
	scanf(&quot;%c&quot;,&amp;c);
	printf(&quot;%d&quot;,c=='s');
	return 0;
}
[/code]
输入数据：s
输出数据：0

为了进一步确认，对上面的程序进行修改，这次引入强制转换符（根据ANSI C数据转换，只保留低位）
[code lang="cpp"]
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
int main(void)
{
	int c;
	scanf(&quot;%c&quot;,&amp;c);
	printf(&quot;%d&quot;,(char)c=='s');
	system(&quot;pause&quot;);
	return 0;
}

[/code]
输入数据：s
输出数据：1

基本可以确认之前的结论

而另一个函数getchar是将int型的返回值赋值给变量（getchar本身得到的结果就是int型），故其不会出现异常。
最后建议大家，如果只是纯粹读取字符，尽量使用getchar()。

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]