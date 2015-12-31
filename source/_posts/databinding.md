title: Android Databinding Library 学习笔记
date: 2016-01-01 01:41:13
categories:
  - android
tags:
  - binding
---

## DataBinding

DataBinding Library提供了一套数据绑定机制，配合Android Gradle插件可以根据Layout自动生成相关类。

DataBinding解决了操纵UI的繁琐，同时为支持MVVM模式迈出了一步。以前操纵的是View控件，现在操纵的是ViewModel，关注点的转移对单元测试也更加友好了。

> DataBinding Demo Set - [https://github.com/aem3372/DataBindingDemo](https://github.com/aem3372/DataBindingDemo "DataBinding相关源码")

<!--more-->

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

> Demo：[SimpleActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/SimpleActivity.java "SimpleActivity")

## 更灵活的Layout

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

> Demo：[ExpressionActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/ExpressionActivity.java "ExpressionActivity")

## 更多的类型

如果要使用更多类型，需要在声明变量前使用`import`标签导入（java.lang中的类已经默认导入），这其中包括自定义的类型。

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

> Demo：[MutilateTypeActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/MutilateTypeActivity.java "MutilateTypeActivity")

## 自定义Binding类名字
 
如果想要自定义Binding类的名字，而不是根据默认规则生成，在`data`标签上通过`class`属性指定即可。

```xml
<data class="CustomClassNameBinding">
	<!-- 声明变量 -->
</data>
```

> Demo：[CustomBindingNameActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/CustomBindingNameActivity.java "CustomBindingNameActivity")

## 事件绑定

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
> Demo：[EventBindingActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/EventBindingActivity.java "EventBindingActivity")

## 属性Setter

对于值为`@{}`格式的控件属性默认会寻找控件中对应的setter方法（这时会忽略xml中的命名空间，但要求值的类型和setter方法中参数一致）。

新建`CustomView`，以便观察属性是否设置成功。

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

在Layout文件中使用`CustomView`，并设置属性`ui:customAttribute`。

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

正是这样的方案使得DataBinding Library向前兼容。但是android命名空间下的属性仍然有一些没有对应的setter方法，因此存在一组adapters来为各个控件指定这些属性对应的方法名。

> Demo：[AttributeSetterActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/AttributeSetterActivity.java "AttributeSetterActivity")

## 动态绑定

DataBinding类支持在运行时和视图实例进行绑定，只需要调用它的`bind`方法即可。这一点正好配合`RecyclerView`可以完成很多灵活的布局。

`RecyclerAdapter`可以按如下方式实现，其中对于`ViewHolder`来说只需要持有Binding类就可以了。

```java
public class RecyclerAdapter extends RecyclerView.Adapter<RecyclerAdapter.ViewHolder> {

    int[] datas = {0,1,2,3,4,5,6};

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.recycler_item, parent, false);
        return new ViewHolder(itemView);
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        holder.bind(datas[position]);
    }

    @Override
    public int getItemCount() {
        return datas.length;
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        private RecyclerItemBinding binding;

        public ViewHolder(View itemView) {
            super(itemView);
            binding = RecyclerItemBinding.bind(itemView);
        }

        public void bind(int data) {
            binding.setData(data);
        }
    }
}
```

> Demo：[RecyclerViewActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/RecyclerViewActivity.java "RecyclerViewActivity")

## 绑定通知

之前的绑定，只是简单的将值绑定到了视图上，当变量值发生变化时，视图并不会更新，除非你将重新绑定新值到视图。如果需要在变量值发生变化时自动通知视图更新，需要实现了`Observable`接口的类型。基本类型都有与其对应实现了`Observable`接口的类型，复杂类型可以使用`ObservableField`的泛化类型，同时还有`ObservableArrayList`、`ObservableArrayMap`来提供集合支持。常见的类型：

| 基本类型     | Observable类型             |
|:------------ |:-------------------------- |
| `boolean`    | `ObservableBoolean`        |
| `int`        | `ObservableInt`            |
| `String`     | `ObservableField<String>`  |
| `List<T>`    | `ObservableArrayList<T>`   |
| `Map<K,V>`   | `ObservableArrayMap<K,V>`  |

对于自定义类型，想要具备这样功能，需要实现`Observable`接口。不过为了简化实现，Library同时提供了一个`BaseObservable`，将其作为基类，这样你只需要为希望被通知的变量或它的getter方法设置`@Bindable`注解，以及在setter方法中额外调用`notifyChange`方法即可。

```java
public class CustomObservableType extends BaseObservable {

    @Bindable
    private String value;

    public String getValue() {
        return value;
    }

    public void setValue(String value) {
        if(this.value != value) {
            this.value = value;
            notifyChange();
        }
    }
}
```

> Demo：[ObservableTypeActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/ObservableTypeActivity.java "ObservableTypeActivity")

## 获取控件

如果仍然需要像以前一样操纵控件，那么你仍然可以通过id来直接访问控件（不推荐），但是通过Binding类不再需要编写大量`findViewById`方法了，Layout中所有具有id的控件将会成为Binding类中的public field，你可以直接访问它们。

例如，`binding.tip.setText("Hello! DataBinding Id.");`。

> Demo：[IdsActivity](https://github.com/aem3372/DataBindingDemo/blob/master/app/src/main/java/com/aemiot/demo/databinding/activity/IdsActivity.java "IdsActivity")

## 参考文章

- [Data Binding Guide](https://developer.android.com/tools/data-binding/guide.html "Data Binding Guide")
- [MasteringAndroidDataBinding](https://github.com/LyndonChin/MasteringAndroidDataBinding "MasteringAndroidDataBinding")
