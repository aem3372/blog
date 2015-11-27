title: 'TitleBaseActivity组件封装整理笔记|Android日常'
id: 1530
categories:
  - 应用开发
date: 2015-05-17 14:03:39
tags:
---

[note title="TitleBaseActivity布局的方法一"]

**单独制作title_layout.xml，然后在派生的Activity布局开头include该布局，TitleBaseActivity实现提供对Title的控制方法。**

## 注意

title_layout.xml是被动的，由include该布局的Layout XML决定显示它的位置。

## 优点

预览Layout XML时看到的是完整的布局。

## 缺陷

所有派生Activity的Layout XML都与title_layout.xml有较强的耦合，实现对用户不透明，用户替换基类需要修改Layout XML。

[/note]

[note title="TitleBaseActivity布局的方法二"]

**单独制作title_layout.xml，然后重写TitleBaseActivity的setContentView，在该方法中分别填充title_layout.xml和参数指定的布局，接着将两个布局合并返回。**

## 注意

title_layout.xml可以是主动的，也可以是被动的。
如果想将title_layout.xml设计成主动的，设计title_layout.xml时放置具名布局，而此时合并两个布局的方法是将参数指定的布局作为子视图添加到title_layout的具名布局中。
如果想将title_layout.xml设计成被动的，设计title_layout.xml则只包含title相关内容，而此时合并两个布局的方法是新建一个布局，将两个布局分别作为子视图添加到新建的布局中。

## 优点

title_layout.xml将只与TitleBaseActivity有较弱耦合，实现对用户透明，用户替换基类不需要修改Layout XML。

## 缺陷

预览Layout XML时看到的是部分布局。

[/note]

看起来用方案二解决问题更优雅一些。