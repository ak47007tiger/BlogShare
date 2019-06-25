# Scriptable Render Pipeline 可编程渲染管线
- 概述
  - 可以使用c#脚本定制渲染管线
  - 内置渲染和SRP不兼容
    - shader不共用，因为shader lib不同

## Pre-built SRPs
  - 概述
    - HDRP，LWRP
    - 可以和Shader Graph、Post-processing一起使用
    - HDRP和LWRP不兼容，并且同时和built-in Render Pipeline不兼容
- High Definition Render Pipeline 高清渲染管线
  - 需要高端硬件支持，如主机、pc。可用来做高真实度渲染
  - 使用compute shader技术，故需要gpu支持
  - 案例：AAA游戏、自动驾驶、建筑等
  - 使用基于物理的渲染，支持前向渲染和延迟渲染
  - 最新文档：https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@latest/index.html?preview=1
- Lightweight Render Pipeline 轻量渲染管线
  - 对硬件要求不高，可以用在移动平台，并且渲染质量高
  - 使用简化的基于物理的渲染
  - 使用单向前向渲染
  - 可以使用LWRP-VR来做VR功能，LWRP-VR和LWRP在预先做的设置上不同
  - 和内置shader不兼容，需要更新工程
  - 最新文档：https://docs.unity3d.com/Packages/com.unity.render-pipelines.lightweight@latest/index.html

## Setting up a Scriptable Render Pipeline
- Using a pre-built SRP with a new Project
  - 如果不需要定制，使用模板创建新工程
- Installing the latest SRP into an existing Project
  - 在已有工程里使用SRP
  - 使用Package Manager system下载资源，然后选择安装需要的资源
  - 转换成SRP耗时，手动设置的地方多
- Configuring and using a pre-built render pipeline 配置使用内置渲染管线
  - 使用内置渲染管线需要创建 Render Pipeline Asset
  - Creating Scriptable Render Pipeline Assets
    - Pipeline Assets中包含设置项，用于控制渲染质量
    - 创建渲染管线实例对象
      - 中间件资源
      - 渲染管线实现
    - 可以为不同平台和不同的测试环境创建不同的Pipline Assets
    - 因为使用不同的光照模型，HDRP和LWRP不兼容
    - Create>Render Pipeline
  - Configuring and using HDRP
    - **Edit>Project Settings** - **Player** - **Linear Color Space**
      - not support gamma lighting
    - 创建Render Pipeline后 在**Edit > Project Settings > Graphics**
    - 把创建的Pipeline Asset拖上去
    - 在Scriptable Render Pipeline folder之外存储创建的Pipeline Asset，保证HDRP settings和git上可以同步
  - Configuring and using LWRP
    - 类似上面的过程，换个名字
- Creating a custom SRP
  - clone github上的，然后直接使用
  - 创建新工程或者加到已有的工程
    - 把github的资源放到Assets根目录
    - 切换到兼容当前所用的Unity版本的分支
    - 使用命令"git submodule update --init"，为SRP添加子模块
    - 更新Project manifest/dependencies，例如：
      ```
      {
        "dependencies": {
            "com.unity.postprocessing": "file:../ScriptableRenderPipeline/com.unity.postprocessing",
          
            "com.unity.render-pipelines.core": "file:../ScriptableRenderPipeline/com.unity.render-pipelines.core",
          
            "com.unity.shadergraph": "file:../ScriptableRenderPipeline/com.unity.shadergraph",
          
            "com.unity.render-pipelines.lightweight": "file:../ScriptableRenderPipeline/com.unity.render-pipelines.lightweight"
          
        }
      }
      ```
    - 打开unity，可以开始debug和更改了
