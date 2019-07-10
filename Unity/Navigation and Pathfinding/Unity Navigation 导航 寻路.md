# Unity Navigation 导航 寻路
## 概述
- 了解Navigation的能力范围

## Navication系统的构成
- NavMesh
  - 导航网格，是unity烘焙出来的数据
- NavMesh Agents
  - 帮助角色寻路
- Off-Mesh Links
  - 连接2点的组件，也可以用来跨越缝隙
- NavMesh Obstacles 
  - 移动的或者不动的障碍物

## Navication系统的原理
- 两个问题
  - 找到目标场景中的目标位置
    - 从全局考虑整个场景
  - 怎么移动到那里
    - 确定移动方向、避免和其它移动物体碰撞
- Walkable Areas
  - Walkable Areas指人物能站的地方
  - NavMesh被存储为凸多边形
    - 多边形的边界
    - 多边形的邻居
    - 以此退出整个可移动区域
- Finding Paths
  - 找到给的起止点对应的多边形
    - 距离给出的点最近的那个多边形
  - 找到这两个多边形相连的多边形序列
    - 使用A*算法查找
- Following the Path
  - 从开始到终止的区域的多边形序列叫做回廊
  - 人物通过移动到回廊中下一个可见的拐角来抵达目的地
  - 如果有多个人物同时在寻路移动还像上面那样做就有问题了
  - 解决方法：人物会先向后迂回然后用上面的方法寻路
- Avoiding Obstacles
  - 障碍物碰撞在向目标方向移动和阻止碰撞间选取新的速度
  - 寻路过程会撞到其它人物，Unity使用反作用力来预测和阻止碰撞
- Moving the Agent
  - 使用加速度方式平滑自然的移动物体
  - 可以在动画系统中使用root motion来移动物体
  - 或者让导航系统自动处理
- Global and Local
  - Global指查找一个从起始点到目标点的路径，消耗很多cpu和内存
  - Local指高效移动到下一个拐角并且不和其他人物以及移动物体碰撞
- Two Cases for Obstacles
  - 局部障碍物躲避
    - 仅考虑即将发生的碰撞，无法解决绕过陷阱，障碍物堵塞了路径的情况
  - 全局的路径搜索
    - 障碍物在移动的时候会导致NavMesh变化，影响全局路径搜索
      - 通过在NavMesh上产生一个洞来影响路径搜索
- Describing Off-mesh Links
  - 不可以走的地方，比如围墙、关闭的门，使用链接完成寻路
  - 链接2个多边形，告诉寻路系统可以通过特殊的链接寻找路径
  - 指出执行特殊动作的位置

## 创建NavMesh导航网格
- 在创建了所有的组件之后烘焙出运行时导航系统需要的数据
- 选中需要导航的网格，在static中勾选Navigation，设置属性，Bake

## NavMesh building components
- 更高层级的NavMesh构建组件，可以在asset store下载，或者github上下载
- https://store.unity.com/?_ga=2.69928656.1307606252.1561945940-1506278448.1552462098
- https://github.com/Unity-Technologies/NavMeshComponents
  - NavMesh Surface
  - NavMesh Modifier
  - NavMeshModifierVolume
  - NavMeshLink
  - NavMesh building components API

## Advanced NavMesh Bake Settings
- Min Region Area
- Voxel Size
- Smaller Agent Radius
- More Accurate NavMesh

## 创建各类组件
- Creating a NavMesh Agent
- Creating a NavMesh Obstacle
- Creating an Off-mesh Link
- Building Off-Mesh Links Automatically
  - *Drop Height*应该设置大一点，确保烘焙的时候不会导致连接断开
  - *Jump Distance*应该大一点，2*agentRadius大小确保可以跳过间隙

## Navigation Areas and Costs 导航和花费
- unity使用A*算法计算路径
- 花费是一种权重的概念，路线花费越高被选择的可能性越低
- 在AreaTypes面板设置花费值
- 可以在NavMeshAgent组件中勾选哪些Area类型可以该组件寻路

## Loading Multiple NavMeshes using Additive Loading
- 加载多个导航网格
- 加载多个场景的导航网格

## Using NavMesh Agent with Other Components
- NavMeshAgent组件和其它组件配合使用

## 参考
- NavMeshAgent
- NavMeshObstacle
- Off-Mesh Link

## How To 案例
- 巡逻AI
- 动画和导航配合使用