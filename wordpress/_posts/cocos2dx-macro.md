title: cocos2dx引擎学习笔记-自带宏及内部实现（持续更新）
id: 1581
categories:
  - 游戏开发
date: 2015-07-04 16:33:05
tags:
---

[note]
本文以Cocos2dx-3.2为探究。
[/note]

# 禁止复制

[code lang="cpp"]
//CCPlatformMacros.h
//...
// A macro to disallow the copy constructor and operator= functions&lt;
// This should be used in the private: declarations for a class
#if defined(__GNUC__) &amp;&amp; ((__GNUC__ &gt;= 5) || ((__GNUG__ == 4) &amp;&amp; (__GNUC_MINOR__ &gt;= 4))) \
	|| (defined(__clang__) &amp;&amp; (__clang_major__ &gt;= 3)) || (_MSC_VER &gt;= 1800)
#define CC_DISALLOW_COPY_AND_ASSIGN(TypeName) \
    TypeName(const TypeName &amp;) = delete; \
    TypeName &amp;operator =(const TypeName &amp;) = delete;
#else
#define CC_DISALLOW_COPY_AND_ASSIGN(TypeName) \
    TypeName(const TypeName &amp;); \
    TypeName &amp;operator =(const TypeName &amp;);
#endif
[/code]

[code lang="cpp"]
//ccMacros.h
//...
#define DISALLOW_COPY_AND_ASSIGN(TypeName) \
            TypeName(const TypeName&amp;);\
            void operator=(const TypeName&amp;)
[/code]

在Cocos2dx中能找到2个关于禁止复制的宏(至于为什么有两个，因为3.0开始大量去除名称前缀CC，不过为什么去CC后的版本为什么不考虑C++11的特性就不知道了，或许只是暂时没有加上)。

禁止复制的原理挺简单的，支持C++11的平台可以使用C++11删除默认函数的特性来将复制构造函数和赋值操作符函数移除，如果不支持则将作为私有函数只声明不定义（C++Primer上提到过），从而引发编译器报错。

在code style文档中写道，如果需要复制的话，尽可能提供一个类似clone()的函数，而不是使用复制构造和赋值操作符函数，事实上引擎已经提供了相关接口。如果不需要复制，就更应该禁止复制。同时推荐在STL容器中存储指针而不是对象。（这么做的很大一部分原因是，cocos2dx引入了垃圾清除机制而大量使用指针）