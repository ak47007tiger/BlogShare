# Unity 反射shader

## 效果图


## 涉及的知识
- reflection probe，bake场景为cubemap的组件
- cubemap, 一种纹理格式，用于天空盒子
- 基础的shader知识


## 反射shader
```
Shader "DC/Lighting/Reflect"
{
    Properties
    {
        _Environment ("Environment", Cube) = "defaulttexture" {}
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 100

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            #include "UnityCG.cginc"

            struct appdata
            {
                float4 vertex : POSITION;
            };

            struct v2f
            {
                float4 vertex : SV_POSITION;
                float4 worldDir : TEXCOORD1;
            };

            samplerCUBE _Environment;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.worldDir = normalize(mul(unity_ObjectToWorld, v.vertex));
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 col = texCUBE(_Environment, i.worldDir.xyz);
                return col;
            }
            ENDCG
        }
    }
}
```