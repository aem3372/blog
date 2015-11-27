title: C++类类型的转换
tags:
  - c
  - 类
  - 转换
id: 620
categories:
  - 经验之谈
date: 2013-02-10 05:54:45
---

利用C++的类类型转换规则，可以迅速增加一个类与其他类型建立关系，变得和一个内置类型一样好用。

例如，我们定义一个SmallInt类型来作为unsigned char 的**安全版本**。

[code lang="cpp"]
class SmallInt{
public:
    SmallInt():val(0) {}
    SmallInt(unsigned char t):val(t)
{
    if(val &amp;lt; 0 || val&amp;gt;255)
        throw std::out_of_range(&quot;Out of range&quot;);
}
pravate:
    unsigned char val;
};
[/code]

那么为了使这个类和内置类型一样好用，需要**定义**各式各样的**运算符**，例如 **+** 、 **-** 、 ***** 等 。而且每个运算符还要重载多个版本，因为我们可能执行SmallInt 和 int 的运算，也可能执行int 和 SmallInt 的运算，以及SmallInt 和SmallInt 的运算。即使定义了这些运算符，原来需要unsigned char 的部分也需要改写，那么这是非常不便的。为此，我们可以借助定义类转换来完成这一切。

&nbsp;

# 定义类类型向其他类型的转换

定义类类型向其他类型的转换非常简单，只需要在类定义中加入一个成员函数。注意：这个成员函数不能有返回值和形参。

成员函数格式： operator type() { /*转换规则*/ }

例如在SmallInt类定义中加入 operator int() { return val; }

当完成了这个转换定义之后，凡是需要使用int 类型的地方，就能够使用SmallInt 类型，因此我们不需要额外定义各种各样的运算符，原来的代码也不需要改写。具体情况如下：

SmallInt  v(3);

int  x = v;  // v 首先转换为 int 类型，随后对 x 初始化。

&nbsp;

# 定义其他类型向类类型的转换

定义其他类型向类类型的转换，只需要存在一个对应类型为**单形参**的**构造函数**。

你会发现，我们其实在无意之中已经定义了这个隐式的转换，就是这个函数完成了这个定义：

SmallInt(unsigned char t):val(t)

{

if(val &lt; 0 || val &gt; 255)

throw std::out_of_range("Out of range");

}

当完成了这个转换定义之后，凡是需要使用SmallInt 类型的地方，就能够使用int 类型。具体情况如下：

SmallInt calc(int x)

{

return x;

}  // x首先转换为SmallInt类型，然后返回它的副本。

如果想要**屏蔽**这个隐式转换而保留这个构造函数，只要用**explicit** 修饰这个构造函数即可。

&nbsp;

# 注意事项1

C++转换链中不能出现两次**隐式**的**类类型转换**，但**内置类型转换**则不受此限制。

[code lang="cpp"]
#include &lt;iostream&gt;

using namespace std;

struct B{
    B(double t):x(t) {}
    operator int() {return x;}
    double x;
};

struct A{
    A(double t):x(t) {}
    operator B() {return x;}
    double x;
};

int calc(int x)
{
    return x;
}
/*A可以转换为B
B可转换为int
但A不能隐式经过两次类类型转换变为为int
要将A转换到int，需要先强制转换到B类型*/

int main(void)
{
    A d(6);
    cout &lt;&lt; calc( static_cast&lt;B&gt;(d) ) &lt;&lt; endl;
    return 0;
}
[/code]

如果调用calc 时，**不使用强制转换**而直接使用d ，编译器会报错。

&nbsp;

# 注意事项2

类的转换很容易产生**多义性**。其中，当**两个类同时定义了同一个转换时当一个类同时定义了两个有转换关系的类型的转换操作符或构造函数时**通常会产生多义性。

**<span style="color: #ff6600">当两个类分别定义了同一个转换时，产生二义性情况如下</span>**：

[code lang="cpp"]

#include &lt;iostream&gt;

using namespace std;

struct A;
struct B{
    B(double t):x(t) {}
    B(A t); //A向B转换
    double x;
};

struct A{
    A(double t):x(t) {}
    operator B() {return x;} //A向B转换
    double x;
};

B::B(A t):x(t.x){}

B calc(B x)
{
    return x;
}

int main(void)
{
    A d(6);
    calc(d);
    return 0;
}

[/code]

这种情况，编译器不知道是调用**B 的构造函数**来完成转换，还是**A 的转换操作符**来完成转换，因为这两个函数没有高下之分，因而产生二义性。对于这种情况，即便使用强制转换也无法避免出错。但是有两个方法可以解决它：一、显式调用，二、改变构造函数。

一、**显式调用**

calc( B(d) ); // 使用B 的构造函数完成转换

calc( A.operator B(d) ); //使用A 的转换操作符完成转换

二、**改变构造函数**

将B(A); 改为 B(const A&amp;); 使得构造函数匹配性低于转换操作符

**此外，不要在两个类中分别定义同一个转换才是根本的解决办法。**

**<span style="color: #ff6600">当一个类同时定义了两个有转换关系的类型的转换操作符或构造函数时，产生二义性情况如下</span>**：

[code lang="cpp"]

#include &lt;iostream&gt;

using namespace std;

struct A{
    A() {}
    A(double t):x(t) {}
    A(int t):x(t) {}
    double x;
};

int main(void)
{
    int i = 1;
    double d = 2.0;
    long l = 3;
    A obj1(i);
    A obj2(d);
    //A obj3(l);
    //error：long转换到double和int不分高下
    return 0;
}

[/code]

这种错误情况，可以使用指定一个类型的强制转换来处理。不过，最好还是避免同时定义和两个有转换关系的类型的转换。

&nbsp;

# 总结

由此可见隐式转换虽然很方便，让人感觉不到是在使用类类型，而产生是在使用内置类型的错觉，但是不能过度依赖（如果因为此类问题出错，编译器给出的提示通常让人难以理解），你任然需要记住这是一个类类型而不是内置类型。所以，使用过程中任然提倡使用显式转换以确保无误。

&nbsp;

参考书籍：《C++ Primer》Stanley B. Lippman &amp; Josée Lajoie

[warning]

作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。

[/warning]