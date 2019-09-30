# Unity Shader 语义和vert向frag传值

## 语义
- 定义
  - 语义是一个在shader的输入或输出中说明参数的使用意图的字符串
  - 变量在shader不同阶段传递过程中需要语义
- 作用
  - 指明了输入输出的含义，指明希望unity填充什么数据到变量中，把数据填充到什么地方
  - 指明了变量的类型和适用意向

## Unity shader顶点数据支持的语义
- POSITION, float3 or float4
- NORMAL, float3
- TEXCOORD0, float2,float3,float4
- TEXCOORD1,TEXCOORD2,TEXCOORD3, 和上面一样
- TANGENT,float4
- COLOR,float4

## 当语义指定的数据填充不满声明的类型时，数据的默认值填充策略
- 比如 float4 uv1 : TEXCOORD0
  - 最后获得的uv1是 (x,y,0,1)
  - 即，没有数据的是0，但是w通道是1

## 案例代码
```
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
```

## 说明
- 在v2f中使用语义指明了要传递的数据的类型
- 除了特殊语义，如SV_POSITION指明像素位置，非特殊语义都是只要类型对，数据就可以传递
