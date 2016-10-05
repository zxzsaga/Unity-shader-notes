# Pass

Pass 块渲染 GameObject 的几何体。

```glsl
Pass {
    [Name]
    [Tags]
    [RenderSetup]
}
```

可以使用 ShaderLab 的 UsePass 命令来使用其他 Unity Shader 中的 Pass:

```glsl
UsePass "MyShader/MYPASSNAME"
```

Unity 内部会把所有 Pass 的名称转换成大写字母的表示。

## Name and tags

Pass 可以定义它的名字和任意数量的 tags, 用于与渲染引擎的 Pass 通讯。

[tags](https://docs.unity3d.com/Manual/SL-PassTags.html) 有：

- LightMode: 定义该 Pass 在 Unity 的渲染流水线中的角色。
- PassFlags
- RequireOptions: 用于指定当满足某些条件时才渲染该 Pass, 它的值是一个由空格分隔的字符串。目前 Unity 支持的选项有：SoftVegetation. 在后面的版本中，可能会增加更多的选项。

除了上述普通 Pass 以外，Unity Shader 还支持一些特殊的 Pass, 以便进行代码复用或实现更复杂的效果。

- UsePass: 复用其他 Unity Shader 中的 Pass.
- GrabPass: 该 Pass 负责抓取屏幕并将结果存储到一张纹理中，以用于后续的 Pass 处理。

## Render state set-up

见 SubShader 的 RenderSetup
