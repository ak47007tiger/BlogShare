# Unity 动画

## 动画常识
- 展示生活中可见或者想象中的生物、无机物的主动或者被动变化的过程

## 游戏中的动画类型
- 帧动画
- 关键帧动画
  - 属性动画
  - 顶点动画
  - UV动画
- 骨骼动画
  - 人形动画

## 骨骼动画 Skeleton animation
- 骨骼变化位置、大小、旋转，通过蒙皮技术带动顶点变换，完成动画

## 概述
- 动画的核心组件
  - Animation Clip
  - Animator Contorller
  - Animator
  - Avatar
- 设置核心组件的编辑器窗口

## Animation Clip
- 来源
  - 第三方工具导出的
  - Unity Asset Store下载的
  - 单个的clip切出来的
- 能力范围
  - transform变换
  - 组件属性，如 material colour, the intensity of a light, the volume of a sound
  - 脚本变量 including float, integer, enum, vector and Boolean variables
  - 事件回调脚本代码

## FBX文件
- 骨骼动画
- transform导出
- 动画信息
  - 可以单独导出.anim文件，对应着Animation Clip

## 肌肉控制 muscle
- 骨骼动画和肌肉是两个东西