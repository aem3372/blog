title: 不用中间变量实现交换过程
tags:
id: 424
categories:
  - 算法/数据结构
date: 2013-01-14 21:11:29
---

假设要交换元素分别为a,b：

- a ← a+b
- b ← a-b
- a ← a-b

使用程序，验证一下

<!--more-->

```cpp
#include <stdio.h>
int main(void)
{
    int a=2,b=3;
    a = a+b;
    b = a-b;
    a = a-b;
    printf("a=%d,b=%d\n", a,b);
    return 0;
}
```

输出结果为：

a=3,b=2

可见交换成功了。分析它的实现过程，进行推广得：

设t=fun(x,y)是二元原函数，f1(t,x)和f2(t,y)是它的两个反函数，那么存在以下三个关系：

- t=fun(x,y)
- x=f1(t,y)
- y=f2(t,x)

省去中间变量t，可以得知以下三个表达式即可完成交换（C语言描述，其中 fun是原函数，f1,f2是它的两个反函数）

- x=fun(x,y)
- y=f1(t,y)
- x=f2(t,x)

按照这个推广的结论，乘除，平方开方也能够完成一部分交换，之所以说他们只能完成一部分交换，因为除法运算的定义域不是R，开方运算的定义域也不是R，所以乘法和平方在这个算法中都不是一个好的原函数。其中对于整型数来说，最为理想的原函数应该是异或运算，因为异或运算的反函数是它本身。
