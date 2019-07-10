# GPU Instancing 基础教程

## 介绍
- 用来一次性绘制大量小的mesh，减少drawcall数量
- 渲染那些一个drawdraw绘制一个，但是有不同参数的物体，比如颜色，缩放

## 和动态合批 Dynamic Batching的比较
- 动态合批需要使用相同的材质，如果使用的是相同的shader参数不同则不是同一个材质，会打破合批，导致性能下降
- GPU Instancing允许材质有不同参数

## 启用方法
- 如果一个shader支持GPU Instancing，那么在材质的Inspector面板最下方会有选项可以开启

## GPU Instancing的影响范围
- MeshRenderer
- Graphics.DrawMesh
- SkinnedMeshRenderer不被支持

## 编写支持GPU Instancing的Shader
- Unity自动支持Transform不同的物体，其它的数据则需要手动添加
- 添加新的Instance数据需要使用Instance宏
  - UNITY_INSTANCING_BUFFER_START
  - UNITY_DEFINE_INSTANCED_PROP
  - UNITY_INSTANCING_BUFFER_END
- 获取Instance数据的时候也要使用宏来获取
  - UNITY_ACCESS_INSTANCED_PROP

## 官网示例代码
```
Shader "Custom/InstancedColorSurfaceShader" {
    Properties {
        _Color ("Color", Color) = (1,1,1,1)
        _MainTex ("Albedo (RGB)", 2D) = "white" {}
        _Glossiness ("Smoothness", Range(0,1)) = 0.5
        _Metallic ("Metallic", Range(0,1)) = 0.0
    }

    SubShader {
        Tags { "RenderType"="Opaque" }
        LOD 200
        CGPROGRAM
        // Physically based Standard lighting model, and enable shadows on all light types
        #pragma surface surf Standard fullforwardshadows
        // Use Shader model 3.0 target
        #pragma target 3.0
        sampler2D _MainTex;
        struct Input {
            float2 uv_MainTex;
        };
        half _Glossiness;
        half _Metallic;
        UNITY_INSTANCING_BUFFER_START(Props)
           UNITY_DEFINE_INSTANCED_PROP(fixed4, _Color)
        UNITY_INSTANCING_BUFFER_END(Props)
        void surf (Input IN, inout SurfaceOutputStandard o) {
            fixed4 c = tex2D (_MainTex, IN.uv_MainTex) * UNITY_ACCESS_INSTANCED_PROP(Props, _Color);
            o.Albedo = c.rgb;
            o.Metallic = _Metallic;
            o.Smoothness = _Glossiness;
            o.Alpha = c.a;
        }
        ENDCG
    }
    FallBack "Diffuse"
}
```