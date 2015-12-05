title: '剔除多余括号(94NOI)'
tags:
  - NOI
id: 686
categories:
  - 算法/数据结构
date: 2013-02-27 01:01:52
---

## 题目名称

剔除多余括号(94NOI)

## 题目描述

键盘输入一个含有括号的四则运算表达式，表达式中可能含有多余的括号，编程整理该表达式，去掉所有的多余括号，使得原表达式中所有的变量和运算符相对位置保持不变，并保持与原有的表达式等价。

<!-- more -->

## 思路

个人觉得这题应该采用**模拟策略**。从最外层开始，一层层深入表达式（**分冶**的思想），从外层向内部一层层去掉括号。实现上，采用递归处理最合适。递归过程中，如果最外层就是括号，那么忽略这个括号（这个表达式的最低运算符，就是内部表达式的最低级运算符）；如果最外层不是括号，那么根据最低级运算符可以将表达式拆为两个表达式，解出左右两个表达式的最低级运算符（递归求解）再根据四则运算与括号的关系来判断是否需要括号。

假如我们要处理( ((a+b)\*f)-(i/j) ) 这个表达式，按照上述思路处理的图示：

[![述思路处理的图示](http://www.aemiot.com/wp-content/uploads/2013/02/20130227001149.jpg)](http://www.aemiot.com/wp-content/uploads/2013/02/20130227001149.jpg)

四则运算与括号的关系：

| 表达式位置-优先级最低符号 | 加    | 减    | 乘    | 除    |
|:-------------------------:|:-----:|:-----:|:-----:|:-----:|
| 前表达式-加               | N     | N     | Y     | Y     |
| 前表达式-减               | N     | N     | Y     | Y     |
| 前表达式-乘               | N     | N     | N     | N     |
| 前表达式-除               | N     | N     | N     | N     |
| 后表达式-加               | N     | Y     | Y     | Y     |
| 后表达式-减               | N     | Y     | Y     | Y     |
| 后表达式-乘               | N     | N     | N     | Y     |
| 后表达式-除               | N     | N     | N     | Y     |

## 我的代码

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/*寻找下标为n的右括号对应的左括号的下标，如果出现错误返回一个负数*/
int RguoteL(char* str,int n)
{
    int c = 1,i = n,t;
    if(str[n]!=')') return -1;
    while(t=str[--i],i+1)
    {
        if(t==')') ++c;
        if(t=='(') --c;
        if(c==0) return i;
    }
    return -2;
}
/*返回表达式中优先级最低的符号下标，如果找不到运算符就返回-1*/
int FindC(char* str)
{
    int length;
    int i;
    int k = -1;
    length = strlen(str);
    i = length - 1;
    while(i>=0)
    {
        if(str[i]==')') i = RguoteL(str,i);
        if(str[i]=='+'||str[i]=='-') k = i;
        if( (k!='+'&&k!='-') && (str[i]=='*'||str[i]=='/') ) k = i;
        --i;
    }
    return k;
}
/*在两端添加圆括号*/
void Addguote(char* str)
{
    int i,length = strlen(str);
    str[length+2]='\0';
    str[length+1]=')';
    for(i=length;i>0;--i)
        str[i] = str[i-1];
    str[0] = '(';
}
/*整理表达式并返回整理后的表达式,形参表第二个参数指向该表达式优先级最低的符号*/
char GuoteCal(char* str,char* res)
{
    int k=FindC(str),length=strlen(str);
    char *p=(char*)malloc(sizeof(char)*length),t[2];
    char operL,operR,operNow=' ';
    if(k>=0) operNow = str[k];
    if(RguoteL(str,length-1)==0&&str[length-1]==')') /*被一对括号包含*/
    {
        strncpy(p,str+1,length-2);
        p[length-2] = '\0';
        operNow = GuoteCal(p,res); /*最外层括号应该被忽略，因此它的最低级运算符就是内部表达式的最低级运算符*/
        free(p);
        return operNow;
    }
    if(length==1) /*单变量*/
    {
        strncpy(p,str,1);
        p[length] = '\0';
        strcpy(res,p);
        free(p);
        return operNow;
    }
    /*找了到一个最低运算符号*/
    char *pL=(char*)malloc(sizeof(char)*(k+1)),
         *pR=(char*)malloc(sizeof(char)*(length-k));
    strncpy(pL,str,k);
    pL[k]='\0';
    strncpy(pR,str+k+1,length-k-1);
    pR[length-k-1]='\0';
    /*获取左右符号*/
    operL = GuoteCal(pL,pL);
    operR = GuoteCal(pR,pR);
    /*根据四则运算判断是否需要括号*/
    if((operNow=='-'||operNow=='*')&&(operR=='+'||operR=='-')) Addguote(pR);
    if((operNow=='*'||operNow=='/')&&(operL=='+'||operL=='-')) Addguote(pL);
    if(operNow=='/'&&operR!=' ') Addguote(pR);
    /*组合整理后的表达式*/
    strcpy(p,pL);
    t[0]=operNow;t[1]='\0';
    strcat(p,t);
    strcat(p,pR);
    strcpy(res,p);
    /*释放动态内存*/
    free(p);
    free(pL);
    free(pR);
    return operNow;
}

int main(void)
{
    char str[]="(((a+b)*f)-(i/j))",s[100];
    GuoteCal(str,s);
    printf("%s",s);
    return 0;
}
```