# Animator Controllers
- 管理动画
- State Machine
  - 一种流程图
  - 是unity中的可视化编程

## Animation State Machines
- Basic
  - 状态切换 动画 跳转条件
- Animation Parameters
  - 控制状态转换
  - 设置Blend Tree
- State Machine Transitions
  - 状态转换
  - 转换条件
  - 动画转换过程中后续的动画会覆盖之前的动画的改变
- State Machine Behaviours
  - 动画状态转换过程中各种回调
    - OnStateEnter
    - OnStateExit
    - OnStateUpdate
    - OnStateMove
    - OnStateIK
- Sub-State Machines
  - 用来打包一组状态放到一起显示，有指定的进入状态和退出状态
- Animation Layers
  - 同一个物体不同部分使用不同的动画
  - Animation Layer syncing
    - 从另外一个层同步动画状态机过来，但是可以为每个状态设置不同的动画

## Solo and Mute functionality
- 用于单独预览状态机
- Mute 关闭转换
- Solo 仅播放被标记的转换
- Mute优先级更高

## Root Motion
- Body Transform
  - 重心
  - 固定位置
- Root Transform
  - Y轴的投影
- Back into position：动画变换应用到模型但是不改变根节点，动画结束后跟节点还在动画开始的位置
- Apply root motion: 动画播放时会改变根节点transform，动画停止时根节点在动画停止时的位置
- 使用脚本控制transform
  - 用于原地动画
  - 需要加一个脚本并且实现方法OnAnimatorMove，该方法每帧调用

## Blend Tree
- 使用权重使不同的动画对模型的影响进行混合

## Animator Override Controllers
- 保持原先的Animator Controller的结构、参数、逻辑，但是使用新的动画来替换
- 在同一个状态机上使用不同的动画