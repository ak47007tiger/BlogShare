# 2分钟理解PBR(Physically Based Rendering)和BRDF(Bidirectional Reflectance Distribution Function)

## 什么是PBR
- 基于物理的渲染

## 怎么实现PBR
- BRDF是实现PBR的一种方法

## 什么是BRDF
- 物体表面粗糙，很多细小表面产生反射，使用BRDF渲染粗糙表面

## 怎么实现BRDF
- 高光 NDF(Normal Distribution Function)
  - 物体的高光反射，有很多高光公式，可以选用blinn-phong，也可以用其它的
- 几何阴影 GSF(Geometric Shadowing Function)
  - 细小表面互相产生阴影，反射，光能量衰减，最终影响到显示，公式有很多
- 菲涅尔反射 Fresnel Function
  - 物体表面反射和漫反射同时发生，以一定比率混合
  - 可以使用fresnel系数公式计算比率

## 总结
- BRDF是一种实现PBR的方式
- 高光、几何阴影、菲涅尔反射共同构成了一个BRDF渲染