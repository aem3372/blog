title: cocos2dx-3.0版本项目创建
tags:
  - cocos2dx
  - 工程创建
id: 1368
categories:
  - 游戏开发
date: 2014-06-06 15:54:40
---

自3.0发布以来，多次更新了项目创建的方式。网上的教程也很混乱，作为自己尝试了数次才搞定的事情，决定把过程记录下来。

<!-- more -->

首先，要注意的是版本问题。
打开cocos2dx根目录下找到CHANGELOG用记事本打开，可以看到版本更新的记录。
大致发布顺序是：
cocos2d-x-3.0alpha0
cocos2d-x-3.0alpha1
cocos2d-x-3.0beta
cocos2d-x-3.0beta2
**cocos2d-x-3.0rc0
cocos2d-x-3.0rc1
cocos2d-x-3.0rc2
cocos2d-x-3.0 Apr.23 2014**

现在介绍的是较新的方式，应该也是最后确定的创建工程方式。（版本至少要在**3.0rc0之后**）
使用控制台python脚本方式创建。

优点：跨平台性强，自由。

1. 安装Python，并将其目录加入环境变量中（据说3.0以后可能会有问题，推荐使用2.7）。

2. 将 **[cocos-root]/tools/cocos2d-console/bin/ **加入环境变量的系统路径中。
      这时，打开终端/命令行允许**cocos**命令，应该可以看到使用方法。

3. 创建工程，我们可以使用cocos new命令。


    cocos new [-h] [-p PACKAGE_NAME] -l {cpp,lua,js} [-d DIRECTORY]
              [-t TEMPLATE_NAME] [--no-native]
              [PROJECT_NAME]

例如我们要在当前目录创建一个名为test的c++项目，应该执行指令**cocos new -l cpp test**