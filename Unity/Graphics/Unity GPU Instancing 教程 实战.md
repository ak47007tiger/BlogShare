# Unity GPU Instancing 教程 实战

## 实战
- 使用GPU Instancing减少DrawCall
- 编写支持GPU Instancing的Unlit Shader
- 使用MaterialPropertyBlock改变材质属性

## 分析
- 开启前
- 开启后
- 结论：gpu instancing可以在材质属性不同时减少draw call，弥补了动态合批的不足

## Mono脚本
- 脚本的操作：每帧使用MaterialPropertyBlock改变材质属性
```
using UnityEngine;

public class GPUInstancing : MonoBehaviour
{
    public Vector2 center = new Vector2(0.5f,0.5f);

    private MaterialPropertyBlock propertyBlock;

    private static int id_center_x = Shader.PropertyToID("_CenterX");

    private static int id_center_y = Shader.PropertyToID("_CenterY");

    void Start()
    {
        propertyBlock = new MaterialPropertyBlock();
    }

    void Update()
    {
        propertyBlock.SetFloat(id_center_x, center.x);
        propertyBlock.SetFloat(id_center_y, center.y);
        GetComponent<Renderer>().SetPropertyBlock(propertyBlock);
    }
}
```

## Shader代码
- 本shader设置了一个中心，越靠近这个中心颜色越透明，形状像十字星
```
Shader "Unlit/DifferentCenter"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
		_CenterX("CenterX",float) = 0.5
		_CenterY("CenterY",float) = 0.5
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" "Queue"="Transparent" }
        LOD 100
		Blend SrcAlpha OneMinusSrcAlpha

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
			#pragma multi_compile_instancing

            #include "UnityCG.cginc"

            struct appdata
            {
                float4 vertex : POSITION;
                float2 uv : TEXCOORD0;
				UNITY_VERTEX_INPUT_INSTANCE_ID
            };

            struct v2f
            {
                float2 uv : TEXCOORD0;
                UNITY_FOG_COORDS(1)
                float4 vertex : SV_POSITION;
				UNITY_VERTEX_INPUT_INSTANCE_ID
            };

			UNITY_INSTANCING_BUFFER_START(Props)
				UNITY_DEFINE_INSTANCED_PROP(float, _CenterX)
				UNITY_DEFINE_INSTANCED_PROP(float, _CenterY)
			UNITY_INSTANCING_BUFFER_END(Props)

            sampler2D _MainTex;
            float4 _MainTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
				UNITY_SETUP_INSTANCE_ID(v);
				UNITY_TRANSFER_INSTANCE_ID(v, o);
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                UNITY_TRANSFER_FOG(o,o.vertex);
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
				UNITY_SETUP_INSTANCE_ID(i);
                fixed4 col = tex2D(_MainTex, i.uv);
				float cx = UNITY_ACCESS_INSTANCED_PROP(Props, _CenterX);
				float cy = UNITY_ACCESS_INSTANCED_PROP(Props, _CenterY);
				float2 offset = abs(i.uv - float2(cx, cy));
                return fixed4(col.rgb,offset.x + offset.y);
            }
            ENDCG
        }
    }
}
```