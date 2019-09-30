## multi_compile
- 作用
  - 根据编译选项产生shader变体
  - 避免分支语句导致的性能下降
  - 主要用于在代码中选择shader变体
- Unity在打包时会把所有multi_compile产生的shader变体都打进包中

## 效果图

## 示例shader
```
Shader "DC/Shader/ShaderLab/MultiCompile"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        [KeywordEnum(R,G,B)] _CL("ColorSelect", Float) = 0
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
            
            #pragma multi_compile _CL_R _CL_G _CL_B
            //使用 __ 减少一个编译选项，编译选项最多256个
            #pragma multi_compile __ DB_ON

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
                #if DB_ON
                    return fixed4(1,1,0,1);
                #elif _CL_R
                    return fixed4(1,0,0,1);
                #elif _CL_G
                    return fixed4(0,1,0,1);
                #elif _CL_B
                    return fixed4(0,0,1,1);
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

namespace DC
{
  public class MultiCompile : MonoBehaviour
  {
    public Material mat;

    public void OnEnable()
    {
      mat.EnableKeyword("DB_ON");
//      Shader.EnableKeyword("ON");
    }

    public void OnDisable()
    {
      mat.DisableKeyword("DB_ON");
//      Shader.DisableKeyword("DB_ON");
    }
  }
}
```

