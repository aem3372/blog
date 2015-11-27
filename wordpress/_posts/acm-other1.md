title: '话说，我是菜鸟|江西师范大学ACM协会新生选拔赛题解'
tags:
  - ACM
  - 吃糖果
  - 江西师大
id: 766
categories:
  - 算法/数据结构
date: 2013-04-05 01:02:57
---

话说自从被腾讯马拉松虐了之后，心态一直很消极。

准备找点水题来找自信，于是我就去看了下江西师范大学ACM协会新生选拔赛（赛后重挂）打个酱油，事实证明心态不正，结果一定很悲惨！！！

这是结果：

[![acmother](http://www.aemiot.com/wp-content/uploads/2013/04/acmother.jpg)](http://www.aemiot.com/wp-content/uploads/2013/04/acmother.jpg)

虽然是水题，最后全部A掉了，不过= =，错这么多次才A,特别是第一二题都错，让我怎么好意思说我是曾经的OI选手啊！

[toggle title="第一题题解"]

# Problem Description

Ｗｅｌｃｏｍｅ　Ｅｖｅｒｙｏｎｅ~~
期待了很久很久滴华丽丽滴江西师范大学ＡＣＭ协会华丽丽滴在２０１３年０２月２２日成立了~一个够二的日子一个不二的组织~
撒花撒花~~
身为协会滴宣传，我灰常灰常感谢各位能来参加偶们滴招新比赛，。。
好吧，说实话今天我滴主要任务就是给大家打气加油～我也就是颗大一小白菜，轻拍板砖，会砸烂的。。＝＿＝｜
比赛神马滴真心不难的，绝对滴够水，，泳池水深1.5M。。高台跳水注意安全~
好吧，比老师滴实验报告总要难点咯，要不然肿么能体现出大家滴实力捏～那些ｓｃａｎｆ半径算面积神马滴，。完全让人木有运行滴冲动啊，有木有啊啊！！！
申明一下，千万不要利用这个机会体现自己和某人那个一往情深啊ｏｒ那个手足义气啊，。我劝各位哦，这个是要记院级处分滴！！！！！
那么，，神马算作弊捏？？
Ｆｉｒｓｔ　ｏｆ　ａｌｌ，ＱＱ神马传代码，某些Ｃ大神。。你们辛苦了啊，一个人两份代码，吼吼~
Ｔｈｅｎ，度娘再万岁，现在天要下雨度娘要嫁人了，，靠自己咯孩纸~
Ｌａｓｔ，，额，。。。
哎哎～。。写这种东西，孩纸八辈纸滴节操都木有了。。所以要借这个机会好好滴爆料下偶滴师兄们，呜呜。。（捂住嘴然后拖走中。。。）

请注意请注意！！！！！！

如果你没有时间没有准备没有毅力每周上课、刷题、比赛~请跳过此题，ACM需要各位付出大量的精力时间，并且不是你付出了，最后就一定会有所回报。。

我们大家未来的路都还很长，所以，我们仅欢迎那些愿意一直一直互相扶持一同前进的战友~WE ARE A TEAM 

# Input

请输入今天的日期（2013330）。 

# Output

然后，就请输入”I volunteer to join the JXNU_ACM,OVER!“（不包括引号）。（有人说和输入数据无关呢~但这样不会出现RE了O(∩_∩)O）（TIPS：别忘了换行哦） 

# Sample Input

2013330

# Sample Output

I volunteer to join the JXNU_ACM,OVER!

# Author

吴烨琪 

# Source

ACM协会 

# 解题思路

这题真的没啥好说的，我相信也没人需要这题的题解，不过，我还是贴出我的代码吧= =！不过我要很无耻的说，我第一题题目没看完就写了么，特别注意要换行符，可我还是没换行。哎！~

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
using namespace std;
int main(void)
{
    int x;
    cin &gt;&gt; x;
    cout &lt;&lt; &quot;I volunteer to join the JXNU_ACM,OVER!&quot; &lt;&lt; endl;
    return 0;
}
[/code]

[/toggle]

[toggle title="第二题题解"]

# Problem Description

小KD好头疼出题的事。。。因为题目出难了没人写出来的话，比赛就白办了。。。所以便有了以下这道题
输入两个自然数a,b(0<=b<a<=2^63)，计算a与b的和并输出。
(提示：long long使用I64d输入输出，unsigned long long使用I64u输入输出。) 

# Input

RT 

# Output

RT 

# Sample Input

2 1

# Sample Output

3

# Author

李思辰 

# Source

ACM协会 

# 解题思路

加法，也没啥好说的，注意下，别像我一样习惯的打成了%I64d，这里要%I64u。还有就是，别以为一行结果就不要换行了，这里我又没换行，再一次PE。（话说OI赛忽略文件末尾空白符多和谐。）

# 我的代码

[code lang="cpp"]
#include &lt;stdio.h&gt;
unsigned long long a,b;
int main(void)
{
	scanf(&quot;%I64u&quot;,&amp;a,&amp;b);
	printf(&quot;%I64u&quot;,a+b);
}
[/code]
[/toggle]

[toggle title="第三题题解"]

# Problem Description

在C语言课上，大家学习了3位整数倒序输出如 123->321.要是n位整数，你会做吗？
小KD为了让更多人做出这道题，给出了温（xie）馨（e）提示：用字符串有奇效。（学长我错了。。。我不该透题解） 

# Input

m组测试数据(小于100)，每组一行。每行两个数：第一个数n，第二个数是一个n位整数a（n不超出32位整形）。 

# Output

倒序输出n位整数a。每两个数用回车分开。 

# Sample Input

3 123
3 234
3 333

# Sample Output

321
432
333

# Author

李思辰 

# Source

ACM协会 

# 解题思路

倒序么，有好几种思路呢。
首先，可以利用除10和对10取模完成倒序
其次，你可以用字符串读入，然后反向遍历字符串（当然你整型读入也没关系，使用sprintf函数或者ostringstring流对象即可）；
当然，还有一些其他方法，这里就不多说。
在这里，我选择了第二种方法，利用C++ STL，可以快速写完。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;string&gt;
using namespace std;
int main(void)
{
    int n;
    string str;
    while(cin &gt;&gt; n)
    {
        cin &gt;&gt; str;
        if(str.at(0)=='-')  { cout &lt;&lt; '-';  str = str.substr(1,str.size()-1); }
        cout &lt;&lt; string(str.rbegin(),str.rend()) &lt;&lt; endl;
    }
    return 0;
}
[/code]
[/toggle]

[toggle title="第四题题解"]

# Problem Description

江西师范大学的食堂每到中午十二点都是十分拥挤的。
设有N个学生打饭，并且每个学生都需要打K种饭/菜。食堂有M名员工，并且每一位员工都面前都有这K种饭/菜(也就是说每个员工都能打K种饭/菜中的任意一种)。训练有素的食堂员工会用一秒钟将饭菜打好。不计其他时间损耗。
小KD为了避开打饭高峰期。要你写一个程序计算学生打完饭需要多长时间。 

# Input

输入的第一行为一个正整数T，表示有T组测试数据；
接下去有T组测试数据，每组测试数据占一行，包含三个整数N，K，M，N表示学生的人数，K表示每个学生要打的饭菜数，M表示食堂员工的人数。
T<=1000
1<=N<=100
1<=K<=10
1<=M<=100 

# Output

对于每组数据，输出一个整数，表示最快需要多少秒才能使所有同学打完饭。 

# Sample Input

2
2 1 1
3 2 2

# Sample Output

2
3

# Author

李思辰 

# Source

ACM协会 

# 解题思路<h2>
首先吐槽下这题目描述，看完之后，很多细节方面我还真没看明白= =！不过等我看了N遍之后，我突然发现，这不就是我几天前A过的题么，可惜在这期间，我冲动的提交了一次，导致了一次WA。（几天前还是一次A过的，这次居然不能一次过= =！）废话说完了，顺便贴出此题原型吧--<a href="http://acm.hdu.edu.cn/showproblem.php?pid=4519">腾讯马拉松网络赛初赛第四场——郑厂长系列故事——体检（现HDU-Problem-4519）</a>。回归题解了，这题唯一需要注意的就是，当食堂员工数M大于学生数N时，一个时刻也只能有N个员工给N个学生打饭/菜。即当M>N时，与M=N是一样的结果。至于其中数学关系，很容易发现：(n*k)/m向上取整，在C语言中可表述为(n*k-1)/m+1
<h1>我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
int main(void)
{
	int t,n,k,m;
	scanf(&quot;%d&quot;,&amp;t);
	while(t--)
	{
		scanf(&quot;%d%d%d&quot;,&amp;n,&amp;k,&amp;m);
		if(m&gt;n) m = n;
		printf(&quot;%d\n&quot;,(n*k-1)/m+1);
	}
	return 0;
}
[/code]
[/toggle]

[toggle title="第五题题解"]

# Problem Description

在计算机导论课上，我们学到了一种字符串压缩方法：字符串中有多个相同字符连在一起，就以字符加出现次数形式表示。如 abcddddddddef 就可以表示为abcd8ef。现小KD制作了些字符串，让你将其压缩。

# Input

多组输入数据，每组一行，字符长度小于20。 

# Output

压缩后的字符串。 

# Sample Input

abcddddddddef
qwertyuiop
aassddffgghhjjkkll

# Sample Output

abcd8ef
qwertyuiop
a2s2d2f2g2h2j2k2l2

# Author

李思辰 

# Source

ACM协会 

# 解题思路

这题其实和我之前写的某道题（[杭电-1020题解](http://www.aemiot.com/encode.html "最简单的压缩方式|杭电Problem-1020")）差不多，注意下这题和杭电-1020不同的是，去除了字符是‘A’-‘Z’这个限制，也就是要能正确处理空白字符的情况，关于这点，只需要将按字符串读入，换为按行读入即可。（话说这题数据比较坑，它的测试数据中没有连续的空格，导致在按字符串读入时，系统给出的提示不是WA而是PE，就是这个导致我提交了N次都不知道为什么会是PE，一直在检查输出。）

# 我的代码

[code lang="cpp"]
#include &lt;stdio.h&gt;
#include &lt;iostream&gt;
#include &lt;ctype.h&gt;
#include &lt;string&gt;
#include &lt;cstring&gt;
#define MAX 10000
using namespace std;
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
    string st;
    while(cin &gt;&gt; st)
    {
        char *p = str;
        e = 0;
        strcpy(arr,st.c_str());
        while(arr[e])
        {
        x = ReX(arr+e);
        if(x==1) p += sprintf(p,&quot;%c&quot;,arr[e]);
        else p += sprintf(p,&quot;%c%d&quot;,arr[e],x);
        e += x;
        }
        //p += sprintf(p,&quot;\n&quot;);
        p += sprintf(p,&quot;&#92;&#48;&quot;);
        printf(&quot;%s&quot;,str);
    }   
    return 0;
}
[/code]
[/toggle]

[toggle title = "第六题题解"]

# Problem Description

ACM协会新生赛即将结束。。。小KD头疼的划分数线工作开始了。现在已知有n名同学参加ACM协会新生赛，并且知道每个同学的分数，小KD想知道其中第k名的分数是多少。 

# Input

第一行读入n,k，含义如题所述（n<10000,k不超出int型范围）
接下来n行读入n个整数，第i个整数表示第i个同学的分数。 

# Output

一个整数，表示第k名同学的分数是多少。 

# Sample Input

5 3
600 600 350 420 380

# Sample Output

420

# Author

李思辰 

# Source

ACM协会 

# 解题思路

2种方案。第一种，读入数据，排序，输出结果。第二种，利用STL的multiset容器，因为multiset建立时就已经是有序状态（平衡引索二叉树，效率也挺高的哦），然后直接拿multiset的反向迭代器找到我们需要的结果。这里我选择了第二种。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;set&gt;
using namespace std;
int main(void)
{
	int n,k;
	int t;
	cin &gt;&gt; n &gt;&gt; k;
	multiset&lt;int&gt; s;
	while(n--)
	{
		cin &gt;&gt; t;
		s.insert(t);	
	}
	multiset&lt;int&gt;::reverse_iterator itor = s.rbegin();
	for(int i=0; i&lt;k-1; ++i)  ++itor;
	cout &lt;&lt; *itor &lt;&lt; endl;
	return 0;
}
[/code]
[/toggle]

[toggle title ="第七题题解"]

# Problem Description

How far can you make a stack of cards overhang a table? If you have one card, you can create a maximum overhang of half a card length. (We're assuming that the cards must be perpendicular to the table.) With two cards you can make the top card overhang the bottom one by half a card length, and the bottom one overhang the table by a third of a card length, for a total maximum overhang of 1/2 + 1/3 = 5/6 card lengths. In general you can make n cards overhang by 1/2 + 1/3 + 1/4 + ... + 1/(n + 1) card lengths, where the top card overhangs the second by 1/2, the second overhangs tha third by 1/3, the third overhangs the fourth by 1/4, etc., and the bottom card overhangs the table by 1/(n + 1). This is illustrated in the figure below.

 [![1056-1](http://www.aemiot.com/wp-content/uploads/2013/04/1056-1.gif)](http://www.aemiot.com/wp-content/uploads/2013/04/1056-1.gif)

The input consists of one or more test cases, followed by a line containing the number 0.00 that signals the end of the input. Each test case is a single line containing a positive floating-point number c whose value is at least 0.01 and at most 5.20; c will contain exactly three digits.

For each test case, output the minimum number of cards necessary to achieve an overhang of at least c card lengths. Use the exact output format shown in the examples.

# Sample Input

1.00
3.71
0.04
5.19
0.00

# Sample Output

3 card(s)
61 card(s)
1 card(s)
273 card(s)

# Source

Mid-Central USA 2001 

# 解题思路

直接模拟过程，循环加判断即可~！

# 我的代码

[code lang="cpp"]
#include&lt;iostream&gt;
using namespace std;
int main()
{
    int n;
    double sum;
    double length;
    while(cin&gt;&gt;length)
    {
        if(!length)break;
        sum=0,n=0;
        while(length&gt;sum)
            sum+=1.0/(++n+1);
        cout &lt;&lt; n &lt;&lt; &quot; card(s)&quot; &lt;&lt; endl; 
    }
    return 0; 
} 
[/code]
[/toggle]

[toggle title ="第八题题解"]

# Problem Description

Fred Mapper is considering purchasing some land in Louisiana to build his house on. In the process of investigating the land, he learned that the state of Louisiana is actually shrinking by 50 square miles each year, due to erosion caused by the Mississippi River. Since Fred is hoping to live in this house the rest of his life, he needs to know if his land is going to be lost to erosion. 

After doing more research, Fred has learned that the land that is being lost forms a semicircle. This semicircle is part of a circle centered at (0,0), with the line that bisects the circle being the X axis. Locations below the X axis are in the water. The semicircle has an area of 0 at the beginning of year 1\. (Semicircle illustrated in the Figure.) 

# Input

The first line of input will be a positive integer indicating how many data sets will be included (N). 

Each of the next N lines will contain the X and Y Cartesian coordinates of the land Fred is considering. These will be floating point numbers measured in miles. The Y coordinate will be non-negative. (0,0) will not be given.

# Output

For each data set, a single line of output should appear. This line should take the form of: 

“Property N: This property will begin eroding in year Z.” 

Where N is the data set (counting from 1), and Z is the first year (start from 1) this property will be within the semicircle AT THE END OF YEAR Z. Z must be an integer. 

After the last data set, this should print out “END OF OUTPUT.” 

Notes: 

1\. No property will appear exactly on the semicircle boundary: it will either be inside or outside. 

2\. This problem will be judged automatically. Your answer must match exactly, including the capitalization, punctuation, and white-space. This includes the periods at the ends of the lines. 

3\. All locations are given in miles.

# Sample Input

2 
1.0 1.0 
25.0 0.0

# Sample Output

Property 1: This property will begin eroding in year 1\. 
Property 2: This property will begin eroding in year 20\. 
END OF OUTPUT. 

# Source

Mid-Atlantic USA 2001 

# 解题思路

不难发现如下数学关系
x*x + y*y = r*r
PI*r*r / 2 = S
S/50向上取整即是我们的结果。
注意：这里PI精度要高一点（我取3.14会WA，取3.1415926就AC）

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;cstring&gt;
#include &lt;cmath&gt;
using namespace std;
const double PI = 3.1415926;
int main(void)
{
	int i,T,res;
	double x,y,val;
	cin &gt;&gt; T;
	for(i=1; i&lt;=T; ++i)
	{
		cin &gt;&gt; x &gt;&gt; y;
		val =PI*(x*x+y*y)/100;
		res = (val-(int)val)?(int)val+1:(int)val;
		cout &lt;&lt; &quot;Property &quot;&lt;&lt; i &lt;&lt;&quot;: This property will begin eroding in year &quot; &lt;&lt; res &lt;&lt; &quot;.&quot; &lt;&lt; endl;
	}
	cout &lt;&lt; &quot;END OF OUTPUT.&quot; &lt;&lt;endl;
	return 0;
}
[/code]
[/toggle]

[toggle title ="第九题题解"]

# Problem Description

HOHO，终于从Speakless手上赢走了所有的糖果，是Gardon吃糖果时有个特殊的癖好，就是不喜欢将一样的糖果放在一起吃，喜欢先吃一种，下一次吃另一种，这样；可是Gardon不知道是否存在一种吃糖果的顺序使得他能把所有糖果都吃完？请你写个程序帮忙计算一下。

# Input

第一行有一个整数T，接下来T组数据，每组数据占2行，第一行是一个整数N（0<N<=1000000)，第二行是N个数，表示N种糖果的数目Mi(0<Mi<=1000000)。

# Output

对于每组数据，输出一行，包含一个"Yes"或者"No"。

# Sample Input

2
3
4 1 1
5
5 4 3 2 1

# Sample Output

No
Yes

# Hint

Hint
Please use function scanf 

# Author

Gardon 

# Source

Gardon-DYGG Contest 2 

# 解题思路

话说，一开始看到这题时间给了3000ms，我还想模拟呢。后来仔细一想，发现这题其实完全没必要模拟整个过程，因为这是一个组合数学的问题呢，吃糖果的顺序构成一个序列，这个序列是由我们来排的，不连续吃同一种糖其实就是序列中没有两个相邻的是同一种糖。那么可以设想将最多的一种糖（假设有max个）果先全部加入序列，然后将余下的糖果插入其中，如果将问题建立在这样一个数学模型上，那么显然只有余下的糖少于max-1个时才可能会出现不符合题意的序列。因此，我们只要找出最大值max，如果max<=sum-max+1就存在，否则不存在。

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
using namespace std;
int main()
{
     int T,n,m,a,max;
     int i,j;
     long long sum;
     cin &gt;&gt; T;
     while(T--)
     {
         sum=0; max=0;
         cin &gt;&gt; n;
         for (j=0;j&lt;n;j++)
         {
             cin &gt;&gt; a;
             if (a&gt;max) max=a;
             sum+=a;
         }
         if (sum-max+1&lt;max) 
	cout &lt;&lt; &quot;No&quot; &lt;&lt; endl;
         else 
	cout &lt;&lt; &quot;Yes&quot; &lt;&lt; endl;
     }
    return 0;
}
[/code]
[/toggle]

[toggle title ="第十题题解"]

# Problem Description

A simple mathematical formula for e is

[![1012-1](http://www.aemiot.com/wp-content/uploads/2013/04/1012-1.gif)](http://www.aemiot.com/wp-content/uploads/2013/04/1012-1.gif)

where n is allowed to go to infinity. This can actually yield very accurate approximations of e using relatively small values of n.

# Output

Output the approximations of e generated by the above formula for the values of n from 0 to 9\. The beginning of your output should appear similar to that shown below.

# Sample Output

n e
- -----------
0 1
1 2
2 2.5
3 2.666666667
4 2.708333333

# Source

Greater New York 2000 

# 解题思路

纯数学，不多说。唯一值得提下的就是输出格式不能使printf的g控制符，因为g控制符然后设定精度，那么在精度的最后一位如果是0会被忽略。例如printf("%.10g",3.123456780) 那么输出的会是 3.12345678，显然不符合我们题目的要求。

# 我的代码

[code lang="cpp"]
#include &lt;stdio.h&gt;
#include &lt;ctype.h&gt;
#define MAX 10000

const int JC[10]={1,1,2,6,24,120,720,5040,40320,362880};

double ReX(int n)
{
    int i;
    double x=0;
    for(i=0; i&lt;=n; ++i)
        x += 1.0/JC[i];
    return x;
}

int main(void)
{
    int i;
    printf(&quot;n e\n&quot;);
    printf(&quot;- -----------\n&quot;);
    for(i=0; i&lt;=2; ++i)
        printf(&quot;%d %.10g\n&quot;,i,ReX(i));
    for(i=3; i&lt;=9; ++i)
        printf(&quot;%d %.9f\n&quot;,i,ReX(i));
    return 0;
}
[/code]
[/toggle]

[toggle title ="第十一题题解"]

# Problem Description

The digital root of a positive integer is found by summing the digits of the integer. If the resulting value is a single digit then that digit is the digital root. If the resulting value contains two or more digits, those digits are summed and the process is repeated. This is continued as long as necessary to obtain a single digit.

For example, consider the positive integer 24\. Adding the 2 and the 4 yields a value of 6\. Since 6 is a single digit, 6 is the digital root of 24\. Now consider the positive integer 39\. Adding the 3 and the 9 yields 12\. Since 12 is not a single digit, the process must be repeated. Adding the 1 and the 2 yeilds 3, a single digit and also the digital root of 39.

# Input

The input file will contain a list of positive integers, one per line. The end of the input will be indicated by an integer value of zero.

# Output

For each integer in the input, output its digital root on a separate line of the output.

# Sample Input

24
39
0

# Sample Output

6
3

# Source

Greater New York 2000

# 解题思路

这题，我原来单独发过题解的，这里就不重复了，附上一个链接。
[**查看此题题解**](http://www.aemiot.com/problem-1013.html "使用数论知识消除模拟|杭电Problem-1013")
[/toggle]