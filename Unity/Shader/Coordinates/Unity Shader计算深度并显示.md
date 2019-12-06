## 概述
- 本文介绍在shader自己计算深度并且显示的方法

## 效果图
- 接近金剪裁面的深度小所以接近黑色，接近远剪裁面的深度大所以接近白色

## 源码
```
Shader "DC/Shader/ProjectionParams"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
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
                float2 uv : TEXCOORD0;
            };

            struct v2f
            {
                float2 uv : TEXCOORD0;
                float4 vertex : SV_POSITION;
                float viewSpaceDepth : TEXCOORD1;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                //计算[0,1]范围的深度值
                // o.viewSpaceDepth = COMPUTE_DEPTH_01;
                //下面是COMPUTE_DEPTH_01的展开源码
                o.viewSpaceDepth = -mul(UNITY_MATRIX_MV, v.vertex).z;
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                // fixed c = i.viewSpaceDepth;
                fixed c = i.viewSpaceDepth / _ProjectionParams.z;
                // fixed c = i.viewSpaceDepth / 30;
                // fixed c = i.viewSpaceDepth;
                return fixed4(c,c,c,1);
            }
            ENDCG
        }
    }
}

```