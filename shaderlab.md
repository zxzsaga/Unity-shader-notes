# ShaderLab
Unity 的 shader 文件使用一种名为 ShaderLab 的声明式语言编写。实际的 shader 代码使用 HLSL/Cg 语言编写，写在 `CGPROGRAM` 片段里。

"Shader" 是 shader 文件的根命令，每个 shader 文件必须定义一个 "Shader".

```
Shader "name" {
    [Properties]
    Subshaders
    [Fallback]
    [CustomEditor]
}
```

## Properties
在 shader 里可以定义一系列参数，这些参数可以在 Unity 材质面板里进行设置。在 Properties 里可定义这些参数：

```
Properties {
    Property
    [Property ...]
}
```

每个 Property 有下面这些形式：

- `name("display name", Range(min, max)) = number`
- `name("display name", Float) = number`
- `name("display name", Int) = number`
- `name("display name", Color) = (number, number, number, number)`
- `name("display name", Vector) = (number, number, number, number)`
- `name("display name", 2D) = "defaulttexture" {}`
- `name("display name", Cube) = "defaulttexture" {}`
- `name("display name", 3D) = "defaulttexture" {}`

`name` 一般以下划线开始。"defaulttexture" 要么是空字符串，要么是内建默认纹理："white", "black", "gray", "bump".

每个 Property 需要在 CG 中有对应的变量，它们名称必须相同，类型有如下的对应关系：

- Color, Vector: float4, half4, fixed4
- Range, Float: float, half, fixed
- 2D: sampler2D
- Cube: samplerCube
- 3D: sampler3D

Property 还有这样一些属性：

- [HideInInspector]: 在材质面板不显示该属性值
- [NoScaleOffset]: 在材质面板不显示纹理的 tiling 和 offset 属性
- [Normal]: 指示纹理需要一张法线贴图
- [HDR]: 指示纹理需要一张 HDR 纹理
- [Gamma]
- [PerRendererData]

以后再做了解。

## SubShaders
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

详细的内容，见 *ShanderLab SubShader* 笔记。

## Fallback
Fallback 的作用是指明：如果没有 subshader 在硬件上可以运行，那么使用另一个 shader.

```
Fallback "name"
```

或者

```
Fallbacl Off
```

## CustomEditor
自定义材质面板。

## Other command
Category: 对 Shader 的元素进行分组。