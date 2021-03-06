title: 'Graphivz中文乱码的解决过程'
tags:
  - Graphivz
  - 中文乱码
id: 1279
categories:
  - 其他
date: 2014-03-05 22:56:28
---

我在Windows上装了Graphivz，当我将中文输入进Dot文件中，并运行指令`dot grep.dot -Tpng -o grep.png`

试图生成例图时发现中文无法正常显示。
于是首先就想到了Windows对新建的文本文档保存默认是用ANSI(GBK)编码的，并且给出了警告`Warning: Invalid 2-byte UTF8 found in input of graph G - treated as Latin-1\. Perhaps "-Gcharset=latin1" is needed?`

于是，我试图将文档编码改为UTF8，于是我用notepad.exe打开文档另存为，然后编码选择UTF8，重新运行指令，得到如下警告`Warning: grep.dot: syntax error in line 1 near '锘縟igraph'。`

<!--more-->

接着我在互联网上查找资料，不少资料是说需要将文档用UTF8保存，接着设置顶点和边的文字描述字体为中文字体，即：

```
edge [fontname="FangSong"];
node [shape=box, fontname="SimSun" size="20,20"];
```

或者
或者用命令行参数`-Nfontname="Adobe Kaiti Std"`（字体名可以用`fc-list`命令得到的任意字体名，也可以指定字体路径。）
    
    

接着我加上了上述语句，依然失败，而且之前错误提示那两个乱码的字符覆盖了原本的d，这个很让我在意，接着我看到了一篇文章，作者说自己用Java生成的UTF8文本文档就可以正确使用中文，而新建文本文档写的代码就不可以。这时候我想到了之前一直没有在意的一个细节，**BOM（Byte order Mark）**，它会导致文档前多3个字节（这三个字节是微软擅自加上去的），那么如果dot解析不知道多了这3个字节，那么就会连带d字符一起解释为2个字符，那么这也就可以解释之前的警告了。想到这里，我立刻将编码改成了无BOM的标准UTF8文档，执行指令，这次没有给出任何警告，并且成功得到了含有中文的图。

```
digraph G {
    edge [fontname="simsun"];
    node [fontname="simsun"];
    "编码"->"GBK";
    "编码"->"UTF-8";
    "编码"->"UTF-8(BOM)";
}
```

不过至今有一个比较麻烦的事情就是，无法很好的在输出ps格式的时候保持中文，貌似和ps格式本身有关。
在linux下可以用cairo引擎来处理`dot -Tps:cairo chs.dot > chs.ps`

此外，输出pdf也是个不错的方案。


## 参考文章

- [UTF-8文件编码格式中有无签名问题汇总](http://blog.163.com/li_wangyuan/blog/static/52060062010511151146/ "UTF-8文件编码格式中有无签名问题汇总") 

