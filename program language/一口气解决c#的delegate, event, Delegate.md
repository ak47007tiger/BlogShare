## 概述
- 本文旨在说明关键字delegate,event和类Delegate的使用方法
  
## keyword: delegate, event
- delegate：定义一个委托类型，指明什么样的方法属于这样的委托
- event：包装一个委托，实现观察者模式
- 适用：某一个类型的方法调用，方便做观察者模式
### 代码
```
class UseEventAndDelegateKeyword
{
    public delegate void CallFunc();

    private event CallFunc mSender;

    public void Add(CallFunc action)
    {
        mSender += action;
    }

    public void Remove(CallFunc action)
    {
        mSender -= action;
    }

    public void Call()
    {
        if (mSender != null) mSender();
    }
}
```

## Delegate类型
- Delegate：一个class类型，各种各样的方法都可以转化为Delegate
- 适用：需要代表多种类型的方法，也可用于做观察者模式
### 代码
```
class UseDelegateClass
{
    private Delegate mDelegate;

    public void Add(Delegate d)
    {
        mDelegate = Delegate.Combine(mDelegate, d);
    }

    public void Remove(Delegate d)
    {
        mDelegate = Delegate.Remove(mDelegate, d);
    }

    public void Call(params object[] objs)
    {
        if (null != mDelegate)
        {
            mDelegate.DynamicInvoke(objs);
        }
    }
}
```