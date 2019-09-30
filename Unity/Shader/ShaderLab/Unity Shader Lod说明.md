# Unity Shader Lod说明

## 格式
```
Shader "ShaderName"
{
    SubShader
    {
        LOD 100
    }
    SubShader
    {
        LOD 200
    }
    SubShader
    {
        LOD 400
    }
}

```

## 说明
- LOD提供了在不同性能级别的显卡上切换subshader的方法。比如高性能显卡的LOG设置的高一些。
- 设置LOD选择shader的方法
  - Shader.maximumLOD
  - Shader.globalMaximumLOD
- 正常情况下subshader逐个尝试使用，如果可以用则选择使用，否则继续尝试下一个。使用Lod之后可以直接过滤掉不匹配的subshader

## unity内置shader的lod
```
VertexLit kind of shaders = 100
Decal, Reflective VertexLit = 150
Diffuse = 200
Diffuse Detail, Reflective Bumped Unlit, Reflective Bumped VertexLit = 250
Bumped, Specular = 300
Bumped Specular = 400
Parallax = 500
Parallax Specular = 600
```