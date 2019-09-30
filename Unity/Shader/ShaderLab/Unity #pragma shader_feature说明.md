## shader_feature
- 作用
  - 根据编译选项产生shader变体
  - 避免分支语句导致的性能下降
  - 主要用于材质选项上
- unity打包时如果发现没有材质引用shader_feature产生的变体，不会打包该变体
## 效果图

## 示例shader
```
Shader "DC/Shader/ShaderLab/MaterialToggle"
{
    Properties
    {
        [Toggle] _T1("T1", Float)=0
        [Toggle(T2)] _T2("T2", Float)=0
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

            #pragma shader_feature _T1_ON
            #pragma shader_feature T2

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
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                UNITY_TRANSFER_FOG(o,o.vertex);
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                #ifdef _T1_ON
                    fixed b = 0;
                    #ifdef T2
                        b = 0.5;
                    #endif
                    return fixed4(0.5,b,0,1);
                #else
                    fixed4 col = tex2D(_MainTex, i.uv);
                    return col;
                #endif
            }
            ENDCG
        }
    }
}

```

## 示例脚本
```
using UnityEngine;

public class ShaderFeatureControl : MonoBehaviour
{
  public Material _materialToggle;

  void OnEnable()
  {
    _materialToggle.EnableKeyword("T2");
  }

  void OnDisable()
  {
    _materialToggle.DisableKeyword("T2");
  }
}
```