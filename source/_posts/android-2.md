title: 'Android拓展控件（自定义视图）的方法'
id: 1559
categories:
  - android
date: 2015-05-26 00:09:15
---

## 方案一
从某个控件派生，拓展其功能。使用该类时，在Layout XML中直接使用类名即可。

### 优点

方便替换，在需要父控件的，通常可以换成子控件。
方便提供风格控制。

### 缺点

继承方式的通病，`View`往往职责很多，在不了解内部机制的情况下往往难以控制，甚至不能控制。
持续拓展时，随着多次继承，职责将越来越混乱。

<!-- more -->

### 亲身跳坑

给`EditText`添加右侧删除按钮，利用`setCompoundDrawables`设置右侧删除显示，但是`setCompoundDrawables`是一个`public`方法，并且它还有几个同族方法，如果被外部调用将造成显示异常，一种阻止方法是在本类中显式调用父类的`setCompoundDrawables`，本类中重写`setCompoundDrawables`，在实现中抛出一个`RuntimeException`族的异常。但是缺陷也是显然的，你不了解内部调用过程的话（已知构造时可能被调用），你无法保证父类不会调用`setCompoundDrawables`。如果不幸被父类调用`setCompoundDrawables`，则抛出异常。

给`EditText`添加右侧删除按钮，重写`onTouchEvent`实现父类删除功能，我需要额外相响应的是`ACTION_UP`，如果在这里响应完不调用父类中的方法，在不了解内部的情况下似乎是不合理的，可能出现未知问题，而如果不这么做则无法屏蔽在删除按钮上长按导致的弹窗等问题。事实上可以推测出将`ACTION_DOWN`到`ACTION_UP`之间的整个动作都一起屏蔽通常是不会有问题的。


## 方案二

从某个布局派生，功能写在派生类中，视图由Layout XML控制，其中类通过ID找文件。Layout XML的根节点就是该类。

定义容器

```java
public class MyClass extends LinearLayout{
    /**
     * Finalize inflating a view from XML.  This is called as the last phase
     * of inflation, after all child views have been added.
     *
     * <p>Even if the subclass overrides onFinishInflate, they should always be
     * sure to call the super method, so that we get called.
     */
    /*子视图被添加后才能获得Layout XML中定义的子视图。跟踪框架源码，可以看到
      该方法在XML中所有子视图被添加完成后调用，因此可以在这个方法中获取子视图。
     */
    @Override
    protected void onFinishInflate() {
        super.onFinishInflate();
            
    }

    // ... 
}
```

对应的XML

```xml
<package.MyClass xmlns:android="http://schemas.android.com/apk/res/android">
    <View android:id="@+id/view1"/>
    <View android:id="@+id/view2"/>
    <!-- ... -->
</package.MyClass>
```

使用时

```xml
<include 
    android:id="@+id/layout1"
    layout="@layout/mylayout" />
<include 
    android:id="@+id/layout2"
    layout="@layout/mylayout" />
```

插句题外话，因为`include`导致内部id重复，无法在主布局节点上直接使用id找到特定元素。只能间接查找，例如：

```java
View view11 = findViewById(R.id.layout1).findViewById(R.id.view1);
View view12 = findViewById(R.id.layout1).findViewById(R.id.view1);
```

但是需要知道的是，获取这些子元素应该是不合理的，它很可能会破坏原有逻辑。

### 优点

灵活的定制视图，只需要保持id一致就可以，接口也可以大幅度重新定制。
优秀的持续拓展能力，只需要继承或集成它，同时修改Layout XML就能加入新功能。

### 缺点

内部布局对用户暴露，用户可以很轻易的获取子视图并控制，打破原有逻辑。
可能要做较多的接口传递。特别是扩展简单功能的时候，仍然需要传递很多控制方法。
如果只是在某个类型视图上追加一些简单功能，也无法在需要某个基础控件的地方直接替换，而需要修改调用方代码。
`include`标签中无法设置布局参数，布局参数是被包含布局根节点的参数，控制不灵活。可以通过简单的额外嵌套一层布局来相对加强控制，控制力度任然有限，例如`wrap_content`，依然不能变成`match_parent`，而且加深了布局深度，即性能损失。也可以通过手工将XML内容展开，但是显然这样处理非常的不好。还可以自定义一个简单的布局专门用来传递布局参数给它的唯一子视图。

## 方案三

在方案二的基础上做了一些修改，主要是为了处理无法灵活控制布局。类定义不变，其他变化如下：

对应的XML

```xml
<marge xmlns:android="http://schemas.android.com/apk/res/android">
    <View android:id="@+id/view1"/>
    <View android:id="@+id/view2"/>
    <!-- ... -->
</marge>
```

使用时

```xml
<package.MyClass
    android:id="@+id/layout1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
        
    <include 
        layout="@layout/mylayout" />

</package.MyClass>

<package.MyClass
    android:id="@+id/layout2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
        
    <include 
        layout="@layout/mylayout" />

</package.MyClass>
```

### 优点

同方案二，并解决了布局参数难以调整的缺点。

### 缺点

同方案二缺点，除布局参数问题。
`marge`作为根节点，Layout XML预览错乱（貌似现在可以通过`tools:context`处理了）。

## 方案四

在方案二或方案三基础上将响应`onFinishInflate`改为在`OnCreate`中使用`LayoutInflate`填充视图，实际上是将布局主导视图换成了类主导视图。接着定义空间自己的XML属性。

### 优点

使用更像原生控件，子控件被封装在内。

### 缺点

对视图只有有限的控制。

## 总结

四种方案都有各自的优缺点，应该根据不同场景来使用。像方案一就很适合做简单扩展，方案四非常适合做逻辑功能扩展。方案二适合在特定项目中使用，方案三适合对视图控制非常灵活时使用。