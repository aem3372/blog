title: 'styles.xml中命名空间的问题|Android日常'
id: 1502
categories:
  - 应用开发
date: 2015-05-10 00:04:18
tags:
---

[toggle title="背景"]

因为最近决定重构下自己的代码，所以除了拓展基本UI控件的功能外，补充了UI新增属性的xml配置方式。首先是在attrs.xml中配置的属性，在layout.xml中能以命名空间的方式去访问，接着试图给自己的控件添加几种style。但是问题就是在style.xml中想像在layout一样用命名空间访问属性，却在编译时收到了Error。

[/toggle]

# 状况描述

在styles.xml中使用带有自定义命名空间的属性被报告Error.

[code lang="xml"]
&lt;!-- res/values/attrs.xml --&gt;

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;resources&gt;
    &lt;declare-styleable name=&quot;EditTextPlus&quot;&gt;
        &lt;attr name=&quot;deleteButton&quot; format=&quot;reference&quot; /&gt;
    &lt;/declare-styleable&gt;
&lt;/resources&gt;

[/code]

[code lang="xml"]
&lt;!-- res/layout/main_layout.xml --&gt;

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;RelativeLayout 
    xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot;
    android:id=&quot;@+id/RelativeLayout1&quot;
    android:layout_width=&quot;match_parent&quot;
    android:layout_height=&quot;match_parent&quot;
    android:layout_alignParentLeft=&quot;true&quot; &gt;

    &lt;com.aemiot.test.View.EditTextPlus
        android:id=&quot;@+id/editText1&quot;
        style=&quot;@style/EditTextPlus&quot;
        android:layout_width=&quot;wrap_content&quot;
        android:layout_height=&quot;wrap_content&quot;
        android:layout_alignParentTop=&quot;true&quot;
        android:layout_centerHorizontal=&quot;true&quot;
        android:layout_marginTop=&quot;162dp&quot;
        android:ems=&quot;10&quot;
        android:singleLine=&quot;true&quot; /&gt;

	&lt;com.aemiot.test.View.EditTextPlus
	    android:id=&quot;@+id/editText2&quot;
	    style=&quot;@style/EditTextPlusNight&quot;
	    android:layout_width=&quot;wrap_content&quot;
	    android:layout_height=&quot;wrap_content&quot;
	    android:layout_alignLeft=&quot;@+id/editText1&quot;
	    android:layout_centerVertical=&quot;true&quot;
	    android:ems=&quot;10&quot;
	    android:singleLine=&quot;true&quot; /&gt;

&lt;/RelativeLayout&gt;

[/code]

[code lang="xml"]
&lt;!-- res/values/styles.xml --&gt;

&lt;resources 
    xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot;
    xmlns:aem=&quot;http://schemas.android.com/apk/res/com.aemiot.test&quot; &gt;

    &lt;!-- EditTextPlus --&gt;
    &lt;style name=&quot;EditTextPlus&quot;&gt;
        &lt;!-- 如果这里试图用aem:deleteButton将会报告Error: No resource found that matches the given name: attr 'aem:deleteButton'. --&gt;
        &lt;item name=&quot;deleteButton&quot;&gt;@drawable/editviewplus_deletebutton&lt;/item&gt;
    	&lt;item name=&quot;android:background&quot;&gt;@drawable/editviewplus_background&lt;/item&gt;
    	&lt;item name=&quot;android:textColor&quot;&gt;@android:color/black&lt;/item&gt;
    &lt;/style&gt;

    &lt;style name=&quot;EditTextPlusNight&quot;&gt;
        &lt;item name=&quot;deleteButton&quot;&gt;@drawable/editviewplus_deletebutton_night&lt;/item&gt;
    	&lt;item name=&quot;android:background&quot;&gt;@drawable/editviewplus_background_night&lt;/item&gt;    
    	&lt;item name=&quot;android:textColor&quot;&gt;@android:color/white&lt;/item&gt; 
    &lt;/style&gt;

&lt;/resources&gt;

[/code]

# 解决办法

这个问题的具体原因不详，从表面现象上看这里是不支持自己定义的命名空间。那就只好不用命名空间，但是这里又不像在layout里一样不带命名空间的任意一个属性名字都可以被传入，它会在指定包中的values/attrs.xml中寻找属性，如果找不到将会报告一个错误。默认是当前程序包，你可以使用com.aemiot.test:deleteButton来显式指定包名。

[note title="参考文章"]
[如何在android style文件中使用自定义属性](http://blog.csdn.net/zhufuing/article/details/41395219 "http://blog.csdn.net/zhufuing/article/details/41395219")
[/note]