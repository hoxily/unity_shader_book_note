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
:-: | :-: | :-: |
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
