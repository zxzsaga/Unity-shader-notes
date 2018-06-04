设置图形硬件的多种状态，比如是否开启 alpha 混合、是否开启深度测试。有下面这些命令：

- Cull: `Cull Back | Front | Off`, 多边形剔除模式
    - Back: 默认选项，剔除（不渲染）背向观察者的多边形。
    - Front: 剔除面向观察者的多边形。
    - Off: 不剔除。
- ZWrite: `ZWrite On | Off`, 是否开启深度缓冲写入
    - On: 默认选项，渲染实体对象时应该启用
    - Off: 渲染半透明效果时应该启用
- ZTest: `ZTest (Less | Greater | LEqual | GEqual | Equal | NotEqual | Always)`, 深度测试时使用的函数
- Offset: `Offset OffsetFactor, OffsetUnits`
- Blend:

  ```glsl
  Blend sourceBlendMode destBlendMode
  Blend sourceBlendMode destBlendMode, alphaSourceBlendMode alphaDestBlendNode
  BlendOp colorOp
  BlendOp colorOp, alphaOp
  AlphaToMask On | Off
  ```

  开启并设置混合模式。混合因子有以下这些:
    - One: 1
    - Zero: 0
    - SrcColor: 使用源颜色 RGB 分量作为混合因子
    - SrcAlpha: 使用源颜色的 alpha
    - DstColor
    - DstAlpha
    - OneMinusSrcColor
    - OneMinusSrcAlpha
    - OneMinusDstColor
    - OneMinusDstAlpha
- ColorMask: ColorMask可以让我们制定渲染结果的输出通道，而不是通常情况下的RGBA这4个通道全部写入。可选参数是 RGBA 的任意组合以及 0， 这将意味着不会写入到任何通道，可以用来单独做一次Z测试，而不将结果写入颜色通道。

## LOD

在Unity的Quality Settings中我们可以设定允许的最大LOD，当设定的LOD小于SubShader所指定的LOD时，这个SubShader将不可用。

Unity内建Shader定义了一组LOD的数值：

- VertexLit及其系列 = 100
- Decal, Reflective VertexLit = 150
- Diffuse = 200
- Diffuse Detail, Reflective Bumped Unlit, Reflective Bumped VertexLit = 250
- Bumped, Specular = 300
- Bumped Specular = 400
- Parallax = 500
- Parallax Specular = 600