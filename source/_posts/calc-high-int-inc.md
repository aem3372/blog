title: 传统高精度算法系列(二)整数的高精度减法
tags:
  - 高精度
id: 578
categories:
  - 算法/数据结构
date: 2013-02-07 00:24:47
---

按照[整数的高精度加法](http://www.aemiot.com/calc-high-int-sum.html "整数的高精度加法")同样的思路，我们也可以写出整数的高精度减法。当然，高精度减法比加法稍微麻烦一些。

## 算法思路

模拟我们在小学所学的笔算，对于不足位用0补齐，如9818-13，我们认为是9818-0013。

<!--more-->

注意，模拟的时候，要保证大数字减小数字，不然会出错。错误情况如下 0 0 1 3 - 9 8 1 8 = -9 -8 0 -5。这里比较大小，可以直接自己手动写一段代码从两个有效数组末端向前一一对比来判断，也可以将有效部分倒置，然后利用strcmp()比较。（笔者这里使用了前者）。如果现在是小数减大数，只须记录下符号为负号，同时交换两个数，使得大数减小数即可。

对于借位的处理，这里也出现了两种分歧。一种是假设每次都借位了（即先减去 1），如果实际上之前没有借位，就补上 1（可以用之前的差对10取模判断）。另一种是先判断有没有借位，然后记录下来，每次根据之前有没有借位采取不同的表达式（我习惯使用前者）。

同之前一样数是反向存储在数组中的。初始化时 dv = 1（因为对最低位默认之前没有借位），其中有恒等式 Cn = (10 + An - Bn - 1 + dv) mod 10 （dv为之前的借位情况，如果需要借位，dv=0，如果不需要借位 dv=1，这个等式的意义是：先从高位借一位然后减去减数，再减去之前借的一位，如果之前不需要借位，用dv补上）。

根据这个等式我们只需要循环执行以下三步，就能得到大致结果：

- temp = 10 + An - Bn - 1 + dv
- Cn = temp mod 10
- dv = temp div 10

注意，最后输出的时候，先判断符号是否为负号，如果是就额外打印一个负号。

## 我的代码

```cpp
#include <stdio.h>
#include <string.h>
#define MAX 1001
int main(void)
{
    int arr1[MAX] = { 0 }, arr2[MAX] = {0};
    int length, temp, i, length1, length2, t, dv = 1, sign=1;
    char str[MAX];   /* 读入数据，并进行预处理(计算出数字位数，并反向存放) */
    scanf("%s", str);
    length1 = strlen(str);
    for (i = 0; i < length1; ++i)
        arr1[i] = str[length1 - 1 - i] - '0';
    scanf("%s", str);
    length2 = strlen(str);
    for (i = 0; i <  length2; ++i)
        arr2[i] = str[length2 - 1 - i] - '0';
    length = (length1 > length2) ? length1 : length2;
    /*调整两个数组，保证arr1是大数字，arr2是小数字,如果想加快运行速度，可以用两个指针分别指向大数和小数来避免交换*/
    if(length1<length2)
    {
        sign=-1;
        for(i=0; i<length; ++i)
            temp=arr1[i], arr1[i]=arr2[i], arr2[i]=temp;
    }
    else if(length1==length2)
    {
        for(i=length-1; i>=0; --i)
            if(arr1[i]<arr2[i])
            {
                sign=-1;
                for(i=0; i<length; ++i)
                    temp=arr1[i], arr1[i]=arr2[i], arr2[i]=temp;
                break;
            }
    }
    /* 算法核心内容 */
    for (i = 0; i < length; ++i)
    {
        t = 10+arr1[i] - arr2[i] - 1 + dv;
        arr1[i] = t % 10;
        dv = t / 10;
    }
    /* 结束 */
    if(sign == -1)
        printf("-");
    for(i=0; i<length-1; ++i)
        if(arr1[length-1-i]!=0)
            break;
    for (i = i; i < length; ++i)
        printf("%d", arr1[length - 1 -i]);
    return 0;
}
```