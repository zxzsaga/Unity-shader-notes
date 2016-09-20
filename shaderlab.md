# ShaderLab
Unity 的 shader 文件使用一种名为 ShaderLab 的声明式语言编写。实际的 shader 代码使用 HLSL/Cg 语言编写，写在 `CGPROGRAM` 片段里。

"Shader" 是 shader 文件的根命令，每个 shader 文件必须定义一个 "Shader".

```
Shader "name" {
    [Properties]
    SubShader
    [SubShader ...]
    [Fallback]
    [CustomEditor]
}
```
