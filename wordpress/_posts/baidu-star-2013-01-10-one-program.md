title: 百度之星2013.01.10第一题题解
tags:
  - '2013'
  - 百度之星
  - 算法
id: 417
categories:
  - 算法/数据结构
date: 2013-01-11 13:59:58
---

## [2013年1月10号竞赛题目一](/index.php?r=home/detail&amp;id=18)

**主办方：**百度公司

**时间：**2013-01-10 18:00 至 2013-01-11 01:30
<div>

&nbsp;

聚会游戏
<div align="center">

* * *

</div>

Time Limit: 1 Seconds   Memory Limit: 65536K

<div align="center">

* * *

</div>
&nbsp;

百度之星总决赛即是一群编程大牛一决高下的赛场，也是圈内众多网友难得的联欢，在为期一周的聚会中，总少不了各种有趣的游戏。

某年的总决赛聚会中，一个有趣的游戏是这样的：

&nbsp;

游戏由Robin主持，一共有N个人参加（包括主持人），Robin让每个人说出自己在现场认识的人数（如果A认识B，则默认B也认识A），在收到所有选手报出的数据后，他来判断是否有人说谎。Robin说，如果他能判断正确，希望每位选手都能在毕业后来百度工作。

为了帮Robin留住这些天才，现在请您帮他出出主意吧~

特别说明：

1\. 每个人都认识Robin；

2\. 认识的人中不包括自己；

&nbsp;

**Input**

&nbsp;

输入数据包含多组测试用例，每组测试用例有2行，首先一行是一个整数N (1&lt;N&lt;=100)，表示参加游戏的全部人数，接下来一行包括N-1个整数，表示除主持人以外的其余人员报出的认识人数。

N为0的时候结束输入。

&nbsp;

**Output**

&nbsp;

请根据每组输入数据，帮助主持人Robin进行判断：

如果确定有人说谎，请输出“Lie absolutely”

否则，请输出“Maybe truth”

每组数据的输出占一行。

&nbsp;

**Sample Input**

7

5 4 2 3 1 5

7

3 4 2 2 2 3

0

&nbsp;

**Sample Output**

Lie absolutely

Maybe truth

&nbsp;

这题应该不难。

我解题的突破口是主持人带给我的，因为我觉得对数据的第一个处理应该是每个人减去一个认识的人，之后就可以排除掉主持人了。

对于这个过程进行拓展可归纳为如下过程：

A为数组，存放读入数据

man为参加游戏人数

n为数据个数，n=man-1

将读入数据进行从大到下排序（这样最后对认识人少的人执行判断，可以给他们更多选择余地）。

每个数据自减1。（除去认识的主持人）

对A(0)-A(n-1) 的每个数据进行判断（判定过程：当前要判定数据记作A(k)，我们从A(k+1)-A(n)之中找出A( k)个在数组中尽可能靠前的非零数据自减1。如果这个过程能够完成，那么就将A(k)变为0，继续下一个数据判定；如果不能完成，那么有人说谎）

如果所有数据都判定为真，最终数据都变成0的话，那么没有人说谎。

&nbsp;

对于样例输入的第一组数据的判定过程：

5 4 2 3 1 5

5 5 4 3 2 1

4 4 3 2 1 0 （除去每个人都认识的主持人）

0 3 2 1 0 0

这是在 A1的后面找不出3个非零数据，因此有人说谎

&nbsp;

对于样例输入的第二组数据的判定过程：

3 4 2 2 2 3

4 3 3 2 2 2

3 2 2 1 1 1（除去每个人都认识的主持人）

0 1 1 0 1 1

0 0 0 0 1 1

0 0 0 0 0 0

全部都为真，因此没有人说谎。

&nbsp;

附上我的代码：
[code lang="cpp"]

#include&lt;iostream&gt;
using namespace std;
int main(void)
{
    int num;
    while(cin&gt;&gt;num,num)
    {
        int arr[100]={0},res = 1;//res表示当前是否有人说谎，默认没有人说谎
        //读入数据，并且每个人自减1（减去认识的主持人）
        for(int n=0; n &lt; num -1; ++n)
        {
            cin&gt;&gt;arr[n];
            --arr[n];
        }
        //从大到小排序，因为数据范围较小，采用选择排序
        for(int i=0; i &lt; num -1; ++i)
        {
            int max = i;
            int t;
            for(int j=i+1; j &lt; num -1; ++j)
                if(arr[j]&gt;arr[max]) max = j;
            t = arr[max],arr[max] = arr[i],arr[i] =t;
        }
        //判定开始
        for(int i=0; i&lt;num-2 &amp;&amp; res==1; ++i)
        {
            int minip = i+1;
            if(!arr[i]) continue;
            for(int j=0; j&lt;arr[i]; ++j,++minip)
            {
                if(!arr[minip])
                {
                    if(minip == num-1)   { res=0;break; }//发现有人说谎
                    --j;
                    continue;
                }
                --arr[minip];//执行自减
            }
            arr[i] = 0;
        }
        //最后一次核查，避免输入数据为下面的情况，因为我们没有对最后一个数据判定
        //2
        //9
        //0
        for(int i=0; i&lt;num-1; ++i)
            if(arr[i]) res = 0;
        //输出结果
        if(res) cout &lt;&lt; &quot;Maybe truth&quot; &lt;&lt; endl;
        else cout &lt;&lt; &quot;Lie absolutely&quot; &lt;&lt; endl;
    }
}
[/code]

</div>

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]