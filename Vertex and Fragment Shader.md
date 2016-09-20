如果要编写与光照交互的 shader, 请使用 surface shader.

底层的 shader 程序使用 Cg 或者 HLSL 语言编写，写在 Pass 命令里。典型代码如下：
```
Pass {
    // ... the usual pass state setup ...

    CGPROGRAM
    // compilation directives for this snippet, e.g.:
    #pragma vertex vert
    #pragma fragment frag

    // ths Cg/HLSL code itself

    ENDCG
    // ... the rest of pass setup ...
}
```

Cg/HLSL 代码写在 `CGPROGRAM` 和 `ENDCG` 之间。

在代码开始处可以编译由 `#pragma` 语句设定的准则。这些准则指示编译哪些 shader 函数。
- `#pragma vertex name`
- `#pragma fragment name`
- `#pragma geometry name`: DX10的几何着色器，自动开启 `#pragma target 4.0`
- `#pragma hull name`: DX11的 hall shader, 自动开启 `#pragma target 5.0`
- `#pragma domain name`: DX11的 domain shader, 自动开启 `#pragma target 5.0`

其他的还有：
- `#pragma target name`
- `#pragma only_renderers`
- `#pragma exclude_renderers`
- `#pragma multi_compile`
- `#pragma enable_d3d11_debug_symbols`
- `#pragma hardware_tier_variants`

每段代码必须要包含一个 `#pragma vertex` 和一个 `#pragma fragment`

## Rendering platforms
Unity 默认编译至所有的渲染平台，可以使用 `#pragma only_renderers` 和 `#pragma exclude_renderers` 来控制编译的渲染平台。支持的平台有这些：
- d3d9
- d3d11
- glcore
- gles
- gles3
- metal
- d3d11_9x
- xbox360
- xboxone
- ps4
- psp2: psv
- n3ds
- wiiu

## Unity 提供的内置文件和变量
包含文件可以这样：

```
CGPROGRAM
// ...
#include "UnityCG.cginc"
// ...
ENDCG
```

常用的包含文件有：

- UnityCG.cginc: 包含了最常使用的帮助函数、宏和结构体等。
- UnityShaderVariables.cginc: 在编译 Unity Shader 时，会被自动包含进来。包含了许多内置的全局变量，如 UNITY_MATRIX_MVP 等。
- Lighting.cginc: 包含了各种内置的光照模型，如果编写的是 Surface Shader 的话，会自动包含进来。
- HLSLSupport.cginc: 在编译 Unity Shader 时，会被自动包含进来。声明了很多用于跨平台编译的宏和定义。

UnityCG.cginc 是最常接触的一个包含文件，其中常用的结构体有：

- appdata_base: 可用于顶点着色器的输入。顶点位置、顶点法线、第一组纹理坐标。
- appdata_tan: 可用于顶点着色器的输入。顶点位置、顶点切线、顶点法线、第一组纹理坐标。
- appdata_full: 可用于顶点着色器的输入。顶点位置、顶点切线、顶点法线、四组（或更多）纹理坐标。
- appdata_img: 可用于顶点着色器的输入。顶点位置、第一组纹理坐标。
- v2f_img: 可用于顶点着色器的输出。裁剪空间中的位置、纹理坐标。

UnityCG.cginc 中还有一些常用的帮助函数：

- `float3 WorldSpaceViewDir(float4 v)`
- `float3 ObjSpaceViewDir(float4 v)`
- `float3 WorldSpaceLightDir(float4 v)`
- `float3 ObjSpaceLightDir(float4 v)`
- `float3 UnityObjectToWorldNormal(float3 norm)`
- `float3 UnityObjectToWorldDir(in float3 dir)`
- `float3 UnityWorldToObjectDir(float3 dir)`
