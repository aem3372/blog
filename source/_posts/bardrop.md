title: '是否记得俄罗斯方块呢|方块下落问题'
tags:
  - 九宫格
  - 俄罗斯方块
  - 方块下落
id: 694
categories:
  - 算法/数据结构
date: 2013-02-28 23:04:14
---

## 题目名称

方块下落

## 题目描述

有红（R）、绿（G）、蓝（B）、黑（A）、白（W）5种颜色的方块放在一个M*N（M,N<50）的方框中，现要求消去同色相连大于3的所有方块。消去过程为：一次同时消去同一直线（横、竖、斜线）同色相连大于等于3的块。在消去过程中，同一方块可在不同方向上重复使用。方块消去后，上面的块自动下落，重复消去过程，直至不能消去为止。（呵呵，这不就是俄罗斯方块的消除过程么。）

<!-- more -->

## 输入输出格式

第一行为M N，接下来是M*N 的矩阵。例如，设有5*5的方框矩阵分布如下：

则输入序列为：
5 5
RRRAA
WRWAW
WWBAW
WWBWA
BBWWA

输出序列为:
A
W
A
RBWA

## 思路

模拟方块消去过程。一次消去过程可以分为三个阶段完成：**标记**、**消除**、**下落**。之所以将要先标记再消除是因为一个块可以在不同方向上重复使用，如果直接消去，就无法得到正确结果。因为方块落下后，可能形成了新的可消去的方块，因此要**反复以上三步**，直到没有可消除的方块为止，就可以得到正确结果。

## 算法描述

1. 读入数据
2. 模拟消去过程-循环
  - 标记, 如果没有元素被标记，跳出循环执行3模拟消去过程结束，进入下一环节
  - 消去已标记方块
  - 下落
  - 循环2
3. 输出最终方块排列方案
4. 结束

[![20130228223225](http://www.aemiot.com/wp-content/uploads/2013/02/20130228223225.jpg)](http://www.aemiot.com/wp-content/uploads/2013/02/20130228223225.jpg)


标记元素，这里提出**两种方案**处理。第一种方案，先按行遍历二维数组标记，然后按列遍历二维数组标记，最后斜向遍历二维数组标记。如果采用第一种方案，那么最麻烦的地方就在**两次斜向遍历二维数组**。因此，我采用了第二种方案，设置一个九宫格，遍历二维数组的每一个元素，将一个元素写入九宫格**中央**，然后补全九宫格，这样只需判断九宫格以中心元素的4条直线的情况就能完成一个元素周围的标记（这种就是要注意九宫格的**边角问题**，因为元素的某个方向上可能没有元素）。

## 我的代码

    #include <stdio.h>
    #define MAX 100
    /*标记,如果有标记方块，返回真，否则返回假*/
    int SignBar(char aj[ ][MAX],char isSign[ ][MAX],int m,int n)
    {
        int i,j,flag=0;
        /*预处理（初始化 isSign）*/
        for(i=0; i<m; ++i)
            for(j=0; j<n; ++j)
                isSign[i][j] = 0;
        /*处理开始*/
        for(i=0;i<m;++i)
            for(j=0;j<n;++j)
            {
                char t[3][3]={0};
                unsigned short contral[4]={0}; /*分别控制上下左右*/
                /*获取控制信息*/
                if(i==0) contral[0]=1;
                if(i==m-1) contral[1]=1;
                if(j==0) contral[2]=1;
                if(j==n-1) contral[3]=1;
                t[1][1] = aj[i][j];
                /*构造九宫格*/
                if(!contral[0]) t[0][1] = aj[i-1][j]; /*上*/
                if(!contral[1]) t[2][1] = aj[i+1][j]; /*下*/
                if(!contral[2]) t[1][0] = aj[i][j-1]; /*左*/
                if(!contral[3]) t[1][2] = aj[i][j+1]; /*右*/

                if(!contral[0]&&!contral[2])/*左上*/
                    t[0][0] = aj[i-1][j-1];
                if(!contral[0]&&!contral[3])/*右上*/
                    t[0][2] = aj[i-1][j+1];
                if(!contral[1]&&!contral[2])/*左下*/
                    t[2][0] = aj[i+1][j-1];
                if(!contral[1]&&!contral[3])/*右下*/
                    t[2][2] = aj[i+1][j+1];
                /*九宫格行处理*/
                if(t[1][0]!=0&&t[1][0]==t[1][1]&&t[1][1]==t[1][2])
                    isSign[i][j-1] = isSign[i][j] = isSign[i][j+1] = 1,flag=1;
                /*九宫格列处理*/
                if(t[0][1]!=0&&t[0][1]==t[1][1]&&t[1][1]==t[2][1])
                    isSign[i-1][j] = isSign[i][j] = isSign[i+1][j] = 1,flag=1;
                /*斜线处理*/
                if(t[0][0]!=0&&t[0][0]==t[1][1]&&t[1][1]==t[2][2])
                    isSign[i-1][j-1] = isSign[i][j] = isSign[i+1][j+1] = 1,flag=1;
                if(t[0][2]!=0&&t[0][2]==t[1][1]&&t[1][1]==t[2][0])
                    isSign[i-1][j+1] = isSign[i][j] = isSign[i+1][j-1] = 1,flag=1;
            }
        return flag;
    }
    /*消除*/
    void ClearBar(char aj[][MAX],char isSign[][MAX],int m,int n)
    {
        int i,j;
        for(i=0;i<m;++i)
            for(j=0;j<n;++j)
                if(isSign[i][j]==1) aj[i][j]='\0';
    }
    /*下落*/
    void DropBar(char aj[][MAX],char isSign[][MAX],int m,int n)
    {
        int i,j,x,t,flag;
        for(j=0;j<n;++j)
        {
            x=m-1;
            flag=1;
            while(flag&&x>=0)
            {
                if(aj[x][j]=='\0')
                {
                    int z;
                    flag=0;
                    for(z=x;z>0;--z)
                        if(aj[z][j]!='\0')
                        {
                            flag=1;
                            for(t=x;t>0;--t)
                                aj[t][j] = aj[t-1][j];
                            aj[0][j] = '\0';
                            ++x; /*使得x保持原位，再次检测这里是否还有可落下方块*/
                            break;
                        }
                }
              --x;
             }
        }
    }

    int main(void)
    {
        char arr[MAX][MAX] = {0},
                bd[MAX][MAX];
        int i,j,z,m,n;
        scanf("%d%d",&m,&n);
        getchar();/*去除行末空格*/
        for(i=0;i<m;++i)
        {
            for(j=0;j<n;++j)
                arr[i][j] = getchar();
            getchar(); /*去除行末回车*/
        }
        while(SignBar(arr,bd,m,n))
        {
        ClearBar(arr,bd,m,n);
        DropBar(arr,bd,m,n);
        }
        for( i=0; i<m; ++i)
        {
            for(j=0; j<n; ++j)
                printf("%c",arr[i][j]);
            printf("\n");
        }
        return 0;
    }
