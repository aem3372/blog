title: Android Databinding Library 学习笔记(一)
date: 2015-12-26 21:02:25
tags:
---

## DataBinding

DataBinding Library提供了一套数据绑定机制，配合Android Gradle插件可以根据Layout自动生成相关类。

DataBinding解决了操纵UI的繁琐，同时为支持MVVM模式迈出了一步。以前操纵的是View控件，现在操纵的是ViewModel，关注点的转移对单元测试也更加友好了。

> DataBinding Demo - [https://github.com/aem3372/DataBindingDemo](https://github.com/aem3372/DataBindingDemo "DataBinding相关源码")

## 配置DataBinding

确保Android Gradle插件版本高于1.5.0。使用Android Studio新建工程的话，插件配置在{Project}/build.gradle。

```xml
dependencies {
    classpath 'com.android.tools.build:gradle:1.5.0'
}
```

为相应模块开启dataBinding，配置{module}/build.gradle。

```xml
android {
	<!-- 其他配置 -->

	dataBinding {
        enabled = true
    }
}
```

## 新的变化

新的布局格式：

```xml
<layout>
	<data>
		<!-- 声明变量 -->
		<!-- <variable /> -->
	</data>
	<!-- 原来布局的根节点（Root Element）-->
	<!-- 在布局内，可以用使用 @{name} 访问data中定义的变量-->
	<LinearLayout>
	
	</LinearLayout>
</layout>
```

对于这种格式的布局，随后Android Gradle插件会为其在.databinding包下生成一个Binding类，这个类默认名字是根据布局文件名生成的，例如activity_simple.xml会生成`ActivitySimpleBinding`。
在variable中定义的变量，最终会被定义为自动生成的Binding类中的field，并提供相应的getter/setter。

与此同时，函数库提供了一个`DataBindingUtil`的工具类，里面提供了实例化布局同时获取Binding类实例的方法，比如`setContentView`和`inflate`。

## 初次尝试

Demo：SimpleActivity。
新建一个layout，命名为activity_simple.xml。布局内声明`String`类型的`tip`变量，`TextView`直接根据`tip`显示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable name="tip" type="String" />
    </data>

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin"
        tools:context=".activity.SimpleActivity">

        <TextView
            android:text="@{tip}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />
    </FrameLayout>
</layout>
```

新建`SimpleActivity`并在Manifest中声明。使用`DataBindingUtil.setContentView(Activity, int)`来设置布局并获取Binding类。接着通过Binding类为`tip`变量绑定一个字符串值。

```java
public class SimpleActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivitySimpleBinding binding =
                DataBindingUtil.setContentView(this, R.layout.activity_simple);
        binding.setTip("Hello! DataBinding.");
    }
}
```
## 更灵活的Layout

Demo：ExpressionActivity。
在`@{}`中除了能访问之前声明的变量，还能获取其他xml中定义的属性值以及使用java表达式计算。

例如，

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:padding="@dimen/base_padding"
    android:background="#ccc"
    android:textColor="@android:color/holo_blue_dark"
    android:text="@{String.valueOf(@dimen/base_padding)}" />
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:padding="@{(int)@dimen/base_padding * 2}"
    android:background="#ccc"
    android:textColor="@android:color/holo_orange_dark"
    android:text="@{String.valueOf(@dimen/base_padding * 2)}"/>
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:padding="@{(int)@dimen/base_padding * 3}"
    android:background="#ccc"
    android:textColor="@android:color/primary_text_light"
    android:text="@{String.valueOf(@dimen/base_padding * 3)}"/>
```

注意，属性能接收的类型有限制。


## 更多的类型

Demo：MutilateTypeActivity。
如果要使用更多类型，需要在声明变量前使用import标签导入（java.lang中的类已经默认导入），这其中包括自定义的类型。

```xml
<data>
    <import type="java.util.Date"/>
    <import type="java.util.List"/>
    <import type="com.aemiot.demo.databinding.model.CustomType"/>
    <import type="android.graphics.drawable.Drawable"/>

    <variable name="intValue" type="int"/>
    <variable name="shortValue" type="short"/>
    <variable name="longValue" type="long"/>
    <variable name="floatValue" type="float"/>
    <variable name="doubleValue" type="double"/>
    <variable name="integerClassValue" type="Integer"/>
    <variable name="shortClassValue" type="Short"/>
    <variable name="longClassValue" type="Long"/>
    <variable name="floatClassValue" type="Float"/>
    <variable name="doubleClassValue" type="Double"/>
    <variable name="stringValue" type="String"/>
    <variable name="dateValue" type="Date"/>
    <variable name="listValue" type="List"/>
    <variable name="customValue" type="CustomType"/>
    <variable name="drawable" type="Drawable"/>
</data>
```

## 自定义Binding类名字
 
Demo：CustomBindingNameActivity。
如果想要自定义Binding类的名字，而不是根据默认规则生成，在data标签上通过class属性指定即可。

```xml
<data class="CustomClassNameBinding">
	<!-- 声明变量 -->
</data>
```

## 事件绑定

Demo：EventBindingActivity。
对于click、longClick等事件，可以绑定类方法到属性值上，要求是与事件对应的Listener中的方法参数、返回值需要一致。

```java
public class ButtonHandler {

    public void tip(View v) {
        Toast.makeText(v.getContext(), R.string.click_tip, Toast.LENGTH_SHORT).show();
    }
}

```

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <import type="com.aemiot.demo.databinding.event.ButtonHandler" />

        <variable name="tip" type="String" />
        <variable name="handle" type="ButtonHandler" />
    </data>

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin"
        tools:context=".activity.SimpleActivity">

        <Button
            android:text="@{tip}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="@{handle.tip}" />
    </FrameLayout>
</layout>
```

## 属性Setter

对于值为`@{}`格式的控件属性默认会寻找控件中对应的set方法（这时会忽略xml中的命名空间，但要求值的类型和set方法中参数一致）。


新建CustomView，以便观察属性是否设置成功。

```java
public class CustomView extends TextView {
    
    public CustomView(Context context) {
        super(context);
    }

    public CustomView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public CustomView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    public void setCustomAttribute(String value) {
        setText(value);
    }
}
```

在Layout文件中使用CustomView，并设置属性ui:customAttribute。

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:ui="http://schemas.android.com/apk/res-auto">
    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:paddingBottom="@dimen/activity_vertical_margin"
        android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        tools:context="com.aemiot.demo.databinding.activity.AttributeSetterActivity">
        <com.aemiot.demo.databinding.ui.CustomView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            ui:customAttribute="@{@string/app_name}"/>
    </RelativeLayout>
</layout>
```

正是这样的方案使得DataBinding Library向前兼容。但是android命名空间下的属性仍然有一些没有对应的set方法，因此存在一组adapters来为各个控件指定这些属性对应的方法名。


## 参考文章

- [Data Binding Guide](https://developer.android.com/tools/data-binding/guide.html "Data Binding Guide")
- [MasteringAndroidDataBinding](https://github.com/LyndonChin/MasteringAndroidDataBinding "MasteringAndroidDataBinding")
