# Unity 固定渲染管线雾效 Legacy Fog

## Unity5和之后的版本除了关掉雾效，其它Fog命令都无效

## 说明
- 雾效根据相机距离混合生成的像素和固定颜色，只改变rgb通道的值
- Fog
  - Fog {Fog Commands}
- Mode
  - Mode Off | Global | Linear | Exp | Exp2
- Color
  - Color ColorValue
- Density
  - Density FloatValue
- Range
  - Range FloatValue, FloatValue
- 即使使用了fragment shader雾效命令仍然有效
- 在固定管线不支持雾效的平台上，unity在运行时通过补丁的形式支持雾效命令

## 效果


## 代码
```
Shader "Practice/FixedFunction/Fog"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        _Color("Color", Color)=(1,0,0,1)
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 100

        Pass
        {
            Fog{
                //关闭
                // Mode Off
                // Mode Exp2
                Mode Global
                //尝试了几种方式这里指定Color无效，在Rendering/Lighting Settings/Fog下设置颜色有效
                // Color (1,0,0,1)
                Color [_Color]
                Density 0.5
                Range 0, 20
            }
            // SetTexture [_MainTex]{combine primary * texture}
            SetTexture [_MainTex]{combine texture}
        }
    }
}
```