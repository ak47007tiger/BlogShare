## overview
- 使用不同的layer管理状态机
- 不同的layer使用不同的mask可以用来控制物体不同的区域，从而控制不同位置播放不同动画
- mask是AvatarMask，用来设置Avatar哪些地方被影响到

## Blending模式
- override，覆盖其它layer
- adtive，叠加到其它layer

## Animation Layer syncing
- 用于快速制作其它层的状态机，设置不同layer相同状态下动画时间长度
- 直接复制其它层的状态机过来，但是不复制相关的动画，动画在新创建的层中指定
- Timing checkbox设置
  - 选中：根据不同层的weight设置动画长度
  - 不选中：根据原始layer的动画设置动画长度

## 图例
![](https://docs.unity3d.com/uploads/Main/AnimatorSyncedLayer.png)