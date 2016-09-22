# Surface Shader

编写与光照结合的 shader 很复杂。光照有不同的光照类型、阴影选项、渲染路径(forward and deferred rendering), shader 需要以某些方式处理这些和复杂性。

Unity 的 surface shader 是一种代码生成方法，使我们不用编写低层次的 vertex/pixel shader 程序。我们需要使用 Cg/HLSL 来编写。Surface shader 是 Unity 自己创造的一种着色器代码类型，它在背后仍旧会转换成对应的顶点/片元着色器，是对顶点/片元着色器的更高一层抽象。

Surface shader 编译后指出需要哪些输入、哪些输出被填充等等，生成实际的 vertex&pixel shader, 同时生成处理 forward 和 deferred 渲染的渲染路径。

## 如何使用

Surface shader 跟其他 shader 一样，需要写在 CGPROGRAM..ENDCG 段里。不同的是：

- Surface shader 写在 SubShader 里，不在 Pass 里。Surface shader 自己会编译进多个 Pass 里。
- 需要使用

  ```glsl
  #pragma surface surfaceFunction lightModel [optionalparams]
  ```

  语法，声明一个 surface shader, 并在随后对这个 surfaceFunction 进行定义.

### surfaceFunction

surfaceFunction 的定义为：

  ```glsl
  void surf(Input IN, inout SurfaceOutput o)
  ```

- 输入结构 Input 是一个需要自定义的结构，Input 需要包含所有 texture 坐标和额外 surface function 需要的变量。
- 输出结构是内建的，有 SurfaceOutput, SurfaceOutputStandard, SurfaceOutputStandardSpecular 等。

#### 输入结构

输入结构 `Input` 通常包含所有 shader 需要的 texture coordinates. 这些 texture coordinates 的名字必须以 `uv` 开头，再加上 texture 名字（或者以 `uv2` 开头，使用 secend texture coordinate set）。

其他可以放入 `Input` 的参数有：

- `float3 viewDir`: view 方向，用于计算视察效果(Parallax effects), 边缘光(rim lighting)等。
- `COLOR` 语义的 `float4`: 每像素的颜色插值。
- `float4 screenPos`: 屏幕空间位置，用于反射或屏幕空间效果。不适用于 [GrabPass](http://docs.unity3d.com/Manual/SL-GrabPass.html); 需要使用 `ComputerGrabScreenPos` 计算自定义的 UV.
- `float3 worldPos`: 世界空间位置。
- `float3 worldRefl`: world reflection vector, 如果 surface shader 没有写 o.Normal.
- `float3 worldNormal`: world normal vector, 如果 surface shader 没有写 o.Normal.
- `float3 worldRefl; INTERNAL_DATA`: world reflection vector, 如果 surface shader 写了 o.Normal.
- `float3 worldNormal; INTERNAL_DATA`: world normal vector, 如果 surface shader 写了 o.Normal.

#### 输出结构

SurfaceOutput 描述表面属性（比如 albedo color, normal, emission, specularity 等等）。

SurfaceOutput 的结构如下：

```glsl
struct SurfaceOutput {
    fixed3 Albedo;  // diffuse color
    fixed3 Normal;  // tangent space normal, if written
    fixed3 Emission;
    half Specular;  // specular power in 0..1 range
    fixed Gloss;    // specular intensity
    fixed Alpha;    // alpha for transparencies
};
```

Unity 5 里 surface shader 还可以用 physically based lighting models. 内建的 Standard 和 StardardSpecular 光照模型分别可以使用输出结构 SurfaceOutputStandard 和 SurfaceOutputStandardSpecular:

```glsl
struct SurfaceOutputStandard {
    fixed3 Albedo;      // base (diffuse or specular) color
    fixed3 Normal;      // tangent space normal, if written
    half3 Emission;
    half Metallic;      // 0=non-metal, 1=metal
    half Smoothness;    // 0=rough, 1=smooth
    half Occlusion;     // occlusion (default 1)
    fixed Alpha;        // alpha for transparencies
};

struct SurfaceOutputStandardSpecular {
    fixed3 Albedo;      // diffuse color
    fixed3 Specular;    // specular color
    fixed3 Normal;      // tangent space normal, if written
    half3 Emission;
    half Smoothness;    // 0=rough, 1=smooth
    half Occlusion;     // occlusion (default 1)
    fixed Alpha;        // alpha for transparencies
};
```

### lightModel

lightModel: 内建的 lightModel 有 physically based Standard 和 StandardSpecular，简单的 non-physically based Lambert(diffuse) 和 BlinnPhong(specular).
- Standard 光照模型使用 SurfaceOutputStandar 输出结构，匹配 Unity 的 Standard(metallic workflow) shader.
- StandardSpecular 光照模型使用 SurfaceOutputStandardSpecular 输出结构，匹配 Unity 的 Standard(specular setup) shader.
- Lambert 和 BlinnPhong 光照模型不是 physically based, 但在低端硬件上渲染更快。

### optionalparams

**transparency and alpha testing** 用 alpha 和 alphatest 准则控制。

transparency 分为两种：

* traditional alpha blending(use for fading objects out)
* 物理上更真实的 "premultiplied blending"(which allows semitransparent surfaces to retain proper specular reflections).

启用 semitransparency 使得生成的 surface shader 代码包含 blending 命令；然而启用 alpha cutout 会基于给定变量，在生成 pixel shader 时 do a fragment discard.

[TODO](http://docs.unity3d.com/Manual/SL-SurfaceShaders.html)


