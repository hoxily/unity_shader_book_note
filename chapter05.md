# 开始学习Unity Shader之旅

## 5.2.4 使用属性

表5.1 ShaderLab属性类型和CG变量类型的匹配关系

ShaderLab属性类型 | CG变量类型
:-: | :-:
Color, Vector | float4, half4, fixed4
Range, Float | float, half, fixed
2D | sampler2D
Cube | samplerCube
3D | sampler3D

## 5.3 Unity内置的cginc文件和变量

表5.2 unity中一些常用的包含文件。

cginc文件 | 描述
:- | :-
UnityCG.cginc | 包含了最常用使用的帮助函数、宏和结构体等
UnityShaderVariables.cginc | 在编译unity shader时，会被自动包含进来。包含了许多内置的全局变量，如UNITY_MATRIX_MVP等。
Lighting.cginc | 包含了各种内置的光照模型。如果编写的是Surface Shader，会自动包含进来。
HLSLSupport.cginc | 在编译unity shader时会被自动包含进来。声明了很多用于跨平台编译的宏和定义。

表5.3 UnityCG.cginc中的常用结构体

结构体 | 描述 | 包含的变量
:- | :- | :-
appdata_base | 可用于顶点着色器的输入 | 顶点位置、顶点法线、第一组纹理坐标
appdata_tan | 可用于顶点着色器的输入 | 顶点位置、顶点法线、第一组纹理坐标、顶点切线
appdata_full | 可用于顶点着色器的输入 | 顶点位置、顶点法线、4组纹理坐标（或更多）、顶点切线
appdata_img | 可用于顶点着色器的输入 | 顶点位置、第一组纹理坐标
v2f_img | 可用于顶点着色器的输出 | 裁剪空间中的位置、纹理坐标

注：查看UnityCG.cginc可以发现，appdata_full里并没有更多的纹理坐标，只有4组。

## 5.4 unity提供的CG/HLSL语义

表5.5 从应用阶段传递给顶点着色器时Unity支持的常用语义

语义 | 描述
:- | :-
POSITION | 模型空间中的顶点位置，通常是float4
NORMAL | 顶点法线，通常是float3
TANGENT | 顶点切线，通常是float4
TEXCOORD0, TEXCOORD1, TEXCOORD2, TEXCOORD3 | 顶点的纹理坐标。通常是float2或float4
COLOR | 顶点颜色，通常是fixed4或float4

参考unity手册：file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/SL-VertexProgramInputs.html

表5.6 从顶点着色器传递数据给片元着色器时Unity支持的常用语义

语义 | 描述
:- | :-
SV_POSITION | 裁剪空间中的顶点位置。必须包含一个该语义修饰的变量。
COLOR0 | 用于输出第一组顶点颜色，但不是必须的
COLOR1 | 用于输出第二组顶点颜色，但不是必须的
TEXCOORD0~TEXCOORD7 | 用于输出纹理坐标，但不是必须的

参考unity手册：file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/SL-ShaderSemantics.html 的 Vertex shader outputs and fragment shader inputs 小节。

表5.7 片元着色器输出时unity支持的常用语义

语义 | 描述
:- | :-
SV_Target | 输出值将会存储到渲染目标中。

参考unity手册：file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/SL-ShaderSemantics.html 的 Fragment shader output semantics 小节。

## 5.5 shader调试技术

主要包括3种调试方法：
1. 假彩色图像（false-color image）
2. Visual Studio的Graphics Debugger
3. 帧调试器 Frame Debugger

注：书中介绍的假彩色图像技术，可以在官方手册 file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/SL-VertexProgramInputs.html 的各种Visualize找到对应映射方法。并且官方手册里的可视化UV还能应对UV值超出[0, 1]的情况，使用蓝色分量来修饰这种情况。

### 5.5.2 Graphics Debugger

官方手册地址：file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/SL-DebuggingD3D11ShadersWithVS.html

参照手册上的说明，成功Capture到一帧图像。也能在呈现目标上点击查看像素的DrawIndexed方法。

如果没有开启 #pragma enable_d3d11_debug_symbols ，那么只能以汇编形式查看，完全看不懂。

如果开启 #pragma enable_d3d11_debug_symbols，那么调试时能查看到hlsl级代码，但是看起来行号没有对上。因为我选择的normal toggle，但是调试时竟然在 VisualizeUv 函数里跳动。

### 5.5.3 Frame Debugger

Frame Debugger是Unity集成在Editor的帧调试器。可以使用Frame Debugger看到游戏图像的某一帧是如何一步步渲染出来的。

官方手册地址：file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/FrameDebugger.html

## 5.6 小心渲染平台的差异

官方手册地址：file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/SL-PlatformDifferences.html

### 5.6.1 渲染纹理的坐标差异

Unity默认使用OpenGL的屏幕空间坐标。即左下角为(0, 0)，右上角为(1, 1)。

当在DirectX平台上使用渲染到纹理技术时，Unity会为我们翻转屏幕图像纹理，以便在不同平台上达到一致性。但是在特殊情况下Unity不会执行这个翻转操作，即开启了抗锯齿（在Edit->Project Settings->Quality->Anti Aliasing）并且使用了渲染到纹理技术。在这种情况下，Unity首先渲染得到屏幕图像，再由硬件进行抗锯齿处理，得到一张渲染纹理供我们处理。

```shaderlab
#if UNITY_UV_STARTS_AT_TOP
if (_MainTex_TexelSize.y < 0)
{
    uv.y = 1 - uv.y;
}
#endif
```

## 5.7 Shader整洁之道

### 5.7.1 数值精度

### 5.7.2 规范语法

DirectX平台对Shader的语义有更加严格的要求。

### 5.7.3 避免不必要的计算

不同的Shader Target、不同的着色器阶段，我们可使用的临时寄存器和指令数目都是不同的。

详细 Shader Target表见 file:///D:/Users/hoxily/Documents/UnityDocs/Documentation.2018.4/en/Manual/SL-ShaderCompileTargets.html

### 5.7.4 慎用分支和循环语句

### 5.7.5 不要除以0

对于除数可能为0的情况，强制截取到非零的范围。
