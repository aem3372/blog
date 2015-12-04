title: '江西师范大学ACM协会新生选拔赛题解'
tags:
  - ACM
id: 766
categories:
  - 算法/数据结构
date: 2013-04-05 01:02:57
---

## 第一题

### Problem Description

Welcome Everyone~~
期待了很久很久滴华丽丽滴江西师范大学ACM协会华丽丽滴在2013年02月22日成立了~一个够二的日子一个不二的组织~
撒花撒花~~
身为协会滴宣传，我灰常灰常感谢各位能来参加偶们滴招新比赛，。。
好吧，说实话今天我滴主要任务就是给大家打气加油～我也就是颗大一小白菜，轻拍板砖，会砸烂的。。=_=|
比赛神马滴真心不难的，绝对滴够水，，泳池水深1.5M。。高台跳水注意安全~
<!--more-->
好吧，比老师滴实验报告总要难点咯，要不然肿么能体现出大家滴实力捏~那些`scanf`半径算面积神马滴，。完全让人木有运行滴冲动啊，有木有啊啊！！！
申明一下，千万不要利用这个机会体现自己和某人那个一往情深啊or那个手足义气啊，。我劝各位哦，这个是要记院级处分滴！！！！！
那么，，神马算作弊捏??
First of all，QQ神马传代码，某些C大神。。你们辛苦了啊，一个人两份代码，吼吼~
Then，度娘再万岁，现在天要下雨度娘要嫁人了，，靠自己咯孩纸~
Last，，额，。。。
哎哎～。。写这种东西，孩纸八辈纸滴节操都木有了。。所以要借这个机会好好滴爆料下偶滴师兄们，呜呜。。（捂住嘴然后拖走中。。。）

请注意请注意!!!!!!!

如果你没有时间没有准备没有毅力每周上课、刷题、比赛~请跳过此题，ACM需要各位付出大量的精力时间，并且不是你付出了，最后就一定会有所回报。。

我们大家未来的路都还很长，所以，我们仅欢迎那些愿意一直一直互相扶持一同前进的战友~WE ARE A TEAM 

### Input

请输入今天的日期（2013330）。 

### Output

然后，就请输入"I volunteer to join the JXNU_ACM,OVER!"（不包括引号）。（TIPS：别忘了换行哦） 

### Sample Input

2013330

### Sample Output

I volunteer to join the JXNU_ACM,OVER!

### Author

吴烨琪 

### Source

ACM协会 

### 我的代码

```cpp
#include <iostream>
using namespace std;
int main(void)
{
    int x;
    cin >> x;
    cout << "I volunteer to join the JXNU_ACM,OVER!" << endl;
    return 0;
}
```

## 第二题

### Problem Description

小KD好头疼出题的事。。。因为题目出难了没人写出来的话，比赛就白办了。。。所以便有了以下这道题
输入两个自然数a,b(0<=b<a<=2^63)，计算a与b的和并输出。
(提示：`long long`使用`I64d`输入输出，`unsigned long long`使用`I64u`输入输出。) 

### Input

RT 

### Output

RT 

### Sample Input

2 1

### Sample Output

3

### Author

李思辰 

### Source

ACM协会 


### 我的代码

```cpp
#include <stdio.h>
unsigned long long a,b;
int main(void)
{
  scanf("%I64u",&a,&b);
  printf("%I64u",a+b);
}
```

## 第三题题解

### Problem Description

在C语言课上，大家学习了3位整数倒序输出如 123->321.要是n位整数，你会做吗？
小KD为了让更多人做出这道题，给出了温（xie）馨（e）提示：用字符串有奇效。（学长我错了。。。我不该透题解） 

### Input

m组测试数据(小于100)，每组一行。每行两个数：第一个数n，第二个数是一个n位整数a（n不超出32位整形）。 

### Output

倒序输出n位整数a。每两个数用回车分开。 

### Sample Input

3 123
3 234
3 333

### Sample Output

321
432
333

### Author

李思辰 

### Source

ACM协会 

### 解题思路

倒序么，有好几种思路呢。
首先，可以利用除10和对10取模完成倒序
其次，你可以用字符串读入，然后反向遍历字符串（当然你整型读入也没关系，使用`sprintf`函数或者`ostringstring`对象即可）；
当然，还有一些其他方法，这里就不多说。
在这里，我选择了第二种方法，利用C++ STL，可以快速写完。

### 我的代码

```cpp
#include <iostream>
#include <string>
using namespace std;
int main(void)
{
    int n;
    string str;
    while(cin >> n)
    {
        cin >> str;
        if(str.at(0)=='-')  { cout << '-';  str = str.substr(1,str.size()-1); }
        cout << string(str.rbegin(),str.rend()) << endl;
    }
    return 0;
}
```

## 第四题

### Problem Description

江西师范大学的食堂每到中午十二点都是十分拥挤的。
设有N个学生打饭，并且每个学生都需要打K种饭/菜。食堂有M名员工，并且每一位员工都面前都有这K种饭/菜(也就是说每个员工都能打K种饭/菜中的任意一种)。训练有素的食堂员工会用一秒钟将饭菜打好。不计其他时间损耗。
小KD为了避开打饭高峰期。要你写一个程序计算学生打完饭需要多长时间。 

### Input

输入的第一行为一个正整数T，表示有T组测试数据；
接下去有T组测试数据，每组测试数据占一行，包含三个整数N，K，M，N表示学生的人数，K表示每个学生要打的饭菜数，M表示食堂员工的人数。
T<=1000
1<=N<=100
1<=K<=10
1<=M<=100 

### Output

对于每组数据，输出一个整数，表示最快需要多少秒才能使所有同学打完饭。 

### Sample Input

2
2 1 1
3 2 2

### Sample Output

2
3

### Author

李思辰 

### Source

ACM协会 

### 解题思路

这题唯一需要注意的就是，当食堂员工数M大于学生数N时，一个时刻也只能有N个员工给N个学生打饭/菜。即当M>N时，与M=N是一样的结果。至于其中数学关系，很容易发现：(n\*k)/m向上取整。

### 我的代码

```cpp
#include <stdio.h>
int main(void)
{
  int t,n,k,m;
  scanf("%d",&t);
  while(t--)
  {
    scanf("%d%d%d",&n,&k,&m);
    if(m>n) m = n;
    printf("%d\n",(n*k-1)/m+1);
  }
  return 0;
}
```

## 第五题

### Problem Description

在计算机导论课上，我们学到了一种字符串压缩方法：字符串中有多个相同字符连在一起，就以字符加出现次数形式表示。如 abcddddddddef 就可以表示为abcd8ef。现小KD制作了些字符串，让你将其压缩。

### Input

多组输入数据，每组一行，字符长度小于20。 

### Output

压缩后的字符串。 

### Sample Input

abcddddddddef
qwertyuiop
aassddffgghhjjkkll

### Sample Output

abcd8ef
qwertyuiop
a2s2d2f2g2h2j2k2l2

### Author

李思辰 

### Source

ACM协会 

### 我的代码

```cpp
#include <stdio.h>
#include <iostream>
#include <ctype.h>
#include <string>
#include <cstring>
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
    while(cin >> st)
    {
        char *p = str;
        e = 0;
        strcpy(arr,st.c_str());
        while(arr[e])
        {
        x = ReX(arr+e);
        if(x==1) p += sprintf(p,"%c",arr[e]);
        else p += sprintf(p,"%c%d",arr[e],x);
        e += x;
        }
        //p += sprintf(p,"\n");
        p += sprintf(p,"\0");
        printf("%s",str);
    }   
    return 0;
}
```

## 第六题

### Problem Description

ACM协会新生赛即将结束。。。小KD头疼的划分数线工作开始了。现在已知有n名同学参加ACM协会新生赛，并且知道每个同学的分数，小KD想知道其中第k名的分数是多少。 

### Input

第一行读入n,k，含义如题所述（n<10000,k不超出int型范围）
接下来n行读入n个整数，第i个整数表示第i个同学的分数。 

### Output

一个整数，表示第k名同学的分数是多少。 

### Sample Input

5 3
600 600 350 420 380

### Sample Output

420

### Author

李思辰 

### Source

ACM协会 

### 解题思路

2种方案。第一种，读入数据，排序，输出结果。第二种，利用STL的`multiset`容器，因为`multiset`建立时就已经是有序状态（平衡引索二叉树，效率也挺高的哦），然后直接拿`multiset`的反向迭代器找到我们需要的结果。这里我选择了第二种。

### 我的代码

```cpp
#include <iostream>
#include <set>
using namespace std;
int main(void)
{
  int n,k;
  int t;
  cin >> n >> k;
  multiset<int> s;
  while(n--)
  {
    cin >> t;
    s.insert(t);  
  }
  multiset<int>::reverse_iterator itor = s.rbegin();
  for(int i=0; i<k-1; ++i)  ++itor;
  cout << *itor << endl;
  return 0;
}
```

## 第七题

### Problem Description

How far can you make a stack of cards overhang a table? If you have one card, you can create a maximum overhang of half a card length. (We're assuming that the cards must be perpendicular to the table.) With two cards you can make the top card overhang the bottom one by half a card length, and the bottom one overhang the table by a third of a card length, for a total maximum overhang of 1/2 + 1/3 = 5/6 card lengths. In general you can make n cards overhang by 1/2 + 1/3 + 1/4 + ... + 1/(n + 1) card lengths, where the top card overhangs the second by 1/2, the second overhangs tha third by 1/3, the third overhangs the fourth by 1/4, etc., and the bottom card overhangs the table by 1/(n + 1). This is illustrated in the figure below.

 [![1056-1](http://www.aemiot.com/wp-content/uploads/2013/04/1056-1.gif)](http://www.aemiot.com/wp-content/uploads/2013/04/1056-1.gif)

The input consists of one or more test cases, followed by a line containing the number 0.00 that signals the end of the input. Each test case is a single line containing a positive floating-point number c whose value is at least 0.01 and at most 5.20; c will contain exactly three digits.

For each test case, output the minimum number of cards necessary to achieve an overhang of at least c card lengths. Use the exact output format shown in the examples.

### Sample Input

1.00
3.71
0.04
5.19
0.00

### Sample Output

3 card(s)
61 card(s)
1 card(s)
273 card(s)

### Source

Mid-Central USA 2001 

### 我的代码

```cpp
#include<iostream>
using namespace std;
int main()
{
    int n;
    double sum;
    double length;
    while(cin>>length)
    {
        if(!length)break;
        sum=0,n=0;
        while(length>sum)
            sum+=1.0/(++n+1);
        cout << n << " card(s)" << endl; 
    }
    return 0; 
} 
```

## 第八题

### Problem Description

Fred Mapper is considering purchasing some land in Louisiana to build his house on. In the process of investigating the land, he learned that the state of Louisiana is actually shrinking by 50 square miles each year, due to erosion caused by the Mississippi River. Since Fred is hoping to live in this house the rest of his life, he needs to know if his land is going to be lost to erosion. 

After doing more research, Fred has learned that the land that is being lost forms a semicircle. This semicircle is part of a circle centered at (0,0), with the line that bisects the circle being the X axis. Locations below the X axis are in the water. The semicircle has an area of 0 at the beginning of year 1\. (Semicircle illustrated in the Figure.) 

### Input

The first line of input will be a positive integer indicating how many data sets will be included (N). 

Each of the next N lines will contain the X and Y Cartesian coordinates of the land Fred is considering. These will be floating point numbers measured in miles. The Y coordinate will be non-negative. (0,0) will not be given.

### Output

For each data set, a single line of output should appear. This line should take the form of: 

“Property N: This property will begin eroding in year Z.” 

Where N is the data set (counting from 1), and Z is the first year (start from 1) this property will be within the semicircle AT THE END OF YEAR Z. Z must be an integer. 

After the last data set, this should print out “END OF OUTPUT.” 

Notes: 

1\. No property will appear exactly on the semicircle boundary: it will either be inside or outside. 

2\. This problem will be judged automatically. Your answer must match exactly, including the capitalization, punctuation, and white-space. This includes the periods at the ends of the lines. 

3\. All locations are given in miles.

### Sample Input

2 
1.0 1.0 
25.0 0.0

### Sample Output

Property 1: This property will begin eroding in year 1\. 
Property 2: This property will begin eroding in year 20\. 
END OF OUTPUT. 

### Source

Mid-Atlantic USA 2001 

### 解题思路

不难发现如下数学关系
x\*x + y\*y = r\*r
PI\*r\*r / 2 = S
S/50向上取整即是我们的结果。
注意：这里PI精度要高一点（我取3.14会WA，取3.1415926就AC）

### 我的代码

```cpp
#include <iostream>
#include <cstring>
#include <cmath>
using namespace std;
const double PI = 3.1415926;
int main(void)
{
  int i,T,res;
  double x,y,val;
  cin >> T;
  for(i=1; i<=T; ++i)
  {
    cin >> x >> y;
    val =PI*(x*x+y*y)/100;
    res = (val-(int)val)?(int)val+1:(int)val;
    cout << "Property "<< i <<": This property will begin eroding in year " << res << "." << endl;
  }
  cout << "END OF OUTPUT." <<endl;
  return 0;
}
```

## 第九题

### Problem Description

HOHO，终于从Speakless手上赢走了所有的糖果，是Gardon吃糖果时有个特殊的癖好，就是不喜欢将一样的糖果放在一起吃，喜欢先吃一种，下一次吃另一种，这样；可是Gardon不知道是否存在一种吃糖果的顺序使得他能把所有糖果都吃完？请你写个程序帮忙计算一下。

### Input

第一行有一个整数T，接下来T组数据，每组数据占2行，第一行是一个整数N（0<N<=1000000)，第二行是N个数，表示N种糖果的数目Mi(0<Mi<=1000000)。

### Output

对于每组数据，输出一行，包含一个"Yes"或者"No"。

### Sample Input

2
3
4 1 1
5
5 4 3 2 1

### Sample Output

No
Yes

### Hint

Hint
Please use function scanf 

### Author

Gardon 

### Source

Gardon-DYGG Contest 2 

### 解题思路

这是一个组合数学的问题呢，吃糖果的顺序构成一个序列，这个序列是由我们来排的，不连续吃同一种糖其实就是序列中没有两个相邻的是同一种糖。那么可以设想将最多的一种糖（假设有max个）果先全部加入序列，然后将余下的糖果插入其中，如果将问题建立在这样一个数学模型上，那么显然只有余下的糖少于max-1个时才可能会出现不符合题意的序列。因此，我们只要找出最大值max，如果max<=sum-max+1就存在，否则不存在。

### 我的代码

```cpp
#include <iostream>
using namespace std;
int main()
{
     int T,n,m,a,max;
     int i,j;
     long long sum;
     cin >> T;
     while(T--)
     {
         sum=0; max=0;
         cin >> n;
         for (j=0;j<n;j++)
         {
             cin >> a;
             if (a>max) max=a;
             sum+=a;
         }
         if (sum-max+1<max) 
  cout << "No" << endl;
         else 
  cout << "Yes" << endl;
     }
    return 0;
}
```

## 第十题

### Problem Description

A simple mathematical formula for e is

$$e=\sum_{i=0}^{n}\frac{1}{i!}$$

where n is allowed to go to infinity. This can actually yield very accurate approximations of e using relatively small values of n.

### Output

Output the approximations of e generated by the above formula for the values of n from 0 to 9\. The beginning of your output should appear similar to that shown below.

### Sample Output

<pre>
n e
- -----------
0 1
1 2
2 2.5
3 2.666666667
4 2.708333333
</pre>

### Source

Greater New York 2000 

### 解题思路

纯数学，不多说。唯一值得提下的就是输出格式不能使printf的g控制符，因为g控制符然后设定精度，那么在精度的最后一位如果是0会被忽略。例如printf("%.10g",3.123456780) 那么输出的会是 3.12345678，显然不符合我们题目的要求。

### 我的代码

```cpp
#include <stdio.h>
#include <ctype.h>
#define MAX 10000

const int JC[10]={1,1,2,6,24,120,720,5040,40320,362880};

double ReX(int n)
{
    int i;
    double x=0;
    for(i=0; i<=n; ++i)
        x += 1.0/JC[i];
    return x;
}

int main(void)
{
    int i;
    printf("n e\n");
    printf("- -----------\n");
    for(i=0; i<=2; ++i)
        printf("%d %.10g\n",i,ReX(i));
    for(i=3; i<=9; ++i)
        printf("%d %.9f\n",i,ReX(i));
    return 0;
}
```

## 第十一题

### Problem Description

The digital root of a positive integer is found by summing the digits of the integer. If the resulting value is a single digit then that digit is the digital root. If the resulting value contains two or more digits, those digits are summed and the process is repeated. This is continued as long as necessary to obtain a single digit.

For example, consider the positive integer 24\. Adding the 2 and the 4 yields a value of 6\. Since 6 is a single digit, 6 is the digital root of 24\. Now consider the positive integer 39\. Adding the 3 and the 9 yields 12\. Since 12 is not a single digit, the process must be repeated. Adding the 1 and the 2 yeilds 3, a single digit and also the digital root of 39.

### Input

The input file will contain a list of positive integers, one per line. The end of the input will be indicated by an integer value of zero.

### Output

For each integer in the input, output its digital root on a separate line of the output.

### Sample Input

24
39
0

# Sample Output

6
3

### Source

Greater New York 2000

### 解题思路

这题，我原来单独发过题解的，这里就不重复了，附上一个链接。
[使用数论知识消除模拟](http://www.aemiot.com/problem-1013.html "使用数论知识消除模拟")
