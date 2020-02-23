## 目录
- 问题描述
- 自定义Attribute的方法
- 解决方案
  
## 问题描述
- 最近在写一个使用c#实现的服务器，接受到请求后调用对应的业务代码完成响应，不想手动一个一个注册handler希望自动完成，所以有了本文

## 自定义Attribute方法
- 下面的例子定义了回响应请求的类和方法的Attribute
- 类的Attribute
```
using System;

namespace DC
{
    [AttributeUsage(AttributeTargets.Class, Inherited = false)]
    public class HandlerClsCfg : Attribute
    {
        
    }
}
```
- 方法的Attribute
```
using System;

namespace DC
{

    [AttributeUsage(AttributeTargets.Method, Inherited = false)]
    public class HandlerCfg : Attribute
    {
        public int mId;

        public HandlerCfg(int id)
        {
            mId = id;
        }
    }

}
```

## 解决方案
- 在服务器启动的时候扫描所有加了Attribute的方法，将符合条件的方法加到请求处理器映射
- 使用上面自定义的Attribute的请求处理器
```
using DC.Net;
using Dcgameprotobuf;

namespace DC
{
    [HandlerClsCfg]
    public class PlayerHandler : BaseReqHandler
    {
        [HandlerCfg(DCProtocolIds.AddRoleReq)]
        public void HandleAddRoleReq(ClientHandler clientHandler, int id, ProtoPacket packet)
        {
            var roleReq = (AddRoleReq)packet.ProtoObj;
            var role = PlayerDB.Instance.CreateRole(clientHandler.User.ID, (int) roleReq.Job);
            clientHandler.Role = role;
            var addRoleRes = new AddRoleRes();
            clientHandler.Send(addRoleRes);
        }
    }
}
```
- 分发器初始化代码，搜索所有加了自定义Attribute的类的加了Attribute的方法
```
protected override void OnInit()
{
    var types = GetType().Assembly.GetTypes().Where(t=>t.IsClass && t.GetCustomAttributes(typeof(HandlerClsCfg), false).Length > 0);
    foreach (var handlerClsType in types)
    {
        var instance = Activator.CreateInstance(handlerClsType);
        if (instance is BaseReqHandler baseReqHandler)
        {
            baseReqHandler.OnInit();
        }

        var methodInfos = handlerClsType.GetMethods();
        foreach (var methodInfo in methodInfos)
        {
            var handlerCfg = methodInfo.GetCustomAttribute<HandlerCfg>();
            if (handlerCfg == null)
            {
                continue;
            }

            var cfgMId = handlerCfg.mId;
            var handler = methodInfo.CreateDelegate(typeof(ReqHandler), instance);
            mIdToDelegates.Add(cfgMId, handler);
        }
        mReqHandlers.Add((BaseReqHandler) instance);
    }
}
```

## 此时完成了一个方便的请求分发器