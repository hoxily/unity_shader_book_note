# 第8章 透明效果

## 8.5 开启深度写入的半透明效果

使用双Pass来渲染半透明物体。该物体自身的面片的遮挡关系正确，但是因为是无视alpha的条件下做的深度测试，所以自身面片之间无半透明效果。只能是该物体可见部分与背景融合。可以理解为使用Opaque方式得到一个RenderTexture（附带Alpha值），然后将这个RenderTexture融合到背景上。

第一个Pass的要点有两点：

ZWrite On //开启深度写入
ColorMask 0 //屏蔽颜色写入

第二个Pass按照Opaque方式正常渲染。

## 8.7 双面渲染的透明效果

双面透明度混合要点：
1. 复制一份单面透明度混合的Pass代码，得到含有2个Pass的Shader；
2. 在两个Pass中分别使用Cull Front 和 Cull Back，得到分别仅渲染背面和仅渲染正面的结果。
