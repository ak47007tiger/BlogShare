# Water in Unity 使用Unity提供的水效果
- 需要Standard Asset 和 Pro Standard Asset
- 使用Standard Assets packages添加白天、夜里的水
- 不支持vr中的水
  
## Setting up water 设置
## Creating water from scratch (Advanced) 创建步骤
- 简单水
  - 准备mesh，设置Layer为Water Layer
  - 加入WaterSimple script
  - 准备材质
    - 使用FX/Water(simple) shader创建
    - 调整属性
- 带有折射、反射的水
  - 和简单水类似，不同在于
    - 使用Pro Standard Assets/Water/Source下的Water script
    - 使用FX/Water Shader
## Properties in water Materials 材质属性
- 折射、反射
  - Wave scale
  - Reflection/refraction distort，折射、反射的程度，由normal map控制
  - Refraction color
  - Environment reflection/refraction，环境纹理
  - Normalmap，在不同的方向上滚动，使用不同的缩放和速度，第二张图是第一张的一半大小
  - Wave speed
  - Fresnel，菲尔涅反射，根据视线入射角决定反射和折射能被看到多少
- 多数属性在简单水中也被使用
- 简单水使用的或者在显卡不支持的时候被启用的
  - Reflective color/cube and fresnel
    - rgp通道定义水的颜色
    - a通道定义菲涅尔反射效果，根据入射视线
  - Horizon color，仅简单水使用，表面颜色
  - Fallback texture，没有着色器满足要求则显示这个纹理