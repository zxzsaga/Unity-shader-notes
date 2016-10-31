# ShaderLab
Unity 的 shader 文件使用一种名为 ShaderLab 的声明式语言编写。实际的 shader 代码使用 HLSL/Cg 语言编写，写在 `CGPROGRAM` 片段里。

"Shader" 是 shader 文件的根命令，每个 shader 文件必须定义一个 "Shader", 其结构为：

```glsl
Shader "name" {
    [Properties]
    [CGINCLUDE]
    SubShader
    [SubShader ...]
    [Fallback]
    [CustomEditor]
}
```

一个典型的例子如下：

```glsl
Shader "MyShaderName" {
    Properties {
        // 属性
    }

    SubShader {    // 针对显卡 A 的 SubShader
        Pass {
            // 设置渲染状态和标签

            CGPROGRAM    // 开始 CG 代码片段
            #pragma vertex vert
            #pragma fragment frag

            // CG 代码

            ENDCG

            // 其他设置
        }
        // 其他需要的 Pass
    }

    SubShader {
        // 针对显卡 B 的 SubShader
    }

    // 上述 SubShader 都失败后用于回调的 Unity Shader
    Fallback "VertexLit"
}
```
