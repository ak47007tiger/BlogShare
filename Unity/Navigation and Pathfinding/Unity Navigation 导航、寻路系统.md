# Unity Navigation 导航、寻路系统
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

## NavMesh building components 更高层级的NavMesh构建组件，非内置，在github上可下载
- 地址
  - https://store.unity.com/?_ga=2.69928656.1307606252.1561945940-1506278448.1552462098
  - https://github.com/Unity-Technologies/NavMeshComponents
- NavMesh Surface
  - 拓展了原先的NavMesh
    - 支持限制不同agent在不同区域类型
    - 可以指定非static烘焙导航数据
    - 可以指定那些物体被烘焙到导航数据
- NavMesh Modifier
  - 可以在烘焙的时候对要烘焙的对象做额外处理和设置
  - 如果有一堆东西中只有个别物体的设置不同，使用这个会方便
- NavMeshModifierVolume
  - 可以修改一个范围区域的烘焙数据
    - 修改Area Type
    - 修改影响的Agent类型
- NavMeshLink
  - 用来连接2个NavMesh Surface
  - 注意事项
    - 必须是2个不同的NavMesh Surface
    - NavMesh Surface和NavMeshLink必须是相同的Agent类型
    - NavMeshLink的起点或者终点必须只连接在单一的一个NavMeshSurface上
    - 如果有场景分开加载，确保第一个场景中的NavMeshLink不会连接到后续场景中错误的地方
- NavMesh building components API
  - 关于上面组件的属性结束和api说明

## Advanced NavMesh Bake Settings 高级烘焙设置
- Min Region Area
  - 最小区域，如果小于这个区域不会被烘焙进导航数据
- Voxel Size
  - 格子大小，越小性能消耗约大，同时也越精确
- Smaller Agent Radius
  - 最终的导航网格分辨率根据最小的那个生成
  - 手动设置导航网格分辨率使用"Mannual Voxel Size"
- More Accurate NavMesh
  - voxel size和 Agent Radius之间应该有2-8倍的数量关系
  - 如果想构建紧凑的走廊，除了agent size之外，还要留有至少4个voxelSize的空隙
  - 如果是比走廊还小的寻路路线，使用Off-Mesh Links

## 创建各类组件
- 比较简单的，一试便知
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
- 可以在NavMeshAgent组件中勾选Area Mask里的选项决定哪些Area类型可以进行寻路

## Loading Multiple NavMeshes using Additive Loading 加载多个导航网格的方法
- 加载多个场景的时候导航网格自动被加入
- 动态加载场景的时候 导航网格和Off-Mesh Link的关系
  - 导航网格默认不是连在一起的，不同场景的导航网格使用Off-Mesh link链接起来
  - 2个场景的Off-Mesh Link会在第二个场景加载的时候自动链接起来
  - 如果2个场景有重叠区域，并且Off-Mesh Link在重叠区域，Off-Mesh Link不会自动链接

## Using NavMesh Agent with Other Components 导航网格和其它组件系统配合使用的情景
- NavMeshAgent组件和其它组件配合使用，下面是一些可以做、不可以做的清单
- NavMesh Agent and Physics 智能体和物理引擎
  - 无需使用physics colliders来让每个智能体互相躲避，因为导航系统会自动模拟
  - 如果需要物理特性，使用运动学刚体，勾选Rigidbody的Is Kinematic选项
  - 非运动学刚体和导航系统在控制移动上会互相竞争，这会导致无法预料的结果
  - 不使用物理特性也可以移动智能体
    - 使用NavMeshAgent.velocity移动
- NavMesh Agent and Animator 和动画配合
  - 如果Animator开启了Root Motion会导致控制竞争
  - 数据的流向只要一个方向
    - 智能体先移动然后播放动画
    - 或者根据模拟结果使用动画移动角色
  - 根据agent播放动画
    - 使用NavMeshAgent.velocity控制动画，同步移动和动画表现
    - 如果速度和动画不匹配会出现滑动现象
  - 根据动画移动角色
    -  关闭NavMeshAgent.updatePosition and NavMeshAgent.updateRotation
    -  使用NavMeshAgent.nextPosition和Animator.rootPosition计算控制用的变量来控制动画
       -  使用导航系统计算的下一位置和当前位置计算速度来控制播放的动画
  NavMesh Agent and NavMesh Obstacle
    - 不能混合使用
    - 使用Obstacle作为不可移动状态，比如死亡状态
    - 使用priorities优先级来让其它智能体躲避另一个智能体