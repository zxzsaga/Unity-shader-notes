# SubShader
每个 Unity Shader 文件至少要有一个 SubShader. Unity 要显示 mesh 时，就会去寻找要使用的 shader. 第一个被支持的 SubShader 将被选择使用，如果没有 shader 可用，Unity 会尝试使用 fallback shader.

```
SubShader {
    [Tags]
    [CommonState(RendeSetup)]
    Pass
    [Pass ...]
}

```

SubShader 和 Pass 里都可以设置 tag, SubShader 的 tag 将会作用于所有的 Pass.

Shader "LOD(level of detail)" 和 "shader replacement" 是两个建立于 subshaders 之上的技术。

SubShader 定义一列 rendering passes. 这些 pass 可以是 regular pass, use pass, grab pass.

## Tags
Tags 用来指示渲染引擎如何以及何时渲染。

```
Tags {
    "TagName1" = "Value1"
    "TagName2" = "Value2"
    ...
}

```

Tags 需要在 `SubShader` 段里，但不能在 `Pass` 段里。

### Queue tag - Rendering Order
使用 Queue tag 来决定对象的渲染顺序。通过这种方式可以保证透明物体在所有不透明物体渲染之后渲染。预设的 queues 有这些：

- Background(1000): 这个渲染队列会在其他队列之前渲染。典型用例是渲染背景。
- Geometry(2000): 默认队列。用于大多数对象。不透明几何体使用这个队列。
- AlphaTest(2450): 需要 alpha 测试的几何体使用这个队列。这个队列从 Geometry 队列分离出来，因为在渲染完不透明对象之后，再渲染 alpha 测试对象，会更有效率。
- Transparent(3000): 这个渲染队列在 Geometry 和 AlphaTest 之后渲染，以**从后往前**的顺序渲染。任何 alpha-blended(例如不写深度缓冲的 shader) 应该使用这个队列（glass, particle effects）。
- Overlay(4000): 这个渲染队列用来做叠加效果, 最后渲染的对象应该使用这个队列（lens flares）。

可以使用这些 tags 以外的自定义 tag，如 `Geometry+1`

### RenderType tag
RenderType tag 将 shader 分类到预设的几个组里。用于 [Shader Replacement](http://docs.unity3d.com/Manual/SL-ShaderReplacement.html) 和有时制作 [camera's depth texture](http://docs.unity3d.com/Manual/SL-CameraDepthTexture.html).

### DisableBatching tag
一些 SubShader 在使用 Unity 的批处理功能时会出现问题，例如使用了模型空间下的坐标进行顶点动画。这是可以通过该标签来直接指明是否对该 SubShader 使用批处理。

### ForceNoShadowCasting tag
控制使用该 SubShader 的物体是否会投射阴影。

### IgnoreProjector tag
如果这个标签为"True", 使用这个 SubShader 的几何体将不会受 Projector 的影响。通常用于半透明物体。

### CanUseSpriteAtlas tag
当该 SubShader 是用于精灵（sprites）时，将该标签设为"False".

### PreviewType tag
指明材质面板如何预览该材质。默认情况下，材质显示为一个球，可以把该标签设为"Plane", "SkyBox"来改变预览类型。
