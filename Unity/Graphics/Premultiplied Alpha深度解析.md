## 机制
- rgb中存放的不是原始rgb，存放rgb*a后的值

## 传统混合方式
- colA.rgb * colA.a + colB.rgb * (1 - colA.a)
- 这个混合方式忽略了colB.a的影响导致最终结果在需要考虑colB.a的时候异常

## 使用premultiplied alpha的blend方式
- colA.rgb + colB.rgb * (1 - colA.a)
- 如果A使用premultiplied alpha, 最终颜色为colA.rgb + colB.rgb * (1 - colA.a)
- colB作为已经显示的颜色其alpha是1，我说这句话没什么特殊含义，仅描述了一个事实：已经显示的颜色alpha是1

## 原因
- filtering处理像素的时候使用目标像素和周围像素的加权平均值
  - 这改变alpha的值
  - 在透明图像的边界产生新的像素，带有颜色的边界
    - 因为加权平均使颜色拓展到周边，透明区域产生了颜色值
- 这解释了为何png图片显示到游戏中后其边缘会出现带有颜色的边，比如黑边

## 结论
- 使用传统方式blend带有透明像素的图像时因为filtering的原因会导致显示异常
- 当图像很小的时候这种异常会更加明显

## 策略
- 认为已经premultiplied alpha的图filtering之后的图也是premultipied的，使用premultiplied混合方式。这样rgb仍会拓展出去，因为alpha没有变化所以带有颜色的边仍是透明的，不影响显示

## 相关问题
- 精读丢失，float和255的rgb互转会丢失信息

```
filter example: colC = (colA + colB) * 0.5

(1,0,0,1) blend (0,1,0,0.1)
(1,0,0) * 1 + (0,1,0) * (1 - 1) = (1,0,0)

(0,1,0,0.1) blend (1,0,0,1)
(0,1,0) * 0.1 + (1,0,0) * (1 - 0.1) = (0,0.1,0) + (0.9,0,0) = (0.9,0.1,0)
```