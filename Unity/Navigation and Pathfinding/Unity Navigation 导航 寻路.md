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
- Walkable Areas
- Finding Paths
- Following the Path
- Avoiding Obstacles
- Moving the Agent
- Global and Local
- Two Cases for Obstacles
- Describing Off-mesh Links

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


## Off-Mesh Link