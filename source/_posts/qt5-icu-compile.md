title: '通过重编译icu替换icuxxxx.dll减少QT5发布体积'
tags:
  - icu
id: 1393
categories:
  - QT
date: 2014-08-30 03:08:10
---

## 应用背景

最近需要使用QT写一个程序，选择了比较新的QT5.3以获得更多特性进行快速开发。但是QTCreator编译出来的程序是动态链接版本，接着就头痛了，自己程序很小，但是所需要携带的动态链接库却很大，即使是发布版本，依然需要携带一个庞大的qt库，那么如何减小发布体积呢？（ps：开源版本的qt，其lib文件夹下的\*.lib文件不是真正意义上的静态库文件，其装载的是dll的入口地址等信息）

大概有这么两种方案：

1. 重编译qt生成真正的静态链接版本，然后以静态链接方式生成程序。（这方案理论效果应该是最好的，毕竟在编译前，还可以去掉qt中你用不到的模块，可惜静态编译过程复杂，而且容易出错）

2. 继续使用动态版本，但是想办法重编译qt或者部分qt，通过裁减qt减小依赖的dll体积。

这里决定从第二种方案入手。

<!--more-->

观察一个普通的qt5桌面工程，依赖下列DLL文件：

其中Qt5开头的dll，平均每个占用4M，而且看样子是不重编译qt是无法减小其体积的。（通过重编译qt，可以去除对icu等模块的依赖，从根本上解决问题，不过过程比较复杂，需要对qt各个模块的关系有所了解，暂时不说）。

但是，另一方面，可以发现其中第三方库的icudt52.dll高达22M，其实这是一个国际化资源库，携带了各种语言信息等。其中大多数你都是用不到的。（貌似qt4不需要这几个dll文件）也就是说，如果能够重订制icu库，去除不需要的资源，就能大幅减少发布程序依赖DLL体积。（如果你的项目使用了Qt5Webkit，不推荐裁减ICU库。）

## 裁剪过程

ICU库本身就提供了定制的服务，因此非常方便。

### 安装编译环境

安装[MSYS](http://sourceforge.net/projects/mingw/files/MSYS/Base/msys-core/msys-1.0.11/MSYS-1.0.11.exe/download?use_mirror=garr.  "http://sourceforge.net/projects/mingw/files/MSYS/Base/msys-core/msys-1.0.11/MSYS-1.0.11.exe/download?use_mirror=garr. ")

可用的MinGW编译器，直接用Qt自带的MinGW. 

### 下载&编译ICU

下载[ICU源码](http://site.icu-project.org/download  "http://site.icu-project.org/download ")，接着打开MSYS Shell执行（这里注意ICU版本，这里使用的是**52l**） 

```
$ cd icu/source 
$ export PATH=/C/Qt/Qt5.1.0/Tools/mingw48_32/bin:$PATH 
$ ./runConfigureICU MinGW –prefix=$PWD/../install 
```

到[ICU定制数据页面](http://apps.icu-project.org/datacustom/ICUData50.html  "http://apps.icu-project.org/datacustom/ICUData50.html ")把不需要的选项点掉后，点“Get Data Library”按钮下载定制的文件icudt52l.dat，接着把icudt52l.dat文件复制到icu/source/data/in目录内。 

接着执行： 

```
$ make 
$ make install 
```

全部完成后dll文件可以在icu/install/bin目录里找到。

## 参考文章

- [Windows MinGW下编译ICU，用于替换Qt 5.1.0(MinGW 4.8)自带的DLL](http://www.hellprototypes.com/archives/161#postcomment "http://www.hellprototypes.com/archives/161#postcomment")
- [Compiling ICU with MinGW](http://qt-project.org/wiki/Compiling-ICU-with-MinGW "http://qt-project.org/wiki/Compiling-ICU-with-MinGW")
