# Surface Shader Examples

简单的例子，设置表面颜色为白色，使用内建的 Lambert 光照模型：

```glsl
Shader "Example/Diffuse Simple" {
    SubShader {
        Tags { "RenderType" = "Opaque" }
        CGPROGRAM
        #pragma surface surf Lambert
        struct Input {
            float4 color : COLOR;
        };
        void surf(Input IN, inout SurfaceOutput o) {
            o.Albedo = 1;
        }
        ENDCG
    }
    Fallback "Diffuse"
}
```

使用纹理:

```glsl
Shader "Example/Diffuse Texture" {
    Properties {
        _MainTex("Texture", 2D) = "white" {}
    }
    SubShader {
        Tags { "RenderType" = "Opaque" }
        CGPROGRAM
        #pragma surface surf Lambert
        struct Input {
            float2 uv_MainTex;
        };
        sampler2D _MainTex;
        void surf(Input IN, inout SurfaceOutput o) {
            o.Albedo = tex2D(_MainTex, IN.uv_MainTex).rgb;
        }
        ENDCG
    }
    Fallback "Diffuse"
}
```

添加法线贴图、添加边缘光、添加细节纹理、在屏幕空间添加细节纹理：
[TODO](http://docs.unity3d.com/Manual/SL-SurfaceShaderExamples.html)