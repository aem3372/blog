title: 简易分数运算类
tags:
  - c
  - 分数
id: 741
categories:
  - 经验之谈
date: 2013-03-20 00:19:32
---

利用最大公约数和最小公倍数，我们可以模拟出分数的运算。（最大公约数和最小公倍数的算法，请看[这里](http://www.aemiot.com/gcd-lcm.html "易于编程的最大公约数算法和最小公倍数算法|杭电Problem-1019")）

下面就是我封装的一个简易分数运算类。

1\. 这个分数运算类支持使用两个int类型的值创建它。

2\. 支持四则运算。

3\. 支持向double类型的转换。

[code lang="cpp"]
#ifndef INTDOUBLE_HEADER
#define INTDOUBLE_HEADER
#include

typedef int NUM;
NUM ancalc(NUM t1,NUM t2)
{
    int t;
    if(t1&gt;t2) t=t1,t1=t2,t2=t;
    while(t2!=0)
    {
        t=t2;
        t2 = t1%t2;
        t1 = t;
    }
    return t1;
}
NUM calc(NUM t1,NUM t2)
{
    int t;
    return t1*t2/ancalc(t1,t2);
}

class intDouble{
friend intDouble operator+(const intDouble&amp; A,const intDouble&amp; B);
friend intDouble operator-(const intDouble&amp; A,const intDouble&amp; B);
friend intDouble operator*(const intDouble&amp; A,const intDouble&amp; B);
friend intDouble operator/(const intDouble&amp; A,const intDouble&amp; B);
friend std::ostream&amp; operator&lt;&lt;(std::ostream&amp; os,const intDouble&amp; val);
friend std::istream&amp; operator&gt;&gt;(std::istream&amp; is,intDouble&amp; val);
public:
	intDouble():numerator(0),denominator(1) {}
	intDouble(int nume,int deno):numerator(nume),denominator(deno) {}
	operator double () { return numerator / denominator; }
private:
	int numerator,denominator;
};

intDouble operator+ (const intDouble&amp; A,const intDouble&amp; B)
{
	int	numerator(A.numerator+B.numerator),denominator(calc(A.denominator,B.denominator));
	if(!(denominator%numerator))
	{
		int gcd = ancalc(numerator,denominator);
		numerator /= gcd;
		denominator /= gcd;
	}
	return intDouble(numerator,denominator);
}

intDouble operator- (const intDouble&amp; A,const intDouble&amp; B)
{
	int	numerator(A.numerator-B.numerator),denominator(calc(A.denominator,B.denominator));
	if(!(denominator%numerator))
	{
		int gcd = ancalc(numerator,denominator);
		numerator /= gcd;
		denominator /= gcd;
	}
	return intDouble(numerator,denominator);
}

intDouble operator* (const intDouble&amp; A,const intDouble&amp; B)
{
	int	numerator(A.numerator*B.numerator),denominator(A.denominator*B.denominator);
	if(!(denominator%numerator))
	{
		int gcd = ancalc(numerator,denominator);
		numerator /= gcd;
		denominator /= gcd;
	}
	return intDouble(numerator,denominator);
}

intDouble operator/ (const intDouble&amp; A,const intDouble&amp; B)
{
	int	numerator(A.numerator*B.denominator),denominator(A.denominator*B.numerator);
	if(!(denominator%numerator))
	{
		int gcd = ancalc(numerator,denominator);
		numerator /= gcd;
		denominator /= gcd;
	}
	return intDouble(numerator,denominator);
}

intDouble operator += (intDouble&amp; A,const intDouble&amp; B)
{
	return A = A+B;
}

intDouble operator -= (intDouble&amp; A,const intDouble&amp; B)
{
	return A = A-B;
}

intDouble operator *= (intDouble&amp; A,const intDouble&amp; B)
{
	return A = A*B;
}

intDouble operator /= (intDouble&amp; A,const intDouble&amp; B)
{
	return A = A/B;
}

std::ostream&amp; operator&lt;&lt; (std::ostream&amp; os,const intDouble&amp; val)
{
	os &lt;&lt; val.numerator &lt;&lt; &quot;|&quot; &lt;&lt; val.denominator; 	return os; } std::istream&amp; operator&gt;&gt; (std::istream&amp; is,intDouble&amp; val)
{
	if(val.denominator == 1)
		is &gt;&gt; val.numerator;
	else
		is &gt;&gt; val.numerator &gt;&gt; val.denominator;
	return is;
}
#endif
[/code]

[warning]
作者:Aem
本文版权归作者和www.aemiot.com共有，未征得作者本人同意之前，请勿将本文内容用于任何商业用途。 欢迎非商业用途转载，但请在明显位置注明本文作者和出处链接，否则我们保留追究法律责任的权利。
[/warning]