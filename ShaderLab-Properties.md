# Properties
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