# unity 无法在ios上热更的原因和应对
- 苹果禁止
- Mono虚拟机支持动态编译，因为苹果禁止，所以不能用
  
# 动态编译原理
- c# -> CIL
- CIL 通用中间语言
  - 可以被Mono虚拟机解释执行
- 对Mono来说CIL是一种资源文件
- CIL到本地指令的即时编译
  - 依赖JIT (Just in Time)技术
- 在ios上mono是Full AOT模式
  - 一切在运行前都编译成ios的本地代码

# CIL原理
- 高级语言 -> CIL -> 虚拟机运行时生成Native Code并且运行

# 苹果是如何禁止CIL的
- 苹果禁止内存区域的可执行模式
  - 运行时分配的内存都不给可执行的权限

# 如何突破苹果的封锁
- 这是我推断的，没实验过，当提供一个思路
- 编译期生成无用的代码占用足够的内存，确定这段内存的地址，如此便获得有可执行权限的内存段A
  - 如果知道一个native code占用多少内存，重复摆放这个native code就占用了内存
  - 函数名是内存的起点，起点+长度是终点
- 运行时把临时生成的native code复制到内存A里面执行

# 确定可以绕过苹果的封锁的方法
- 使用Lua
  
# ios上热更预制资源导致崩溃的原因
- 直接原因：unity序列化出错
  - 修改了mono代码，资源的序列化方式变更
  - ios不能更新代码，所以用的是旧的序列化设置
  
# 哪些操作一定会导致序列化变更
- 增加/减少mono类field
- 改变mono类field类型 && 新类型的内存占用和旧的不同

# 术语
- JIT：Just in Time 
  - 即时编译
AOT：Ahead of Time
  - 静态编译

# 备注
- CIL的VM是基于堆栈的

# 参考
- https://www.cnblogs.com/murongxiaopifu/p/4278947.html
- https://www.cnblogs.com/murongxiaopifu/p/4211964.html