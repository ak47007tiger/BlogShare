# Unity 固定渲染管线光照(Legacy Lighting)实例

## 说明
- material和lighting参数控制的是顶点光照模型
- 如果使用了顶点着色器，上面的相关命令就无效了
- 建议使用可编程着色器，固定管线光照已经过时
- 顶点着色和光照在使用纹理前就已经被计算，运行在顶点处理阶段

## 语法
- Lighting On | Off
  - 开启光照Material Block才能起效果
  - 如果没看就使用Color命令指定的颜色
- Color color
  - 设置成固定颜色
- Material{Material Block}
  - 影响最终光照颜色的主要部分
- SeparateSpecular On | Off
  - 需要Lighting On
  - 控制高光
  - 加到pass的最后，不受纹理影响
- ColorMaterial AmbientAndDiffuse | Emission
  - 使用顶点的颜色替换MaterialBlock里面指定的颜色
  - AmbientAndDiffuse 换掉Ambient和Diffuse
  - Emission 换掉Emission

## Material Block
### 如果不指定某个属性，则这个属性不产生影响
- Diffuse 漫反射颜色。物体基本颜色
- Ambient Lighting Window里面配置的环境光
- Specular 高光
- Shininess 高光比率 范围(0,1) 越大高光越强
- Emission 自发光
### 最终光照公式
> Ambient * Lighting Window’s Ambient Intensity setting + (Light Color * Diffuse + Light Color * Specular) + Emission
### 一般Diffuse和Ambient颜色相同

