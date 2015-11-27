title: 2013江西理工大学软件学院ACM选拔赛题解
tags:
  - ACM
id: 1151
categories:
  - 算法/数据结构
date: 2013-11-24 18:10:22
---

比赛情况：[http://acm.hdu.edu.cn/diy/contest_ranklist.php?cid=21699&page=1](http://acm.hdu.edu.cn/diy/contest_ranklist.php?cid=21699&page=1)

赛后重挂：[http://acm.hdu.edu.cn/diy/contest_show.php?cid=21697](http://acm.hdu.edu.cn/diy/contest_show.php?cid=21697)

[toggle title="第一题题解"]

# Problem Description

Aem最近在阅读一本书《浪潮之巅》，这本书基本上就是IT史，作者用独特的方式，分析了IT届近来大公司（IBM、苹果、微软、甲骨文等）的兴起与衰亡。大多数公司都是随着新的事物出现而迅速发展的，如同互联网的出现，致使大家对长途电话的需求急剧下降，一个刚兴起的互联网小公司甚至就能够击垮老牌的电话大公司。
Aem在阅读了这本书之后，开始思考起自己的人生来，而且最近也有不少新生问我一些我常常思考的问题。这也从侧面反映出新生还是很纠结的，一方面是对自己未来的不确定，对自己要学什么不确定。
所以，我决定运用我最近生活中的感悟以及IT的历史来讲解一下这些问题，接下来是我的见解：
人的一生，重在过程。在我看来，人的一生过的坦坦荡荡，追求自己的理想，充分发挥自己的天赋，挑战困难，品悟世间最宝贵的感情就是最好的结局了。 
大家都是知道的，IT行业是一个发展快速的行业，而现在IT行业促使着世界的快速变革。
在这种大环境下，你学习编程而只是学习一些语法什么的，然后不断复制别人的产品，这将致使你始终是一个二流程序员，而谈不上工程师。就像互联网小公司击垮老牌电话大公司一样。
为此，你所需要的能力应该是，能够为你带来创新技术优势编程灵魂-算法（再一次确定了自己一直以来选择的路是正确的），创新所必须的敏锐眼光（虽然我很不看好乔布斯，但是乔布斯早在第一次见到CD音乐盘时，就想到了音乐播放器将会有很大的市场，事实上iPod在苹果公司最危难的时候救了苹果一命）。在这点上，我希望ACM集训队能够帮助大家，在这里我们主要学习算法，我们也会做一些小产品（将算法变成产品）。并且现在各大公司普遍看好ACM，所以别再犹豫了，时代在快速变革，只有自己不断努力，不要再为自己找借口才能成功。 
好了，废话这么多，其实大家都不耐烦了吧。
正题，为了感谢《浪潮之巅》带给我的知识，我决定号召大家打印这本书作者的名字的拼音字母—Wu Jun

# Input

没有输入 

# Output

Wu Jun（提示：后面还有一个换行符） 
注意：最后还有一个换行符

# Sample Input

# Sample Output

Wu Jun（提示：后面还有一个换行符）

# Author

chuyuaem

# 解题思路

这题真的没啥好说的，直接上标程。

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;
int main()
{
    printf(&quot;Wu Jun\n&quot;);
    return 0;
}
[/code]

[/toggle]

[toggle title="第二题题解"]

# Problem Description

你的任务非常简单，对于一系列数，找出每个数中最小的数字。

# Input

第一行是整数T，表示有T个数。
接下来T行，每行一个数K。（0 <= K <= 2^64 - 1）

# Output

找出每个数中最小的数字。
每个数字一行。（第一个数对应的最小数字输出在第一行，以此类推）

# Sample Input

2
456
78

# Sample Output

4
7

# Author

chuyuaem

# 解题思路

使用64位无符号整型，或者当字符处理。

# 我的代码--64位无符号整型版

[code lang="c"]
#include &lt;stdio.h&gt;
int main()
{
	unsigned __int64 s;
	int t;
	scanf(&quot;%d&quot;,&amp;t);
	while(t--)
	{
		scanf(&quot;%I64u&quot;, &amp;s);
		int min = s%10;
		while(s)
		{
			if(s%10 &lt; min)
				min = s%10;
			s/=10;
		}
		printf(&quot;%d\n&quot;, min);
	}
	return 0;
}
[/code]

# 我的代码--字符处理版本

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;string&gt;
using namespace std;
int main()
{
    string s;
    int t;
    cin &gt;&gt; t;
    while(t--)
    {
         cin &gt;&gt; s;
         char c = s[0];
         for(int i=1; i&lt;s.size(); ++i)
         {
             if(s[i] &lt; c)
                 c = s[i];
         }
         cout &lt;&lt; c &lt;&lt; endl;
    }
    return 0;
}
[/code]

[/toggle]

[toggle title="第三题题解"]

# Problem Description

我们设计了一个新的手机,你的任务是写一个界面来显示电池的状态。
在这里,我们使用”.”空的部分，“-”表示非空部分

每一行有14个字符。
电池的状态用x%，表示剩余电量为x;
这里x将永远是10的倍数,而且从不
超过100。 

# Input

第一行有一个数T(T < 10),表示有T组数据。
接下来T行，每一行都有一个数x。(0 < x < 100,x是10的倍数) 

# Output

对于每组数据，画出它对应的电池图形。再每画完一个图形后，需要额外一个空行。 

# Sample Input

2
0
60

# Sample Output

<pre>
*------------*
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
|............|
*------------*

*------------*
|............|
|............|
|............|
|............|
|------------|
|------------|
|------------|
|------------|
|------------|
|------------|
*------------*
</pre>

# Author

chuyuaem

# 解题思路

找规律，或者直接暴力，反正输入非常有限

# 我的代码

[code lang="c"]
#include &lt;stdio.h&gt;  
int main()  
{  
	int T,t,k,i;
	scanf(&quot;%d&quot;,&amp;T);
	for(i=1; i&lt;=T; ++i)
	{
		scanf(&quot;%d&quot;,&amp;t);
		printf(&quot;*------------*\n&quot;);
		for(k=0; k&lt;10-t/10; ++k)
			printf(&quot;|............|\n&quot;);	
		for(k=0; k&lt;t/10; ++k)
			printf(&quot;|------------|\n&quot;);
		printf(&quot;*------------*\n&quot;);
		printf(&quot;\n&quot;);
	}
	return 0;  
} 
[/code]

[/toggle]

[toggle title="第四题题解"]

# Problem Description

一天，Aem背着包去买东西。
在商场，逛得眼花缭乱，最后他将以一些东西列入候选列表。
由于Aem认为散装的比包装的更加省钱，所以被列入候选列表的商品都是按重量计费的。
但是Aem的包是有限的，承重量为K，而被土豪附身了的Aem也正在为这个问题困惑。作为一名优秀的程序员，你当然是奋不顾身的冲上去帮助Aem咯。你的任务就是，在一系列候选商品中，告诉Aem每个商品要买多少（不买则相当于购买重量0，另外每个商品都有自己的最大购买重量，毕竟东西也是有限的嘛）才能使自己买的东西价值最高（真心！土豪啊！） 

# Input

第一行一个整数T，表示有T组数据。
每组数据的第一行，两个数字，包的承重量K和候选商品数量N。
接下来N行，每行2个数，分别是一个商品的单位重量价格和最大购买重量。
(为了便于处理，假定候选列表中没有2个商品的单位重量价格是相同的，并且你只能购买单位重量整数倍的商品) 

# Output

对每组数据在输出N个数字(每行一个)，分别为编号1...N商品的购买重量。

# Sample Input

1
10 5
2 4
7 5
3 3
4 5
1 2

# Sample Output

0
5
0
5
0

# Author

chuyuaem

# 解题思路

对于商品列表，优先拿单位重量价格大的，拿完一个商品，背包还没满，就继续拿第二大的，以此类推。（其实就是贪心）

# 我的代码

[code lang="cpp"]
#include &lt;iostream&gt;
#include &lt;cstring&gt;
using namespace std;
int cost[1000];
int wid[1000];
int use[1000];
int main()
{
	int T;
	cin &gt;&gt; T;
	while(T--)
	{
		memset(use,0,sizeof(use));
		int K,N;
		cin &gt;&gt; K &gt;&gt; N;
		for(int i=0; i&lt;N; ++i)
		{
			cin &gt;&gt; cost[i] &gt;&gt; wid[i];
		}
		int sum = 0; //重量计数
		int count = 0; //已拿物品数
		while(sum != K &amp;&amp; count != N)
		{
			int maxIndex = 0;
			for(int i=1; i&lt;N; ++i)
				if(cost[i]&gt;cost[maxIndex])
					maxIndex = i;
			++count; //拿一件
			use[maxIndex] = K-sum&gt;wid[maxIndex] ?wid[maxIndex]:K-sum;
			sum += use[maxIndex];
			cost[maxIndex] = 0;
		}
		for(int i=0; i&lt;N; ++i)
			cout &lt;&lt; use[i] &lt;&lt; endl;
	}
	return 0;
}
[/code]

[/toggle]

[toggle title="第五题题解"]

# Problem Description

在一个月黑风高的晚上，小白不小心又把小猫惹火了，小猫很生气，后果很严重，轻则伤筋断骨，重则孤苦终老。于是千百年来人们一直在思考一个问题-----MM生气了怎么办，当然，情圣大师一般是XXXXXXXX就搞定的，但是腼腆的小白一直都是有贼心没贼胆的孩子，于是他选择了最古老的方式-----发短信。对于短信来说，有时能让钢铁之躯化为绕指柔，能让东海枯竭，六月飞雪。还能让滚滚财宝汇入一家之手，腰缠万贯，佳丽三千（- =你说的是移动老总吧）。话说天下短信三分天下，文艺青年用土豪金手写，普通青年用智能机拼音，二逼青年用诺基亚按键。小白不幸还是用着高中时期麻麻买的诺基亚核桃机。所以按键如图---
　　 1　　　 2　　　　3
　　　　　　abc　　　def
　　 4　　　 5　　　　6
　　ghi　　　jkl　　　mno
　　 7　　　 8　　　　9
　　pqrs　　tuv　　　wxyz 
机智的小白想来想去还是觉得用高端内涵有深度的英吉利语言比较好突出自身的洋气。但是懒惰的小白又不想想怎么去输入这些话，所以请上档次的你帮他设计一个输入一行英文或者空格然后告诉他需要打哪些数字的程序。
空格输出1.
如输入:big
输出:224444

# Input

一行字符，保证没有小写字母和空格以外的字符。
长度<1000

# Output

输出转换手机键盘的数字。
提示：末尾需要输出一个换行符

# Sample Input

hello girl

# Sample Output

443355555566614444777555

# Author

万博文

# 解题思路

暴力判断，然后转换。

# 我的代码

[code lang="c"]
#include&lt;stdio.h&gt;
int main()
{
     char s[1000];
     int i,n,k;
     int t;
     i=0;n=0;
     while (scanf(&quot;%c&quot;,&amp;s[i])==1)
     {
         i=i+1;      
     }
     n=i;
     for (i=0;i&lt;n;i++)
     {
         if (s[i]==' ') {printf(&quot;1&quot;);continue;}
         if (s[i]=='a') {printf(&quot;2&quot;);continue;}
         if (s[i]=='b') {printf(&quot;22&quot;);continue;}
         if (s[i]=='c') {printf(&quot;222&quot;);continue;}
         if (s[i]=='d') {printf(&quot;3&quot;);continue;}
         if (s[i]=='e') {printf(&quot;33&quot;);continue;}
         if (s[i]=='f') {printf(&quot;333&quot;);continue;}
         if (s[i]=='g') {printf(&quot;4&quot;);continue;}
         if (s[i]=='h') {printf(&quot;44&quot;);continue;}
         if (s[i]=='i') {printf(&quot;444&quot;);continue;}
         if (s[i]=='j') {printf(&quot;5&quot;);continue;}
         if (s[i]=='k') {printf(&quot;55&quot;);continue;}
         if (s[i]=='l') {printf(&quot;555&quot;);continue;}
         if (s[i]=='m') {printf(&quot;6&quot;);continue;}
         if (s[i]=='n') {printf(&quot;66&quot;);continue;}
         if (s[i]=='o') {printf(&quot;666&quot;);continue;}
         if (s[i]=='p') {printf(&quot;7&quot;);continue;}
         if (s[i]=='q') {printf(&quot;77&quot;);continue;}
         if (s[i]=='r') {printf(&quot;777&quot;);continue;}
         if (s[i]=='s') {printf(&quot;7777&quot;);continue;}
         if (s[i]=='t') {printf(&quot;8&quot;);continue;}
         if (s[i]=='u') {printf(&quot;88&quot;);continue;}
         if (s[i]=='v') {printf(&quot;888&quot;);continue;}
         if (s[i]=='w') {printf(&quot;9&quot;);continue;}
         if (s[i]=='x') {printf(&quot;99&quot;);continue;}
         if (s[i]=='y') {printf(&quot;999&quot;);continue;}
         if (s[i]=='z') {printf(&quot;9999&quot;);continue;}
     }
        printf(&quot;\n&quot;);  
        return 0;   
}
[/code]

[/toggle]

[toggle title = "第六题题解"]

# Problem Description

果然是最传统的方法，发短信过去后，小猫略有回应，不愧是上得了厅堂，下得了厨房，打得过流氓，砸的开核桃的诺基亚。但是一切就这样完美的结束了吗？答案当然是否定的，很悲催，由于惹恼了小猫，所以现在小白要将功赎罪帮小猫帮一件事情，因为小猫在玩一个Valley of Dragon的游戏，就是里面乒乒乓乓的打架啦打怪啦，但是人和人都有强弱之分，所以小猫希望小白能做出一个计算软件，通过输入其他人的数据就求出别人最大能力。首先是输入护甲---T，然后是DPS---D，再是控制能力----C，接下来是移动速度V，在接下来是他所拥有的佣兽个数s。最后是输入性别N。

而要求的power=(t*d^3+c^2*v)*s
最后如果性别是B则power最终要输出原来的0.8，如果是A则不变。（不要问我为什么要性别，我已经和谐了）

# Input

第一行是一个整数N，表示接下来有N组数据。
每组数据由6个整数构成，分别是t,d,c,v,s,n （0<t,d,c,v,s<15）

# Output

power值。（输出到小数点后3位） 

# Sample Input

1
5 1 7 4 3 A

# Sample Output

603.000

# Author

万博文

# 解题思路

直接计算。注意精度，float是不够的。

# 我的代码

[code lang="cpp"]
#include&lt;iostream&gt;
#include&lt;cstdio&gt;
using namespace std;
int main()
{
    int T,t,d,c,v,s;
    char n;
    cin &gt;&gt; T;
    while(T--)
    {
        cin &gt;&gt; t&gt;&gt; d&gt;&gt; c&gt;&gt; v&gt;&gt; s&gt;&gt; n;
        if(n=='B')
            printf(&quot;%.3f\n&quot;,(double)(t*d*d*d+c*c*v)*s*0.8);
        else
            printf(&quot;%.3f\n&quot;,(double)(t*d*d*d+c*c*v)*s);
    }
    return 0;
}
[/code]

[/toggle]

[toggle title ="第七题题解"]

# Problem Description

功夫不负有心人，在小白左右讨好之后，小猫最终还是原谅了小白，于是小白迈向了最后一步---------去小猫家.....................帮小猫打通关游戏，是的，我相信大家一定没有想歪，学编程的都是最最纯洁的孩子。但是每次去小猫家都会遇到一个很令小白蛋疼的问题----爬楼梯，很普通的楼梯，但是层数会经常换，小白可以一次上一层也能一次两层，甚至一次三层，这得益于多年运动的结果，但是在这里却是个累赘。因为大门的密码是从一层上到最后一层有多少种方法~
比如有三层阶梯，则他上去的方法有4中。
 分别是 1 1 1 
 1 2 
 2 1 
 3 
并且这一次由于常年失修，在其中会出现坏掉的阶梯，坏的阶梯不能够踩踏。
于是小白又来请你帮他设计一个程序，让他轻易就得知方法的种数。

# Input

有多组数据。
每组数据的第一行输入N（n<=100）层楼梯，K(k<=10)个坏的楼梯
每组数据的第二行到k+1行输入坏掉的阶梯。
当N，K同时为0时，意味着没有数据了，并且这对0不要计算.

# Output

对于每组数据，输出方案总数ans mod 89223后的值。

# Sample Input

5 2
4 
2
7 2
4
6
0 0

# Sample Output

2
6

# Author

万博文

# 解题思路

递推，当遇到坏的电梯时，特殊处理下。

# 我的代码

[code lang="c"]
#include&lt;stdio.h&gt;
int main()
{
    int f[101],a[101];
    int i,n,k,m,ans;
    while(scanf(&quot;%d %d&quot;,&amp;n,&amp;k) != EOF)
    {
	if(n==0 &amp;&amp; k==0) 
		break;
    	for (i=1;i&lt;=n;i++)
    	{
    	    a[i]=0;
    	    f[i]=0;
    	}
    	for (i=0;i&lt;k;i++)
    	{
    	  scanf(&quot;%d&quot;,&amp;m);
    	  a[m]=1;
    	}

    	if (a[1]==0) f[1]=1;
    	if (a[2]==0) f[2]=f[1]+1;
    	if (a[3]==0) f[3]=f[1]+f[2]+1;

    	for (i=4;i&lt;=n;i++)
    	{
    	    if (a[i]==1) f[i]=0;
    	    else
    	    {
    	       f[i]=(f[i-3]+f[i-2]+f[i-1])%89223;
    	    }
    	}    
   	ans=f[n];
   	printf(&quot;%d\n&quot;,ans);
    }
    return 0;    
} 	
[/code]

[/toggle]

[toggle title ="第八题题解"]

# Problem Description

我以为我会是最坚强的那一个 我还是高估了自己
我以为你会是最无情的那一个 还是我贬低了自己
就算不能够在一起 我还是为你担心
就算你可能听不清 也代表我的心意
那北极星的眼泪 闪过你曾经的眼角迷离
那玫瑰花的葬礼 埋葬的却是关于你的回忆
如果时光可以倒流 我希望不要和你分离
如果注定分离 我希望不要和你相遇
——摘自《XX失恋日记》第XX卷520页

XX暗恋失败了。
XX是某公司的一名员工。
XX自从第一次遇到自己心仪的女神，这1000多个日夜他一直憧憬着他们美好的未来。为见到心中的女神，他每天都是早到晚归，夺得了不少荣誉。当他鼓足了勇气准备去表白的时候，心中的女神却满面春风地送来了一包喜糖......
现在XX满脑子就只有一件事，如果时间能倒流多好啊！（ps：其实出题人认为没缘分就是没缘分！）
假设现在已知当前的时间，让时间倒退回若干，你能计算出钟表显示的时间吗？

# Input

第一行是一个整数N，表示有N组数据。
接下来的N行，每行包括2个时间HH:MM:SS hh:mm:ss
HH:MM:SS为当前的时间
hh:mm:ss是希望倒退时间的多少。
[数据范围]
00<=HH<=11
00<=hh<=99
00<=MM, SS, mm, ss<=59

# Output

输出时光倒流后的时间
输出格式，每组数据输出占一行。
这一行打印一个如下形式的时间。
HH:MM:SS
每个显示2位，如果不足则补0。

# Sample Input

2
11:28:32 02:14:21
05:00:00 96:00:01

# Sample Output

09:14:11
04:59:59

# Author

chuyuaem

# 解题思路

这题，我原来单独发过题解的，这里就不重复了，附上一个链接。
[**查看此题题解**](http://www.aemiot.com/hdu-problem-4510.html "水题-时间计算|小Q系列故事——为什么时光不能倒流|HDU-4510")

[/toggle]

[toggle title ="第九题题解"]

# Problem Description

曾经有人问我，正式比赛会不会有跟热身赛一样的题。我当时说这是个坑。嗯，没错，坑一次。反正，你们都没有在比赛结束后好好思考过这道题吧~~啦啦啦~~啦啦啦~~

一本书的页面从自然数1开始顺序编码直到自然数n。书的页码按照n的位数编排，不足时补充前导数字0。例如，n是149时，第6页用数字006表示，而不是06或6。现在要求你对给定书的总页码n，计算出书的全部页码分别用到多少次数字0,1，2，...，9。

# Input

有若干组数据，每组数据一行。 （输入结束标志应该以EOF为准，当scanf函数的返回值为EOF时，表示已经没有数据可以读了）
每行一个数字n。（1≤n≤10^9）

# Output

每组输入数据对应10行。分别是0-9的使用次数。 
输出完一组数据后，额外输出一个换行。

# Sample Input

11
1

# Sample Output

10
4
1
1
1
1
1
1
1
1

0
1
0
0
0
0
0
0
0
0

# Author

chuyuaem

# 解题思路

这题，也是我原来发过题解的，这里就不重复了，附上一个链接。
[**点击查看题解（该比赛第七题）**](http://www.aemiot.com/jxustnc-2013-xss0.html "2013江西理工大学软件学院ACM新生热身赛题解")

[/toggle]

[toggle title ="第十题题解"]

# Problem Description

这是最后一题，是不是觉得很恐怖，是不是觉得很难！
如果你这么认为，那你就输了。
当然，这题也不是那么的简单。

两个数的最大公约数与最小公倍数的关系如下：两数之积除以最大公约数就可得到最小公倍数。

现在，要求你求出一系列数的最小公倍数。

# Input

第一行是一个数字T，表示有T组数据。
接下来T行，每行第一个数m表示后面还有m个数。

# Output

对每组数据输出这m个数的最小公倍数。

# Sample Input

2
3 5 7 15
6 4 10296 936 1287 792 1

# Sample Output

105
10296

# Author

chuyuaem

# 解题思路

这题，还是是我原来发过题解的，这里就不重复了，附上一个链接。
[点击查看题解](http://www.aemiot.com/gcd-lcm.html "易于编程的最大公约数算法和最小公倍数算法|HDU-Problem-1019")

[/toggle]