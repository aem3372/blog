title: 'styles.xml中命名空间的问题'
id: 1502
categories:
  - android
date: 2015-05-10 00:04:18
---

## 背景

因为最近决定重构下自己的代码，所以除了拓展基本UI控件的功能外，补充了UI新增属性的xml配置方式。首先是在attrs.xml中配置的属性，在layout.xml中能以命名空间的方式去访问，接着试图给自己的控件添加几种style。但是问题就是在style.xml中想像在layout一样用命名空间访问属性，却在编译时收到了Error。

## 状况描述

在styles.xml中使用带有自定义命名空间的属性被报告Error.

<!--more-->

res/values/attrs.xml

    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <declare-styleable name="EditTextPlus">
            <attr name="deleteButton" format="reference" />
        </declare-styleable>
    </resources>


res/layout/main_layout.xml

    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout 
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/RelativeLayout1"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_alignParentLeft="true" >

        <com.aemiot.test.View.EditTextPlus
            android:id="@+id/editText1"
            style="@style/EditTextPlus"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentTop="true"
            android:layout_centerHorizontal="true"
            android:layout_marginTop="162dp"
            android:ems="10"
            android:singleLine="true" />

        <com.aemiot.test.View.EditTextPlus
            android:id="@+id/editText2"
            style="@style/EditTextPlusNight"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignLeft="@+id/editText1"
            android:layout_centerVertical="true"
            android:ems="10"
            android:singleLine="true" />

    </RelativeLayout>

res/values/styles.xml

    <resources 
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:aem="http://schemas.android.com/apk/res/com.aemiot.test" >

        <!-- EditTextPlus -->
        <style name="EditTextPlus">
            <!-- 如果这里试图用aem:deleteButton将会报告Error: No resource found that matches the given name: attr 'aem:deleteButton'. -->
            <item name="deleteButton">@drawable/editviewplus_deletebutton</item>
            <item name="android:background">@drawable/editviewplus_background</item>
            <item name="android:textColor">@android:color/black</item>
        </style>
        
        <style name="EditTextPlusNight">
            <item name="deleteButton">@drawable/editviewplus_deletebutton_night</item>
            <item name="android:background">@drawable/editviewplus_background_night</item>    
            <item name="android:textColor">@android:color/white</item> 
        </style>
        
    </resources>


## 解决办法

这个问题的具体原因不详，从表面现象上看这里是不支持自己定义的命名空间。那就只好不用命名空间，但是这里又不像在layout里一样不带命名空间的任意一个属性名字都可以被传入，它会在指定包中的values/attrs.xml中寻找属性，如果找不到将会报告一个错误。默认是当前程序包，你可以使用com.aemiot.test:deleteButton来显式指定包名。

## 参考文章

- [如何在android style文件中使用自定义属性](http://blog.csdn.net/zhufuing/article/details/41395219 "http://blog.csdn.net/zhufuing/article/details/41395219")
