# Unity DrawCall优化相关 动态合批 静态合批

## 什么是DrawCall
- CPU呼叫GPU进行绘制是一次DrawCall
- DrawCall导致性能下降的原因：CPU传送数据给GPU比较慢，GPU完成绘制过程后等待

## 优化方法
- 相同材质的物体一同绘制

## 如何相同材质的物体一同绘制
- 把使用相同材质的物体的网格合并一同绘制

## 为什么这样可以
- 底层的图形api有一个绘制三角形的接口，这个就是CPU调用gpu绘制的接口
- 在调用这个接口前会准备顶点数据，即网格数据
- 把网格合并后只要进行一次绘制三角形的接口调用，减少了DrawCall

## Unity中的合批
### 静态合批 Static Batching
- Unity中把物体标记为Static，然后开启静态合批
- 限制
  - 需要保持static，不能改变transform
  - 使用相同材质的物体才能合批
  - 一个批次上限为~15k个顶点
### 动态合批 Dynamic Batching
- Unity自带动态合批，需要在Unity中开启动态合批选项
- 会导致cpu消耗，如果不是gpu有瓶颈，最好关闭动态合批
- 前提
  - 180 vertices for a shader using vertex position, normal, UV0, UV1 and tangents
  - 300 vertices for a shader using vertex position, normal and a single UV
- 限制
  - 物体的transform必须有相同的缩放系数
  - 使用相同材质
  - 一起逐个渲染

## 开启方法
- Player Settings > Other Settings
