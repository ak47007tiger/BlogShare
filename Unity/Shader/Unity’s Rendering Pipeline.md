# Unity’s Rendering Pipeline
- shaders定义了一个物体看起来怎么样，和光线交互的方法
- 在surface shaders中unity自动处理lighting, shadowing, lightmapping, forward vs. defered rendering

## Rendering Paths
- 使用Pass Tags中的设置根据Rendering Path来决定使用哪个Pass
- Forward Rendering, ForwardBase and ForwardAdd passes
- Deferred Shading, Deferred pass
- Legacy Deferred Lighting, PrepassBass and PrepassFinal
- Legacy Vertex Lit, Vertex, VertexLMRGBM and VertexLM pass
- 如果想要渲染Shadows或者DeapthTexture，ShadowCaster pass

## Forward Rendering path


## Deferred Shading path