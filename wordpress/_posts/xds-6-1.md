title: 线段树介绍｜六一大礼包
tags:
  - 线段树
id: 851
categories:
  - 算法/数据结构
date: 2013-06-01 01:20:45
---

线段树适合解决N次成段更新的问题。例如，给你N条不超过K的线段，然后有M次询问，每次询问要求你回答一个点出现了几次。当给你的线段是[2,5] [4,6] [0,7] 时，依次问你2出现了多少次？4出现了多少次？7出现了多少次？你应该要给出的答案是 2， 3， 1。

一个很容易想到的算法就是建立一个N*2的数组，读入每个线段的首尾值记录在数组中，每次询问遍历整个数组逐个判断得出结果。
不可否认这是一种解决办法，但是在大数据面前，它显得非常不合理。（数据量的不同决定了算法，在不考虑输入数据的情况下，讨论算法的优劣是没有意义的。）

上面的算法每次询问需要遍历整个数组，因此时间复杂度是O（M*N），空间复杂度是O（N*2）。

另一个很容易想到的算法就是开拓一个很大的数组，读入一个线段的首尾值后，将线段范围内的点都更新。那么询问的时候，每次询问的时候直接读取下标就可以了。

这种算法的复杂度取决于出现的线段最长值K和线段多少的N，时间复杂度是O（K*N），空间复杂度是O（K）。

可以说第一种算法适用于线段数目不多，查询也不多的情况，第二种算法适用于线段的最大长度很小，线段数量不多，但是询问次数非常多的情况。

可以说这两种方法对数据都有种严格的要求。一个较为折中的考虑就是采用线段树来存储数据。基于线段树的算法将会带给你 O（（M+N）*logK）的时间复杂度，代价只是空间复杂度的上升（现在机器内存都已经很大了，在这种问题上时间显得比空间重要）。

首先我们来看看线段的树的基本结构

[code lang="c"]
struct Node{
int left,right;
int val;
Node *lefttree,*righttree;
}
[/code]

除了树的基本结构之外，主要是多了一个左端点值和一个右端点值。

例如，最大长度为7 的线段树，如图所述:

[![xds-1](http://www.aemiot.com/wp-content/uploads/2013/06/xds-1.gif)](http://www.aemiot.com/wp-content/uploads/2013/06/xds-1.gif)

不难发现，任何一条长度不超过7的线段都可以分割为若干条线段放入树中。（因为我们查找时只关心点，所以线段可以分割。）
因为是二叉树，因此空间复杂度是O（K*2）。

依次向其中加入 [2,5] [4,6] [0,7] 三条线段，线段树结点值的变化：

[![xds-2](http://www.aemiot.com/wp-content/uploads/2013/06/xds-2.gif)](http://www.aemiot.com/wp-content/uploads/2013/06/xds-2.gif)

**                                                   加入[2,5]**

[![xds-3](http://www.aemiot.com/wp-content/uploads/2013/06/xds-3.gif)](http://www.aemiot.com/wp-content/uploads/2013/06/xds-3.gif)

**                                                   加入[4,6]**

[![xds-4](http://www.aemiot.com/wp-content/uploads/2013/06/xds-4.gif)](http://www.aemiot.com/wp-content/uploads/2013/06/xds-4.gif)

**                                                  加入[0,7]**

可以看出，[2,5]被分割为 [2,3]和[4,5]存入了树中,[4,6]被分割为 [4,5]和[6-6]存入了树中,[0-7]直接存入了树中。

因为这种储存方式，使得它在加入一个线段的平均时间复杂度为O（logK）。并且当我们要查询某个点的时候，我们只需要沿着二叉树的某条路径从树的顶部走向底部，将沿途的值都加起来即可。例如，查询端点5，首先在树根[0,7]获得了1，向右走,在[4,7]没有获得，再向左走，在[4,5]获得了2，向右走，在[5,5]没有获得，因此得出结果为 1+2 = 3。正因为它是从树根走向叶子节点所以它的时间复杂度也是O（logK）。因此线段树总体的时间复杂度是O（（M+N）*logK）。

这就使得它在空间足够的时候，对大数据的处理，也是高效的。但是对于小数据，而K又特别大的时候，建树的过程非常耗时，这就很不划算了。因此，是否采用线段树，要看数据的情况。

# 线段树样例代码

[code lang="c"]
#include &lt;stdio.h&gt;
#include &lt;malloc.h&gt;
typedef struct Node{
    int left,right;
    int mid; /*这个只是为了减少重复计算*/
    int val;
    Node *lefttree,*righttree;
}Node,*PNode;

PNode build(PNode pnode,int left,int right)
{
    if(pnode == NULL)
    {
        pnode = (PNode) malloc(sizeof(Node));
        pnode-&gt;left = left;
        pnode-&gt;right= right;
        pnode-&gt;mid  = left+((right - left)/2 + 1);
        pnode-&gt;val  = 0;
        pnode-&gt;lefttree = NULL;
        pnode-&gt;righttree= NULL;
    }
    if(left != right)
    {
        pnode-&gt;lefttree = build(pnode-&gt;lefttree,left,pnode-&gt;mid-1);
        pnode-&gt;righttree= build(pnode-&gt;righttree,pnode-&gt;mid,right);
    }
    return pnode;
}

void add(PNode pnode,int left,int right)
{
    if(left==pnode-&gt;left &amp;&amp; right==pnode-&gt;right)
    {
        ++pnode-&gt;val;
        return ;
    }
    if(left &lt; pnode-&gt;mid)
    {
        add(pnode-&gt;lefttree,left,(right&gt;pnode-&gt;mid-1)?pnode-&gt;mid-1:right);
    }
    if(right &gt;= pnode-&gt;mid)
    {
        add(pnode-&gt;righttree,(left&lt;pnode-&gt;mid)?pnode-&gt;mid:left,right);
    }
}

int search(PNode pnode,int val)
{
    int res = pnode-&gt;val;
    if(val &lt; pnode-&gt;mid &amp;&amp; pnode-&gt;lefttree != NULL)
        res += search(pnode-&gt;lefttree,val);
    if(val &gt;= pnode-&gt;mid &amp;&amp; pnode-&gt;righttree != NULL)
        res += search(pnode-&gt;righttree,val);
    return res;
}

/*用于调试的操作*/
#define MAX 100
PNode buf[MAX];
int first=0, last=0;

void push(PNode x)
{
    if(last &lt;= MAX)
        buf[last++] = x;
    else
        printf(&quot;Quere rangle error!&quot;);
}

PNode pop()
{
    if(first &lt; last)
        return buf[first++];
    else
        printf(&quot;Quere rangle error!&quot;);
    return NULL;
}

int buf_size()
{
    return last-first;
}

void print(PNode pnode)
{
    PNode now;
    push(pnode);
    while(buf_size())
    {
        now = pop();
        printf(&quot;%d - %d\t%d\n&quot;,now-&gt;left,now-&gt;right,now-&gt;val);
        if(now-&gt;lefttree != NULL)
            push(now-&gt;lefttree);
        if(now-&gt;righttree != NULL)
            push(now-&gt;righttree);
    }
    printf(&quot;\n&quot;);
}
/*用于调试的操作--END*/

int main()
{
    PNode root = NULL;
    root = build(root,0,7);
    add(root,2,5);
    add(root,4,6);
    add(root,0,7);
    print(root);
    int t;
    while(scanf(&quot;%d&quot;,&amp;t)==1)
        printf(&quot;%d\n&quot;,search(root,t));
}

[/code]