# Event System 事件系统
- 用来发事件给object
## overview
- 暴露的接口很少
- 主要作为Event System modules的管理者和信使
- 主要职责
  - 管理被选中的GameObject
  - 管理正在使用的输入模块
  - 管理射线
  - 更新输入系统
## Input Modules
- Event System的主要逻辑
- 功能
  - 处理输入
  - 管理事件状态
  - 给scene object发事件
- 同一时间只能有一个Input Module被激活，而且必须和Event System在同一个GameObjct上
## Raycasters
- 用来确定指针(手指、鼠标)在哪里，一般在场景里面配置

# Messaging System
## 概览
  - 在Monobehaviour中实现了对应的接口后就可以收到来自messaging framework的消息
  - 不仅仅可以在UI中使用这个系统，也可以在游戏逻辑中使用
## How Do I Define A Custom Message 自定义消息
- ITarget继承IEventSystemHandler接口
- 实现ITarget接口
- 触发
  - ExecuteEvents.Execute<ICustomMessageTarget>(target, data, (x,y)=>x.Message1());
    - target 希望处理这个消息的对象
    - data 数据
    - 参数3 指明这个对象的哪个方法来处理
  - ExecuteHierarchy(GameObject root, EventSystems.BaseEventData eventData, EventFunction<T> callbackFunction);
    - 该方法可以从一个根节点开始查找一个可以处理消息的对象

# Input Modules
- event system主要的逻辑，可以被配置和自定义
- 平台
  - standalone平台
  - Touch类平台
- Input Modules被设计于映射硬件特定的输入到被messaging system发送的事件
- 在Monobehaviour中实现对应的接口就可以接受到Input Module发出的事件

# Supported Events
- 自定义Input Module可以更进一步定义系统支持的events
- 具体类型见：https://docs.unity3d.com/Manual/SupportedEvents.html

# Raycasters
- 概述
  - Event System用Raycaster来决定把当前输入发给谁
- 类型
  - Graphic Raycaster UI元素使用
  - Physic 2D Raycaster 2D物理元素使用
  - Physics Raycaster 3D物理元素使用

# Event System Reference
## Event System Manager
- 管理用来构成事件的元素
- 每帧更新的时候选择要使用的Input Module并且用其做完处理者
## Standalone Input Module
- 设计用于鼠标类型输入的平台
- 使用Graphics Raycaster 和 Physics Raycaster计算什么元素被指着
- 每秒钟的事件数量有限
- 流程
  - if new press
    - enter
    - press
    - cash drag handler
    - begin drag
    - set pressed obj
  - if continuing press
    - 处理移动事件
    - 发送DragEvent给之前缓存的drag handler
    - 处理PointerEnter和PointerExit如果是在2个obj间移动
  - if release
    - PointerUp
    - 如按下和松开的都是同一个obj，发送PointerClick
    - Drop Event如果有drag handler
    - EndDrag
  - 处理鼠标滚轮滚动事件
## Touch Input Module
- 已经陈旧了，现在Touch Input在StandaloneInputModule里处理
## Event Trigger
- 接收来自Event System的事件并且调用注册处理这些事件的方法
- 一个挂载了EventTrigger的GameObject会拦截所有事件
- 可以用**Add New Event Type button**选择Event Trigger要包含的事件