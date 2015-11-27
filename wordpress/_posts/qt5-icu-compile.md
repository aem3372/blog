title: '[QT5裁剪]重编译icu|替换icuxxxx.dll|减少qt5发布体积'
tags:
  - icu
  - QT
  - 裁剪
id: 1393
categories:
  - 应用开发
date: 2014-08-30 03:08:10
---

[toggle title="应用背景"]

最近需要使用QT写一个程序，选择了比较新的QT5.3以获得更多特性进行快速开发。但是QTCreator编译出来的程序是动态链接版本，接着就头痛了，自己程序很小，但是所需要携带的动态链接库却很大，即使是发布版本，依然需要携带一个庞大的qt库，那么如何减小发布体积呢？（ps：开源版本的qt，其lib文件夹下的*.lib文件不是真正意义上的静态库文件，其装载的是dll的入口地址等信息）

大概有这么两种方案：

1.重编译qt生成真正的静态链接版本，然后以静态链接方式生成程序。（这方案理论效果应该是最好的，毕竟在编译前，还可以去掉qt中你用不到的模块，可惜静态编译过程复杂，而且容易出错）

2.继续使用动态版本，但是想办法重编译qt或者部分qt，通过裁减qt减小依赖的dll体积。

<p>这里决定从第二种方案入手。

观察一个普通的qt5桌面工程，依赖下列DLL文件：

其中Qt5开头的dll，平均每个占用4M，而且看样子是不重编译qt是无法减小其体积的。（通过重编译qt，可以去除对icu等模块的依赖，从根本上解决问题，不过过程比较复杂，需要对qt各个模块的关系有所了解，暂时不说）。

但是，另一方面，可以发现其中第三方库的icudt52.dll高达22M，其实这是一个国际化资源库，携带了各种语言信息等。其中大多数你都是用不到的。（貌似qt4不需要这几个dll文件）也就是说，如果能够重订制icu库，去除不需要的资源，就能大幅减少发布程序依赖DLL体积。

[/toggle]

[note]
如果你的项目使用了Qt5Webkit，不推荐裁减ICU库。
[/note]

ICU库本身就提供了定制的服务，因此非常方便。

1： 安装编译环境

     (1) 安装MSYS： [http://sourceforge.net/projects/mingw/files/MSYS/Base/msys-core/msys-1.0.11/MSYS-1.0.11.exe/download?use_mirror=garr. ](http://sourceforge.net/projects/mingw/files/MSYS/Base/msys-core/msys-1.0.11/MSYS-1.0.11.exe/download?use_mirror=garr.  "http://sourceforge.net/projects/mingw/files/MSYS/Base/msys-core/msys-1.0.11/MSYS-1.0.11.exe/download?use_mirror=garr. ")

     (2)可用的MinGW编译器，直接用Qt自带的MinGW. 

2：下载ICU源码： [http://site.icu-project.org/download ](http://site.icu-project.org/download  "http://site.icu-project.org/download ")

3：打开MSYS Shell:（这里注意ICU版本，这里使用的是**52l**） 

[code lang="shell"]
    $ cd icu/source 
    $ export PATH=/C/Qt/Qt5.1.0/Tools/mingw48_32/bin:$PATH 
    $ ./runConfigureICU MinGW –prefix=$PWD/../install 
[/code]

4：到ICU网站定制数据文件：[http://apps.icu-project.org/datacustom/ICUData50.html ](http://apps.icu-project.org/datacustom/ICUData50.html  "http://apps.icu-project.org/datacustom/ICUData50.html ")

    把不需要的选项点掉后，点“Get Data Library”按钮下载定制的文件：“icudt52l.dat” 

5：把“icudt52l.dat”文件复制到： **icu/source/data/in** 目录内。 

6：MSYS Shell 再执行： 

[code lang="shell"]
    $ make 
    $ make install 
[/code]

7：全部完成后dll文件可以在icu/install/bin目录里找到。

[note title="参考文章"]
1\. [Windows MinGW下编译ICU，用于替换Qt 5.1.0(MinGW 4.8)自带的DLL](http://www.hellprototypes.com/archives/161#postcomment "http://www.hellprototypes.com/archives/161#postcomment")
2\. [Compiling ICU with MinGW](http://qt-project.org/wiki/Compiling-ICU-with-MinGW "http://qt-project.org/wiki/Compiling-ICU-with-MinGW")
[/note]