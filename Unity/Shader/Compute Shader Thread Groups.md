# 概述
- 文本旨在
  - 介绍 Dispatch(x,y,z)
  - 介绍 numthreads(x,y,z)
  - 说明 Dispatch 和 numthreads 间的联系
    - 参数间关系
    - 如何传递参数
  - 纹理大小和dispatch、numthreads
  
## Dispatch
- dispatch(x,y,z)：使用 x * y * z 个线程组

## numthreads
- numthreads(x,y,z)：一个线程组有多少线程
- 意义
  - 同一个组内的shader invocation能够共享变量和特殊函数

## 联系
- texture.size.x * texture.size.y = (dispatch.x * dispatch.y * dispatch.z) * (numthreads.x * numthreads.y * numthreads.z)

## 问题
- 如何在shader中区分是哪个像素
```
void FillWithRed (uint3 dtid : SV_DispatchThreadID)
{
    res[dtid.xy] = float4(1,0,0,1);
}
```
dtid.x是水平方向的像素
dtid.y是竖直方向的像素
- 如果 texture 像素数量 < 线程数量 会发生什么

## 例子
- 400x300的纹理选择的参数
  - dispatch
  - numthreads