title: '解决CocosStudio动画编辑器取消补间动画在程序中加载后无效问题'
tags:
    - CocosStudio
    - 补间动画
id: 1422
categories:
    - cocos2dx
date: 2014-08-30 03:41:35
---

## 开发环境
库版本： Cocos2d-x 3.0
编辑器版本： Cocos Studio 1.6.0

---

**异常描述： 取消补间动画后，在编辑器中看到已经没有了补间动画，但是将动画导出后加载到程序中还是有补间动画的。**

[![CocosStudio-001](http://www.aemiot.com/wp-content/uploads/2014/08/CocosStudio-001.png)](http://www.aemiot.com/wp-content/uploads/2014/08/CocosStudio-001.png)

用记事本打开导出的Json文件，发现存在属性 "tweenFrame": false。 可以看到导出的信息是完整的。

那么问题就在程序上，跟踪程序，发现成员变量isTween一直是true。 继续跟踪，发现**CCDatas.cpp中FrameData::copy函数没有对isTween进行拷贝**。所以在函数中加上isTween的拷贝过程就好。

<!-- more -->

修正后的代码:

```cpp
//CCDatas.cpp
void FrameData::copy(const BaseData *baseData)
{
    BaseData::copy(baseData);
        
    if (const FrameData *frameData = dynamic_cast<const FrameData*>(baseData))
    {
        duration = frameData->duration;
        displayIndex = frameData->displayIndex;
        
        tweenEasing = frameData->tweenEasing;
        easingParamNumber = frameData->easingParamNumber;
        
        CC_SAFE_DELETE(easingParams);
        if (easingParamNumber != 0)
        {
            easingParams = new float[easingParamNumber];
            for (int i = 0; i<easingParamNumber; i++)
            {
                easingParams[i] = frameData->easingParams[i];
            }
        }

        blendFunc = frameData->blendFunc;
        // Bug Fix
        isTween = frameData->isTween;
    }
}
```

## 参考文章
- [cocos2dx3.0无法取消Armatrue骨骼动画中的补间效果问题的解决办法](http://blog.csdn.net/leafvmaple/article/details/24894015 "http://blog.csdn.net/leafvmaple/article/details/24894015")
