title: '解读代码--三角形坐标系|HDU-Problem-4092|2011 Asia Shanghai Regional Contest'
tags:
  - '2011'
  - '4092'
  - ACM
  - hdu
  - Shanghai
  - 区域赛
id: 972
categories:
  - 算法/数据结构
date: 2013-07-09 20:15:49
---

# 题目    Yummy Triangular Pizza

题目地址：[http://acm.hdu.edu.cn/showproblem.php?pid=4092](http://acm.hdu.edu.cn/showproblem.php?pid=4092 "http://acm.hdu.edu.cn/showproblem.php?pid=4092")

# 解题思路

暴力搜索，打表
比较有意思的地方就是，选择不同的方式表示三角形，在搜索的时候效率差异很大。

# 将要解读的代码（重点解析三角形坐标系）

ps:摘自他人（来源网络），这题之所以摘录他人代码，因为他的这种记录三角形方式非常值得我们学习（比我的渣渣方式好很多）

[code lang="cpp"]
#include &lt;iostream&gt;
#include&lt;cstdio&gt;
#include&lt;set&gt;
#include&lt;algorithm&gt;
using namespace std;
#define checkn 16
struct tria
{
    short x,y,z;//三角形三条线，每条线一个值
    tria() {}
    tria(short _x,short _y,short _z)
    {
        x=_x;
        y=_y;
        z=_z;
    }
    tria add(short d)//3个方向添加时添加的三角形
    {
        short mov=(x+y+z==0?1:-1);
        if(d==0)
        {
            return tria(x,y+mov,z+mov);
        }
        else if(d==1)
        {
            return tria(x+mov,y,z+mov);
        }
        else
        {
            return tria(x+mov,y+mov,z);
        }
    }

    void rotate()//120度，线变换
    {
        int tmp=x;
        x=y;
        y=z;
        z=tmp;
    }
    void updown()//180度，上下三角形变换
    {
        x=-x+1,y=-y+1,z=-z;
    }

    void mov(short mx,short my,short mz)
    {
        x=x+mx,y=y+my,z=z+mz;
    }
};
bool operator==(const tria &amp;a,const tria &amp;b)
{
    return a.x==b.x&amp;&amp;a.y==b.y&amp;&amp;a.z==b.z;
}
bool operator&lt;(const tria &amp;a,const tria &amp;b)
{
    if(a.x!=b.x)return a.x&lt;b.x;
    else if(a.y!=b.y)return a.y&lt;b.y;
    else return a.z&lt;b.z;
}
#define maxn 16

struct hashnode
{
    short h[16];
    short hn;
};
bool operator&lt;(const hashnode &amp;a,const hashnode &amp;b)
{
    if(a.hn!=b.hn)return a.hn&lt;b.hn;
    for(int i=0;i&lt;a.hn;i++)
    {
        if(a.h[i]&lt;b.h[i])
            return true;
        else if(a.h[i]&gt;b.h[i])
            return false;
    }
    return false;
}

struct pizza
{
    tria arr[maxn];
    short an;
    pizza()
    {
        an=0;
    }

    bool add(const tria &amp;t)
    {
        for(short i=0; i&lt;an; i++)
        {
            if(arr[i]==t)//已经添加过该三角形
            {
                return false;
            }
        }
        arr[an++]=t;
        return true;
    }

    void remove()
    {
        an--;
    }
    hashnode Hash()
    {
        hashnode ret;
        ret.hn=an;
        for(short i=0;i&lt;an;i++)
        {
            ret.h[i]=arr[i].x+8;
            ret.h[i]&lt;&lt;=4;
            ret.h[i]+=arr[i].y+8;
            ret.h[i]&lt;&lt;=4;
            ret.h[i]+=arr[i].z+8;
        }
        return ret;
    }
};

bool operator&lt;(const pizza &amp;a,const pizza &amp;b)
{
    if(a.an!=b.an)return a.an&lt;b.an;
    for(short i=0; i&lt;a.an; i++)
    {
        if(a.arr[i]&lt;b.arr[i])
        {
            return true;
        }
        else if(b.arr[i]&lt;a.arr[i])
        {
            return false;
        }
    }
    return false;
}
int ans[maxn+9]= {0};

set&lt;hashnode&gt; sp;

bool canFind(pizza p)
{
    for(short i=0; i&lt;3; i++)
    {
        sort(p.arr,p.arr+p.an);
        if(sp.find(p.Hash())!=sp.end())
            return true;

        for(short j=0; j&lt;p.an; j++)
        {
            p.arr[j].rotate();
        }
    }
    return false;
}

bool Find(pizza tmp)
{
    for(short i=0; i&lt;tmp.an; i++)
    {
        short x=tmp.arr[i].x,y=tmp.arr[i].y,z=tmp.arr[i].z;
        if(x+y+z==0)
        {
            pizza t2=tmp;

            for(short j=0; j&lt;t2.an; j++)
            {
                t2.arr[j].mov(-x,-y,-z);
            }

            if(canFind(t2))
                return true;

        }
        else
        {
            pizza t2=tmp;
            for(short j=0; j&lt;t2.an; j++)
            {
                t2.arr[j].updown();
                t2.arr[j].mov(x-1,y-1,z);
            }

            if(canFind(t2))
                return true;
        }

    }
    return false;
}
void Insert(pizza p)
{
    sort(p.arr,p.arr+p.an);
    sp.insert(p.Hash());
    for(short i=0;i&lt;p.an;i++)
    {
        p.arr[i].rotate();
    }
    sort(p.arr,p.arr+p.an);
    sp.insert(p.Hash());
    for(short i=0;i&lt;p.an;i++)
    {
        p.arr[i].rotate();
    }
    sort(p.arr,p.arr+p.an);
    sp.insert(p.Hash());
}
void dfs(pizza p)
{
    if(p.an==checkn+1)return ;
    if(Find(p))
    {
        return;
    }
    ans[p.an]++;
    Insert(p);

    for(short i=0; i&lt;p.an; i++)
    {
        for(short j=0; j&lt;3; j++)
        {
            tria t=p.arr[i].add(j);
            if(p.add(t))
            {
                dfs(p);
                p.remove();
            }
        }
    }
}
int ans2[maxn+10]={0,1,1,1,4,6,19,43,120,307,866,
                   2336,6588,18373,52119,147700,422016};
int main()
{
    pizza p;
    p.add(tria(0,0,0));
    dfs(p);
    cout&lt;&lt;&quot;{&quot;;
    for(int i=0; i&lt;=maxn; i++)
    {
        cout&lt;&lt;ans[i]&lt;&lt;',';
    }
    cout&lt;&lt;&quot;};&quot;&lt;&lt;endl;
    return 0;
/*
    int t;
    cin&gt;&gt;t;
    for(int ti=1;ti&lt;=t;ti++)
    {
        int n;
        cin&gt;&gt;n;
        cout&lt;&lt;&quot;Case #&quot;&lt;&lt;ti&lt;&lt;&quot;: &quot;;
        cout&lt;&lt;ans2[n]&lt;&lt;endl;
    }*/
}

[/code]

# 代码解析

刚入手这段代码的时候，因为备注少，愣是没看懂这代码= =
后来慢慢品味，倒也看出些名堂来，于是就拿出来分享了。
首先看他的三角形类（这种结构便于旋转x60度）：
short x,y,z;//三角形三条线，每条线一个值
既然每条线一个值，我们就要建立起一个坐标系，我们大概可以想到的是如图所示的情况：

[![hdu-4092-1](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-1.bmp)](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-1.bmp)
当然就这一点，很难认识到这个坐标系的全貌以及巧妙

接下来看类的这个成员函数----tria add(short d);
在这个成员函数中，mov = (x+y+z==0?1:-1); 这句是最难理解的作用，但是它的作用，其实只要动手画一画就可以知道，如下组图：

[![hdu-4092-2](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-2.bmp)](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-2.bmp)

[![hdu-4092-3](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-3.bmp)](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-3.bmp)

[![hdu-4092-4](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-4.bmp)](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-4.bmp)

最后发现三种线的走势如下图：

[![hdu-4092-5](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-5.bmp)](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-5.bmp)

可见mov = (x+y+z==0?1:-1);的强大....说实在我不知道这是什么数学性质，只是暂时记住这种用法了（如果数学高手看明白了，望留言告诉我）。

接下来看看在这种坐标系下如何旋转：

[![hdu-4092-6](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-6.bmp)](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-6.bmp)

void rotate()//120度，线变换；

[![hdu-4092-7](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-7.bmp)](http://www.aemiot.com/wp-content/uploads/2013/07/hdu-4092-7.bmp)

void updown()//180度，上下三角形变换
这里可以看到，-x+1的反函数是它自身，-z的反函数也是它自身，这样的数学性质也正好体现两次180度旋转会原地不动的现象。

三角形的x60度的旋转都可以通过上述两种旋转合成得到，因此，使用这种坐标系是非常好的。

接下来的一系列操作时ACMer很熟悉的通过hash进行状态压缩，然后通过搜索获取所有可能，计算结果，输出为数组格式，打表。这里就不详细解释了，重点是介绍下发现的这种三角形坐标系。~

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]