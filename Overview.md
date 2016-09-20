Unity shader 跟渲染流水线的 shader 有很大不同。

Unity 的 shader 能够以以下三种方式编写：
- 作为 surface shaders
- 作为 vertex and fragment shaders
- 作为 fixed function shaders

不管采用哪种方式写 shader, 都需要封装在 ShaderLab 语言里。