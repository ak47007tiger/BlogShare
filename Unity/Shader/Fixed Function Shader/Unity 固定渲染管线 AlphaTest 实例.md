# Unity 固定渲染管线 AlphaTest 实例

## 说明
- AlphaTest是拒绝渲染一个像素最后的机会
- 如果使用了FragmentShader，AlphaTest不再起效
- AlphaTest comparison AlphaValue
  - ccomparison是比较命令
  - AlphaValue是(0,1)直接的值
  - 像素的Alpha比较通过才会被渲染
- AlphaTest在深度测试通过之后才会执行，否则不执行
- 详情 https://docs.unity3d.com/Manual/SL-AlphaTest.html

## 效果图


## 代码
```
Shader "Practice/FixedFunction/AlphaTest"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        _AlphaLimit("AlphaLimit", float)=0
    }
    SubShader
    {
        Tags { "Queue"="Transparent" "RenderType"="Transparent" }
        LOD 100
        Blend SrcAlpha OneMinusSrcAlpha

        Pass
        {
            Lighting Off
            //关闭
            //AlphaTest Off
            //启用，超过或者等于该值才渲染到屏幕
            AlphaTest GEqual [_AlphaLimit]
            SetTexture [_MainTex]{combine texture}
        }
    }
}
```