# Unity Compute shaders 计算着色器
- 概述
  - 运行在显卡上独立与普通的渲染管线之外的程序
  - 可以运行并行算法
  - 可以加速游戏渲染速度
  - SystemInfo.supportsComputeShaders 支持信息
## Compute shader Assets Unity用什么来存储计算着色器
- .compute文件，使用hlsl风格的语言
- 下面的shader用红色填充纹理
```
#pragma kernel FillWithRed

RWTexture2D<float4> res;

[numthreads(1,1,1)]
void FillWithRed (uint3 dtid : SV_DispatchThreadID)
{
    res[dtid.xy] = float4(1,0,0,1);
}
```
- compute shader必须有一个kernel，可以有多个kernel
- #pragma kernel KernelName
```
#pragma kernel KernelA
#pragma kernel KernelB
```
## Invoking compute shaders 执行计算着色器的方法
- 在脚本中持有ComputeShader引用，使用ComputeShader.Dispatch运行
- 数据准备
  - ComputeBuffer
  - 开启了enableRandomWrite的RenderTexture

## Texture samplers in compute shaders 计算着色器中的纹理和采样器
- 纹理材质不分家
- 在unity中使用遵循命名规则
- 格式TextureNameSimpler
```
Texture2D MyTex; SamplerState samplerMyTex
```
- 使用内置sampler
  - 设置filter mode
  - 设置wrap mode
  - 格式：textureNameFilterModeWrapMode

## Cross-platform support 跨平台支持
- 概述
  - unity会翻译hlsl到特定平台的shader语言
- Cross-platform best practices
  - DX11支持的很多特性不被 Metal or OpenGL ES支持，所以能少用特性就少用
  - out-of-bounds 避免内存越界错误
  - 初始化资源。如果没有初始化，有的平台获得0，有的NaNs
  - 绑定所有资源。即使某个if分支不会使用一些资源，仍要绑定
- Platform-specific differences
  - Metal
    - 不支持纹理上的原子操作
    - 不支持GetDimensions查询，所有要传到shader里面
  - OpenGL ES 3.1仅支持4个Compute Buffer，所以为opengl es平台开发的时候最好把数据用structs组装

## HLSL-only or GLSL-only compute shaders 特定平台处理
- 有可能出现只要HLSL的平台或者需要手动写GLSL的情况
- CGPROGRAM ENDCG包裹的内容不会被非HLSL平台处理
- GLSLPROGRAM ENDGLSL包裹的内容会被逐字作为GLSL源码处理。只在目标平台是OpenGL或者GLSL平台有效。
- 自动转换的shader使用HLSL数据布局方式，手动写的GLSL使用GLSL数据布局方式
- data layout：数据在内存中放置的方式，可能跟字节对齐有关