## 机制
- rgb中存放的不是原始rgb，存放rgb*a后的值

## 传统混合方式
- colA.rgb * colA.a + colB.rgb * (1 - colA.a)
- 这个混合方式忽略了colB.a的影响导致最终结果在需要考虑colB.a的时候异常

## 使用premultiplied alpha的blend方式
- colA.rgb + colB.rgb * (1 - colA.a)
- 如果A使用premultiplied alpha, colA.rgb + colB.rgb * (1 - colA)
- colB作为已经显示的颜色其alpha是1

## 原因
- filtering处理像素的时候使用目标像素和周围像素的加权平均值
  - 这改变alpha的值
  - 在透明图像的边界产生新的像素，带有颜色的边界

## 结论
- 使用传统方式blend带有透明像素的图像时因为filtering的原因会导致显示异常
- 当图像很小的时候这种异常会更加明显

## 策略
- 认为已经premultiplied alpha的图filtering之后的图也是premultipied的，使用premultiplied混合方式

## 相关问题
- 精读丢失，float和255的rgb互转会丢失信息

```
filter example: colC = (colA + colB) * 0.5

(1,0,0,1) blend (0,1,0,0.1)
(1,0,0) * 1 + (0,1,0) * (1 - 1) = (1,0,0)

(0,1,0,0.1) blend (1,0,0,1)
(0,1,0) * 0.1 + (1,0,0) * (1 - 0.1) = (0,0.1,0) + (0.9,0,0) = (0.9,0.1,0)
```