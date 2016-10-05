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
