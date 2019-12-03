# 第3章 unity shader基础

当没有unity时，直接使用图像编程接口，将会需要类似下面的伪代码：

略

（我并不关心Unity之外的体系是如何处理Shader加载、运行的，应该聚焦于Unity Shader自身。）

## 3.1 unity shader 概述

### 3.1.1 一对好兄弟：材质与shader

在unity中，unity shader实例化后，成为材质。在材质上可以调整该unity shader定义的各种纹理、属性等参数。

### 3.1.2 unity中的材质

unity为材质提供了可视化的调节面板，使得开发者不再需要在代码中设置和改变渲染所需的各种参数。

### 3.1.3 unity中的shader

unity shader需要与材质结合使用。新建的材质，更改shader后，材质面板中就会显示该shader的各种属性，例如颜色、纹理、浮点数、滑动条、向量等。

## 3.2 unity shader的基础：shaderlab语言

unity shader是Unity提供的高层级的渲染抽象层。通过这种方式让开发者能更加轻松地控制渲染。

![图3.6](images/chapter03_difference_between_unity_shader_and_raw_graphics_api.svg)

左图为没有使用unity shader的情况。开发者需要和很多文件和设置打交道。右图为在unity shader的帮助下，开发者只需要使用shaderlab来编写unity shader即可完成所有的工作。

在unity中，所有的unity shader都是使用shaderlab来编写的。shaderlab是unity提供的一种说明性语言。它使用了一些嵌套在花括号内部的语法来描述一个unity shader文件的结构。这些结构包含了许多渲染所需要的数据。从设计上来说，shaderlab类似于cgfx和direct3d effects语言。它们都定义了要显示一个材质所需的所有东西，而不仅仅是着色器代码。

一个unity shader的基础结构如下所示：

```shaderlab
    Shader "ShaderName" {
        Properties {
            //属性定义
        }
        SubShader {
            //显卡A使用的子着色器
        }
        SubShader {
            //显卡B使用的子着色器
        }
        Fallback "VertexLit"
    }
```

unity会在背后根据使用的平台把这些结构编译成真正的代码和shader文件，而开发者只需要和unity shader打交道即可。

## 3.3 Unity shader的结构

### 3.3.1 shader的名字

shader的名字可以含有斜杠，用以分类。

```shaderlab
    Shader "Custom/MyShader" {

    }
```

### 3.3.2 Properties属性定义

Properties语法如下：

```shaderlab
    Properties {
        propertyName ("displayName", PropertyType) = DefaultValue
    }
```

- propertyName通常以下划线开头，用于在shader代码中访问。
- displayName是用于在材质面板上显示的名称。
- 属性有类型。
- 属性可以设定默认值。

表3.1 Properties语句支持的属性类型

属性类型 | 默认值语法 | 例子
:- | :- | :- |
Int | number | _Int("Int", Int) = 2
Float | number | _Float("Float", Float) = 1.5
Range | number | _Range("Range", Range(0.0, 5.0)) = 3.0
Color | (r, g, b, a) | _Color("Color", Color) = (1.0, 1.0, 1.0, 1.0)
Vector | (x, y, z, w) | _Vector("Vector", Vector) = (1.0, 2.0, -3.0, 4.0)
2D | "默认纹理名称" {} | _2D("2D", 2D) = "" {}
2D | "默认纹理名称" {} | _2D("2D", 2D) = "white" {}
2D | "默认纹理名称" {} | _2D("2D", 2D) = "black" {}
2D | "默认纹理名称" {} | _2D("2D", 2D) = "gray" {}
2D | "默认纹理名称" {} | _2D("2D", 2D) = "bump" {}
Cube | "默认纹理名称" {} | _Cube("Cube", Cube) = "white" {}
3D | "默认纹理名称" {} | _3D("3D", 3D) = "black" {}

下面的代码给出了一个展示所有属性类型的例子：

```shaderlab
Shader "Chapter03/ShaderPropertyExample" {
    Properties {
        //数值以及滑动条
        _Int("Int", Int) = 2
        _Float("Float", Float) = 1.5
        _Range("Range", Range(0.0, 5.0)) = 3.0
        //颜色和向量
        _Color("Color", Color) = (1.0, 1.0, 1.0, 1.0)
        _Vector("Vector", Vector) = (1.0, 2.0, -3.0, 4.0)
        //纹理
        _2D("2D", 2D) = "" {}
        _2D_2("2D_2", 2D) = "white" {}
        _2D_3("2D_3", 2D) = "black" {}
        _2D_4("2D_4", 2D) = "gray" {}
        _2D_5("2D_5", 2D) = "bump" {}
        _Cube("Cube", Cube) = "white" {}
        _3D("3D", 3D) = "black" {}
    }

    Fallback "Diffuse"
}
```

下图给出了上述代码在材面板中显示的结果。

![图3.8](images/chapter03_shader_property_example.png)

图3.8 不同属性类型在材质面板中显示的结果。

看起来纹理类型的默认纹理名称并不能在材质面板中体现。在运行时才体现。

测试代码如下：

```shaderlab
//这段代码修改自Unlit模板。
Shader "Chapter03/TestDefaultTextureName"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        _NormalTex ("NormalMap", 2D) = "bump" {}
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 100

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            // make fog work
            #pragma multi_compile_fog

            #include "UnityCG.cginc"

            struct appdata
            {
                float4 vertex : POSITION;
                float2 uv : TEXCOORD0;
            };

            struct v2f
            {
                float2 uv : TEXCOORD0;
                UNITY_FOG_COORDS(1)
                float4 vertex : SV_POSITION;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;

            sampler2D _NormalTex;
            float4 _NormalTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                UNITY_TRANSFER_FOG(o,o.vertex);
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                // sample the texture
                fixed4 col = fixed4(1, 1, 1, 1);
                if (i.uv.x > 0.5)
                {
                    col = tex2D(_MainTex, i.uv);
                }
                else
                {
                    col = tex2D(_NormalTex, i.uv);
                }
                // apply fog
                UNITY_APPLY_FOG(i.fogCoord, col);
                return col;
            }
            ENDCG
        }
    }
}
```

其材质面板的预览效果如下：

![图3.8.2 材质的预览面板上虽然NormalMap也显示着None(Texture)，但是下方的预览效果显示一半的蓝色和一半的白色。](images/chapter03_bump_default_texture.png)

可以看出来，默认的bump纹理对法线方向没有扰动。参见 [为什么法线贴图偏蓝色？](https://blog.csdn.net/taoqilin/article/details/49976927) 中对法线贴图的解释。

Unity允许我们重载默认的材质编辑面板，以提供更多自定义的数据类型。参见官方手册 [Custom Shader GUI](https://docs.unity3d.com/Manual/SL-CustomShaderGUI.html) 这篇文章。

为了在shader中可以访问到这些属性，需要在CG代码片中定义和这些属性类型相匹配的变量。即使不在Properties块中声明这些属性，也可以直接在CG代码片中定义变量。此时可以通过脚本向shader传递这些属性值。

Properties块的作用仅仅是为了让这些属性可以出现在材质面板中。

### 3.3.3 SubShader

每个unity shader文件包含至少一个SubShader块（SubShader的数量 $\ge$ 1）。当unity需要加载这个shader时，它会扫描所有的SubShader，然后选择第一个能够在目标平台上运行的SubShader。如果都不支持的话，它就会使用Fallback指定的shader。

不同的显卡具有不同的能力。一些旧的显卡仅能支持一定数目的操作指令，而一些更高级的显卡可以支持更多的指令。我们就可以使用SubShader功能，在旧的显卡上使用计算复杂度较低的着色器，而在高级的显卡上使用计算复杂度较高的着色器，以便提供更出色的画面。

SubShader的定义通常如下：

```shaderlab
SubShader {
    //Tags
    Tags { "TagName" = "Value" ...}
    
    //RenderSetup
    Cull Back | Front | Off
    ZTest Less | Greater | LEqual | GEqual | Equal | NotEqual | Always
    ZWrite On | Off
    Blend SrcFactor DstFactor
}
```