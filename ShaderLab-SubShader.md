# SubShader
每个 Unity Shader 文件至少要有一个 SubShader. Unity 要显示 mesh 时，就会去寻找要使用的 shader. 第一个被支持的 SubShader 将被选择使用，如果没有 shader 可用，Unity 会尝试使用 fallback shader.

```
SubShader {
    [Tags]
    [CommonState(RendeSetup)]
    Pass
    [Pass ...]
}

```

SubShader 和 Pass 里都可以设置 tag, SubShader 的 tag 将会作用于所有的 Pass.

Shader "LOD(level of detail)" 和 "shader replacement" 是两个建立于 subshaders 之上的技术。

SubShader 定义一列 rendering passes. 这些 pass 可以是 regular pass, use pass, grab pass.
