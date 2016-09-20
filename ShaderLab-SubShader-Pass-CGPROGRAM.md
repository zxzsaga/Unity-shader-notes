# CGPROGRAM
一段 CG 程序的例子是：

```
CGPROGRAM

#pragma vertex vert      // 指定顶点着色器
#pragma fragment frag    // 指定片元着色器

// 使用一个结构体来定义顶点着色器的输入
struct a2v {
    // POSITION 语义告诉 Unity 用模型空间的顶点坐标填充 vertex 变量
    float4 vertex : POSITION;
    // NORMAL 语义告诉 Unity 用模型空间的法线方向填充 normal 变量
    float3 normal : NORMAL;
    // TEXCOORD0 语义告诉 Unity 用模型的第一套纹理坐标填充 texcoord 变量
    float4 texcoord : TEXCOORD0;
}

// 使用一个结构体来定义顶点着色器的输出
struct v2f {
    // SV_POSITION 语义告诉 Unity, pos 里包含了顶点在裁剪空间中的位置信息
    float4 pos : SV_POSITION;
    // COLOR0 语义可以用于存储颜色信息
    fixed3 color : COLOR0;
}

v2f vert(a2v v) : SV_POSITION {
    v2f o;
    o.pos = mul(UNITY_MATRIX_MVP, v.vertex);
    o.color = v.normal * 0.5 + fixed3(0.5, 0.5, 0.5);
    return o;
}

fixed4 frag(v2f i) : SV_Target {
    return fixed4(i.color, 1.0);   
}

ENDCG
```

`POSITION`, `NORMAL` 这些语义中的数据，是由 Material 的 Mesh Renderer 组件，在每帧调用 Draw Call 时提供的。

如果在 SubShader 的 Properties 中定义了属性，在 CG 代码中需要定义一个与属性名称和类型都匹配的变量，这样才能使用：

```
Properties {
    _Color("Color Tint", Color) = (1.0, 1.0, 1.0, 1.0)
}
SubShader {
    Pass {
        CGPROGRAM

        fixed4 _Color;

        ENDCG
    }
}

```

## Unity 提供的内置文件和变量
CG 程序中可以使用 `#include` 包含 `.cginc` 类型的文件，以获得一些很有用的变量和函数。例如：

```
CGPROGRAM
#include "UnityCG.cginc"
ENDCG
```

Unity 的内置着色器分布在这些文件夹下：
- CGIncludes: 内置包含文件
- DefaultResources: 内置组件或功能所需要的 Unity Shader, 例如一些 GUI 元素使用的 Shader
- DefaultResourcesExtra: 内置 Unity Shader
- Editor

常用的包含文件有：

| 文件名 | 描述 |
| - | - |
| UnityCG.cginc | 包含了最常使用的帮助函数、宏和结构体等 |
| UnityShaderVariables.cginc | 在编译 Unity Shader 时，会被自动包含进来。包含了许多内置的全局变量，如 UNITY_MATRIX_MVP 等 |
| Lighting.cginc | 包含了各种内置的光照模型，如果编写的是 Surface Shader 的话，会自动包含进来 |
| HLSLSupport.cginc | 在编译 Unity Shader 时，会被自动包含进来。声明了很多用于跨平台编译的宏和命令 |

TODO: P108