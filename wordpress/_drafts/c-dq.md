title: C语言小技巧，调整结构体对齐方式简化数据读取
id: 641
categories:
  - 未分类
tags:
---

C语言中默认对齐方式：偏移量必须是自身对齐值的倍数。其中内置类型的自身对齐值是它的大小，结构体的自身对齐值等于结构体内自身对齐值最大的成员的自身对齐值。
[code lang="cpp"]
#include &lt;stdio.h&gt;

struct d{
double r;
int s;
};
/* size = 16 */

struct v{
    char x;
    struct d g;
};
/* size = 24 */

struct x{
    char x;
    struct v g;
};
/* size = 32 */

[/code]