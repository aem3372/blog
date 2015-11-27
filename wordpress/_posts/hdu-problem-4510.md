title: '水题-时间计算|小Q系列故事——为什么时光不能倒流|HDU-4510'
tags:
  - '4510'
  - ACM
  - hdu
id: 1153
categories:
  - 算法/数据结构
date: 2013-11-24 13:44:51
---

# 题目

[小Q系列故事——为什么时光不能倒流](http://acm.hdu.edu.cn/showproblem.php?pid=4510 "小Q系列故事——为什么时光不能倒流")

# 时空限制

Time Limit: 300/100 MS (Java/Others)    Memory Limit: 65535/32768 K (Java/Others)

# 题目描述

我以为我会是最坚强的那一个 我还是高估了自己
　　我以为你会是最无情的那一个 还是我贬低了自己

　　就算不能够在一起 我还是为你担心
　　就算你可能听不清 也代表我的心意

　　那北极星的眼泪 闪过你曾经的眼角迷离
　　那玫瑰花的葬礼 埋葬的却是关于你的回忆

　　如果时光可以倒流 我希望不要和你分离
　　如果注定分离 我希望不要和你相遇

　　　　——摘自《小Q失恋日记 》第17卷520页

　　这是码农小Q第58次失恋了，也是陷得最深的一次。
　　要知道，小Q自从第一次到腾讯公司报到，就被风姿绰约的前台MM彻底迷住了，这1000多个日日夜夜他无时无刻不在憧憬着他们美好的未来。为了能见到MM，他每天早到晚归，甘愿加班，连续3年被评为优秀员工，并且以全公司最快的速度晋级到四级岗位。就在他终于鼓足勇气准备表白的时候，MM却满面春风地送来了一包喜糖......
　　现在小Q专门请了年休假治疗情伤，但情绪总不见好转，每天足不出户，眼睛盯着墙上的钟表，反复念叨：“表白要趁早，时光不倒流，表白要趁早，时光不倒流......”
　　假设现在已知当前的时间，让时间倒退回若干，你能计算出钟表显示的时间吗？

# Input

输入首先包含一个整数N，表示有N组测试用例。
接下来的N行表示N个测试用例，每行包括2个时间HH:MM:SS hh:mm:ss
HH:MM:SS表示当前的时间，hh:mm:ss表示希望倒退回去的时间。
[Technical Specification]
00<=HH<=11
00<=hh<=99
00<=MM, SS, mm, ss<=59

# Output

请计算并输出钟表倒退后显示的时间，要求输出格式为HH:MM:SS（即时分秒均显示2位，不足则补0），每组数据输出占一行。

# Sample Input

2
11:28:32 02:14:21
05:00:00 96:00:01

# Sample Output

09:14:11
04:59:59

# 解题思路

直接模拟咯。

# 我的代码

[code lang="cpp"]
#include&lt;stdio.h&gt;
int main()
{
    int n,HH,MM,SS,hh,mm,ss;
    while(~scanf(&quot;%d&quot;,&amp;n))
    {
        while(n--)
        {
            scanf(&quot;%d:%d:%d %d:%d:%d&quot;,&amp;HH,&amp;MM,&amp;SS,&amp;hh,&amp;mm,&amp;ss);
            int th,tm,ts;
            if(SS&lt;ss)
            {
                SS+=60;
                MM--;
            }
            ts=SS-ss;

            if(MM&lt;mm)
            {
                MM+=60;
                HH--;
            }
            tm=MM-mm;

            while(HH&lt;hh)
            {
                HH+=12;
            }
            th=HH-hh;

            printf(&quot;%02d:%02d:%02d\n&quot;,th,tm,ts);
        }
    }
    return 0;
}
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]