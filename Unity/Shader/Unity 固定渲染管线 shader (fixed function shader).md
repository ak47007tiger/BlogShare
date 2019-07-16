# Unity 固定渲染管线 shader (fixed function shader)

## 指南
- 固定渲染管线无法编程计算，通过修改参数和配置进行渲染操作
- 固定管线着色器已经过时
- 学会unity shader的语法后主要借鉴OpenGL中的固定管线着色器来编写unity固定管线shader
  - 掌握光照模型
  - 掌握各类渲染设置

## 前置知识
- OpenGL or Direct3D
- HLSL,Cg,GLSL or Metal Shader programming languages

## 概述
- 在unity内部，fixed function shader在导入shader时被转换为 vertex fragment programs

## 构成
- Properties
  - 传入shader的参数
    - 名称("显示名称", 类型) = 默认值
- The shader body
  - 定义shader主体内容
- Passes
  - 定义具体每一次渲染

## buildin properties and functions 内建属性和方法
- 这些属性是直接被OpenGL映射的，可以参考
  - OpenGL red book http://opengl.org/documentation/red_book
- 例如
  - 光照模型
    - Diffuse
    - Ambient
    - Shininess
    - Specular
    - Emission
  - 纹理、颜色、混合
    ```
    SetTexture [_Texture]{
      constantColor
      Combine
    }
    ```