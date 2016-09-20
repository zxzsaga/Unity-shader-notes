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