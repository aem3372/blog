title: 传统高精度算法系列(一)整数的高精度加法
tags:
  - 高精度
id: 353
categories:
  - 算法/数据结构
date: 2012-12-30 20:28:21
---

C语言的基本数据类型提供的可计算范围非常有限的，需要处理超过范围的数据时，高精度算法通常可以派上用场（考虑性能的话，未必是最优方案）。

## 算法思路

模拟我们在小学所学的笔算，对于不足位用0补齐，如9818+13，我们认为是9818+0013，在计算机中也同样可以完成这个过程。

<!--more-->

为了方便进位，我们在计算前，将两个数反向储存至数组中。初始化时 dv = 0，其中有恒等式   Cn  = （An + Bn + dv）mod  10  （dv为进位结果）

根据这个等式我们只需要循环执行以下三步：

- temp = An + Bn +dv
- Cn = temp mod 10
- dv = temp div 10

循环完成后，我们还需要进行最后一步，因为两个最高位相加仍可能产生进位，所以我们在这里还要额外做一次对 dv 的判断，如果 dv 值为 1 的话， Cn+1 应该要进位，即 Cn+1 = 1。

## 我的代码

```cpp
#include<stdio.h>
#include<string.h>
#define MAX 1001
int main(void)
{
    int arr1[MAX]={0},arr2[MAX]={0};
    int length,i,length1,length2,t,dv = 0;
    char str[MAX];
    /*读入数据，并进行预处理(计算出数字位数，并方向存放)*/
    scanf("%s",str);
    length1 = strlen(str);
    for(i=0; i<length1; ++i)
        arr1[i] = str[length1-1-i] - '0';
    scanf("%s",str);
    length2 = strlen(str);
    for(i=0; i<length2; ++i)
        arr2[i] = str[length2-1-i] - '0';
    length = (length1>length2)?length1:length2;
    /*算法核心内容*/
    for(i=0; i<length; ++i)
    {
        t = arr1[i] + arr2[i] +dv;
        arr1[i] = t % 10;
        dv = t / 10;
    }
    if(dv != 0) arr1[length++] = dv;
    /*结束*/
    for(i=0; i<length; ++i)
        printf("%d",arr1[length-1-i]);
    return 0;
}
```