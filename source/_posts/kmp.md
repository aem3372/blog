title: '阅读Matrix67的《KMP算法详解》'
tags:
  - KMP
  - 字符串
  - 模式匹配
id: 809
categories:
  - 算法/数据结构
date: 2013-05-14 13:06:33
---

说到KMP算法，个人还是非常喜欢Matrix67写的，因此忍不住就直接引用他的了。

> [KMP算法详解|Matrix67博客](http://www.matrix67.com/blog/archives/115 "KMP算法详解|Matrix67博客")

<!--more-->

## 附上我按以上算法写出的C语言代码

```cpp
#include <iostream>
#include <cstring>
#include <assert.h>
using namespace std;

int* Build(char Patter[], int n)
{
    assert(n>0);
    int *next = new int[n];
    next[0] = -1;
    int j = -1;
    int i;
    for(i=1; i<n; ++i)
    {
        while(j>-1 && Patter[j+1] != Patter[i])
            j = next[j];
        if(Patter[j+1] == Patter[i])
            ++j;
        next[i] = j;
    }
    return next;
}

int Count(char Text[],char Patter[], int Next[], int m, int n)
{
    assert(n>0 && m>0);
    if(n==0 || m==0)
        return 0;
    int i;
    int j = -1;
    int ans = 0;
    for(i=0; i<m; ++i)
    {
        while(j>-1 && Patter[j+1] != Text[i])
            j = Next[j];
        if(Patter[j+1] == Text[i])
            ++j;
        if(j+1 == n)
        {
            ++ans;
            j = Next[j];
        }
    }
    return ans;
}

void Clear(int *next)
{
    delete [] next;
}

int main()
{
    char Patter[] = "aba";
    char Text[]   = "aabababaaaaaabab";
    int* Next = NULL;
    Next = Build(Patter,strlen(Patter));
    cout << Count(Text,Patter,Next,strlen(Text),strlen(Patter)) << endl;
    Clear(Next);
    return 0;
}
```