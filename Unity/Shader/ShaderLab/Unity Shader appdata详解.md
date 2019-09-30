# Unity Shader appdata详解

## 内置shader代码
```
struct appdata_base {
    float4 vertex : POSITION;//顶点位置
    float3 normal : NORMAL;//发现
    float4 texcoord : TEXCOORD0;//纹理坐标
    UNITY_VERTEX_INPUT_INSTANCE_ID
};

struct appdata_tan {
    float4 vertex : POSITION;
    float4 tangent : TANGENT;//切线
    float3 normal : NORMAL;
    float4 texcoord : TEXCOORD0;
    UNITY_VERTEX_INPUT_INSTANCE_ID
};

struct appdata_full {
    float4 vertex : POSITION;
    float4 tangent : TANGENT;
    float3 normal : NORMAL;
    float4 texcoord : TEXCOORD0;
    float4 texcoord1 : TEXCOORD1;//第二纹理坐标
    float4 texcoord2 : TEXCOORD2;//第三纹理坐标
    float4 texcoord3 : TEXCOORD3;//第四纹理坐标
    fixed4 color : COLOR;//顶点颜色
    UNITY_VERTEX_INPUT_INSTANCE_ID
};
```

## 自定义一个顶点数据
```
struct appdata
{
    float4 vertex : POSITION;//获得顶点数据
    float2 uv : TEXCOORD0;//获得纹理坐标数据
    float4 normal: NORMAL;//获得法线数据
};
```

## 完整代码
```
Shader "Practice/Syntax/Sematics"
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
                float4 normal: NORMAL;
            };

            struct v2f
            {
                float2 uv : TEXCOORD0;
                // float4 vertex : SV_POSITION;
                float4 vertex : SV_POSITION;
                // float4 worldNormal : TEXCOORD1;
                // float4 worldNormal : COLOR;
                // float4 worldNormal : TEXCOORD11;
                float4 worldNormal : TANGENT;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                o.worldNormal = mul(unity_ObjectToWorld,v.normal);
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                return fixed4(i.worldNormal.xyz, 1);
                fixed4 col = tex2D(_MainTex, i.uv);
                return col;
            }
            ENDCG
        }
    }
}

```