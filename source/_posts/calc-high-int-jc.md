title: 传统高精度算法系列(三)整数的高精度阶乘
tags:
  - 高精度
id: 762
categories:
  - 算法/数据结构
date: 2013-04-01 13:21:16
---

与[高精度加法](http://www.aemiot.com/calc-high-int-sum.html)和[高精度减法](http://www.aemiot.com/calc-high-int-inc.html)原理类似，计算高精度阶乘实质是多次高精度乘单精度的乘法，同样按照竖向式运算模拟。

<!--more-->

## 我的代码

    #include <stdio.h>
    #define MAX 5000
    int arr[MAX] = {0};
    int main(void)
    {
        int n,i,j,k;
        scanf("%d",&n);
        arr[0]=1;
        k=1; /*当前长度为1*/
        for(i=2; i<=n; ++i)
        {
            /*对于有效部分乘上k*/
            int c = 0;
            for(j=0; j<k; ++j)
            {
                int s = arr[j]*i + c;
                arr[j] = s % 10;
                c = s / 10;
            }
            /*更改有效长度*/
            while(c)
            {
                arr[k++] = c %10;
                c /= 10;
            }
        }
        for(i=k-1; i>=0; --i)
            printf("%d",arr[i]);
        return 0;
    }
