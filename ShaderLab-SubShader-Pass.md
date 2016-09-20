# Pass
Pass 块渲染 GameObject 的几何体。

```
Pass {
    [Name]
    [Tags]
    [RenderSetup]
    [CGPROGRAM]
}
```

可以使用 ShaderLab 的 UsePass 命令来使用其他 Unity Shader 中的 Pass:

```
UsePass "MyShader/MYPASSNAME"
```

Unity 内部会把所有 Pass 的名称转换成大写字母的表示。

## Name and tags
Pass 可以定义它的名字和任意数量的 tags, 用于与渲染引擎的 Pass 通讯。

tags 有：

- LightMode: 定义该 Pass 在 Unity 的渲染流水线中的角色。
- PassFlags
- RequireOptions: 用于指定当满足某些条件时才渲染该 Pass, 它的值是一个由空格分隔的字符串。目前 Unity 支持的选项有：SoftVegetation. 在后面的版本中，可能会增加更多的选项。

除了上述普通 Pass 以外，Unity Shader 还支持一些特殊的 Pass, 以便进行代码复用或实现更复杂的效果。

- UsePass: 复用其他 Unity Shader 中的 Pass.
- GrabPass: 该 Pass 负责抓取屏幕并将结果存储到一张纹理中，以用于后续的 Pass 处理。

## Render state set-up


Pass 设置图形硬件的多种状态，比如是否开启 alpha 混合、是否开启深度测试。有下面这些命令

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

  ```
  Blend sourceBlendMode destBlendMode
  Blend sourceBlendMode destBlendMode, alphaSourceBlendMode alphaDestBlendNode
  BlendOp colorOp
  BlendOp colorOp, alphaOp
  AlphaToMask On | Off
  ```

  开启并设置混合模式。

  混合因子有以下这些:

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

- ColorMask