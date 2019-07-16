# Unity 固定渲染管线 SetTexture实例

## 说明
- SetTexture提供一种使用纹理并且进行混合的方式
- 后面的Block中可以定义颜色和进行数学运算

## 重点
- 混合计算方法
  - *,+,one-,lerp
- alpha混合
- 加入颜色constantColor

## 代码
```
Shader "Practice/FixedFunctionShader" {
    Properties {
        _Tex1 ("Texture1", 2D) = "white" {}
        _Tex2 ("Texture2", 2D) = "white" {}
        _Tex3 ("Texture2", 2D) = "white" {}
        _Color("Color",Color)=(1,1,1,1)
    }
    SubShader {
        Tags{
            "RenderType"="Transparent"
            "Queue"="Transparent"
        }
        Blend SrcAlpha OneMinusSrcAlpha
        Pass {
			Lighting Off
            //设置纹理，无block
            SetTexture [_Tex1]

            //设置纹理，空block
            // SetTexture [_Tex1]{}
            
            //设置纹理，数学运算
            // SetTexture [_Tex1] {
                // combine one-texture, texture
                // combine texture, one-texture
                // combine previous
            // }

            //混合方式，Alpha混合控制
            SetTexture [_Tex2] {
                // combine one-texture double, previous
                // combine one-texture, one-texture
                // combine one-texture, previous * texture
                // combine texture, previous * texture
                // combine one-texture, previous
                // combine texture quad, previous
                combine previous + texture
                // combine texture lerp (texture) previous
            }

            //三张纹理进行混合的方法
            SetTexture [_Tex3]{
                // constantColor (1,1,1,1)
                constantColor [_Color]
                // combine previous + texture,texture
                // combine texture lerp(texture) previous, texture lerp(texture) previous
                // combine texture lerp(texture) previous, constant
                // combine texture lerp(texture) previous, previous + texture
                combine constant lerp(texture) previous, previous + texture
            }

        }
    }
}
```