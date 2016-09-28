# Properties

在 shader 里可以定义一系列参数，这些参数可以在 Unity 材质面板里进行设置。在 Properties 里可定义这些参数：

```glsl
Properties {
    Property
    [Property ...]
}
```

Property 的格式为：

```glsl
//  变量名    Inspector显示名称  变量类型     默认值
_AmbientColor ("Ambient Color", Color) = (1, 1, 1, 1)
```

具体的有下面这些形式：

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

| ShaderLab 属性类型 | CG 变量类型 | 功能 |
| - | - | - |
| Color | float4, half4, fixed4 | 色块，通过拾色器获取颜色 |
| Vector | float4, half4, fixed4 | |
| Range(min, max) | float, half, fixed | float 属性，以滑动条的形式调节 |
| Float | float, half, fixed | 非滑动条的 float 属性 |
| 2D | sampler2D | 2D 纹理。拖拽一个纹理 |
| Cube | samplerCube | 立方贴图，拖拽立方贴图 |
| 3D | sampler3D | 3D 纹理 |

Property 还有这样一些属性：
- [HideInInspector]: 在材质面板不显示该属性值
- [NoScaleOffset]: 在材质面板不显示纹理的 tiling 和 offset 属性
- [Normal]: 指示纹理需要一张法线贴图
- [HDR]: 指示纹理需要一张 HDR 纹理
- [Gamma]: 指示 float/vector 属性是一个 sRGB 值
- [PerRendererData]

以后再做了解。

Property 前面还可以加上 Enum 参数，以在 inspector 面板中显示单选下拉框，例如：

```glsl
[Enum(Metallic Alpha,0,Albedo Alpha,1)] _SmoothnessTextureChannel ("Smoothness texture channel", Float) = 0 
```

这样 inspector 里的 Smoothness texture channel 就会是由 Metallic Alpha 和 Albedo Alpha 两项组成的单选下拉框，分别表示0和1。