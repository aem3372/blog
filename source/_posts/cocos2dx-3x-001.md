title: cocos2dx-3.0公共基类和拷贝机制的改变
tags:
  - 公共基类
  - 拷贝机制
id: 1379
categories:
  - cocos2dx
date: 2014-06-06 20:47:17
---

在cocos2dx-2.x的实现中，公共基类是CCObject。
打开[cocos-root]/cocos2dx/cocoa/CCObject.h查看定义：

<!-- more -->

```cpp
/**
 * @js NA
 * @lua NA
 */
class CC_DLL CCCopying
{
public:
    virtual CCObject* copyWithZone(CCZone* pZone);
};

/**
 * @js NA
 */
class CC_DLL CCObject : public CCCopying
{
public:
    // object id, CCScriptSupport need public m_uID
    unsigned int        m_uID;
    // Lua reference id
    int                 m_nLuaID;
protected:
    // count of references
    unsigned int        m_uReference;
    // count of autorelease
    unsigned int        m_uAutoReleaseCount;
public:
    CCObject(void);
    /**
     *  @lua NA
     */
    virtual ~CCObject(void);
    
    void release(void);
    void retain(void);
    CCObject* autorelease(void);
    CCObject* copy(void);
    bool isSingleReference(void) const;
    unsigned int retainCount(void) const;
    virtual bool isEqual(const CCObject* pObject);

    virtual void acceptVisitor(CCDataVisitor &visitor);

    virtual void update(float dt) {CC_UNUSED_PARAM(dt);};
    
    friend class CCAutoreleasePool;
};
```

CCObject继承自CCCopying(没有其他类派生自CCCopying，这里的派生其实是接口机制，其功能很单一，所以不会称CCCopying是公共基类)。
CCCopying的唯一成员函数，其默认实现简单到只通过一个断言提示一个未实现。

```cpp
CCObject* CCCopying::copyWithZone(CCZone *pZone)
{
    CC_UNUSED_PARAM(pZone);
    CCAssert(0, "not implement");
    return 0;
}
```

CCObject为对象提供了拷贝机制、内存管理机制以及一些基本逻辑。
其中我认为一个设计不合理的地方，就是绑定了过多功能，而且设计人员也考虑到了不是每个派生类都需要拷贝机制，但是却强制实现了一个默认接口，而该实现居然是提示未生实现。（因此在cocos2dx-3.x中被重写）

但是在cocos2dx-3.x之中就找不到CCObject类了，取而代之的是Ref类。
在[cocos-root]/cocos/base下可以找到CCRef.h文件，查看定义：

```cpp
/**
 * @addtogroup base_nodes
 * @{
 */

class Ref;

/** Interface that defines how to clone an Ref */
class CC_DLL Clonable
{
public:
    /** returns a copy of the Ref */
    virtual Clonable* clone() const = 0;
    /**
     * @js NA
     * @lua NA
     */
    virtual ~Clonable() {};

    /** returns a copy of the Ref.
     @deprecated Use clone() instead
     */
    CC_DEPRECATED_ATTRIBUTE Ref* copy() const
    {
        // use "clone" instead
        CC_ASSERT(false);
        return nullptr;
    }
};

class CC_DLL Ref
{
public:
    /**
     * Retains the ownership.
     *
     * This increases the Ref's reference count.
     *
     * @see release, autorelease
     * @js NA
     */
    void retain();
    
    /**
     * Release the ownership immediately.
     *
     * This decrements the Ref's reference count.
     *
     * If the reference count reaches 0 after the descrement, this Ref is
     * destructed.
     *
     * @see retain, autorelease
     * @js NA
     */
    void release();

    /**
     * Release the ownership sometime soon automatically.
     *
     * This descrements the Ref's reference count at the end of current
     * autorelease pool block.
     *
     * If the reference count reaches 0 after the descrement, this Ref is
     * destructed.
     *
     * @returns The Ref itself.
     *
     * @see AutoreleasePool, retain, release
     * @js NA
     * @lua NA
     */
    Ref* autorelease();

    /**
     * Returns the Ref's current reference count.
     *
     * @returns The Ref's reference count.
     * @js NA
     */
    unsigned int getReferenceCount() const;
    
protected:
    /**
     * Constructor
     *
     * The Ref's reference count is 1 after construction.
     * @js NA
     */
    Ref();
    
public:
    /**
     * @js NA
     * @lua NA
     */
    virtual ~Ref();
    
protected:
    /// count of references
    unsigned int _referenceCount;
    
    friend class AutoreleasePool;
    
#if CC_ENABLE_SCRIPT_BINDING
public:
    /// object id, ScriptSupport need public _ID
    unsigned int        _ID;
    /// Lua reference id
    int                 _luaID;
#endif
};
```

这次的Ref就规范得多，首先为拷贝定义了clonable接口，Ref也没有去实现clonable接口，这就相当于将拷贝机制独立出来了。如果一个继承Ref的类需要拷贝功能，就要自己实现clonable接口。
而Ref作为公共基类提供内存管理功能，以及和Lua脚本通信的数据维护。