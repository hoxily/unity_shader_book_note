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
