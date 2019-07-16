---
title: qt-opengl（一） OpenGL简介
date: 2019-07-04 20:23:50
categories: "qt-opengl"
tags:
	- qt-opengl
---
最近想学习3d知识，网上查阅资料，主流的就是OpenGL跟DirectX,不知道为什么，可能是我脑海中听到关于opengl的标签更多吧，亦或是我知道了Directx是出自微软，本身在当时选择2d界面我用Qt，没有选择用C#,一个是跟我过往的经历有关，个人原因，另外就是我不是很喜欢C#的语法，加之我曾经比较容易就破解过C#的软件，总总原因促成了我不太喜欢C#，不太喜欢微软的一些产品，也导致了我不喜欢DirectX。最后我选择了学习opengl，因为它是C写的，只要看到C做的东西我都很喜欢，在我现在的认知当中，C还是最高效的。即使是go，也是模仿C，另外增加了一些命名空间之类的操作，我对go不是很熟悉，也是道听途说。现在把思绪拉回正题opengl。往后我会把我学习到的opengl知识记录下来，以供未来回过头来参考。<!-- more -->

## OpenGL简介
OpenGL是一种应用程序编程接口（Application Programming Interface,API）,它是一种可以对图形硬件设备特性进行访问的软件库。目前最新版本的opengl已经到4.5版本了。opengl被设计为一个现代化的，硬件无关的接口，因此我们可以在不考虑计算机操作系统或窗口系统的前提下载多种不同的图形硬件系统上，或者完全通过软件的方式实现opengl接口。opengl本身并不包含任何执行窗口任务或者处理用户输入的函数。事实上，我们需要通过应用程序所运行的窗口系统提供的接口来执行这类操作。与此类似，opengl也没有提供任何用于表达三维物体模型，或者读取图像文件的（例如JPEG/PNG格式文件）的操作。这个时候，我们需要通过一系列的几何图元（包括点、线、三角形以及面片）来创建三维物体。OpenGL就像一个潘多拉盒子，打开后会产生很多美妙的事。
## OpenGL渲染执行的主要操作
* 从OpenGL的几何图元设置数据，用于构建形状。
* 使用不同的着色器（shader）对输入的图元数据执行计算操作，判断它们的位置、颜色，以及其它渲染属性。关于着色器我们主要用到的是顶点着色器跟片元着色器，关于着色器语言GLSL写成了一本单独的书，感兴趣的小伙伴可以自己去查看。
* 将输入图元的数学描述转换为与屏幕位置对应的像素片元（fragment)。这一步作光栅化（resterization）。（opengl中的片元若最终渲染为图像，那就是像素）
* 最后，针对光栅化过程产生的每个片元，执行片元着色器（fragment shader），从而决定这个片元的最终颜色和位置。
* 如果有必要，还需要对每个片元执行一些额外的操作，例如判断片元对应的对象是否可见，或者将片元的颜色与当前的屏幕位置与颜色融合。

补充：OpenGL另一个本质的概念叫着色器。另外OpenGL规范被一个叫Khronos的组织维护着


## OpenGL学习干货
这个是OpenGL领域里面最好的教程，没错就是最好的。作者是一个名叫JoeyDeVries的大牛写的。
传送门英文网站：[**https://learnopengl.com/**](https://learnopengl.com/)
传送门中文网站：[**https://learnopengl-cn.github.io/**](https://learnopengl-cn.github.io/)

当然最丰富的资源还数opengl的官方网站:[**https://www.opengl.org/**](https://www.opengl.org/) 
在这里你可以获取最新，最全的opengl相关资讯，你还可以在这个网站上面查询到所有接口的说明，要想成为一个opengl的大牛，opengl api接口说明查看你是少不了，也绕不去的点。
OpenGL API接口官方说明查询网站：
[**https://www.khronos.org/registry/OpenGL-Refpages/gl4/**](https://www.khronos.org/registry/OpenGL-Refpages/gl4/)

当然书籍对我来说也是必不可上的，我看电子书，经常会眼睛花，所以还是比较喜欢看书。不太喜欢看电子书。
这里是官方的书籍购买，有好多版本：
[**https://www.khronos.org/developers/books/**](https://www.khronos.org/developers/books/)
在这里我推荐我买这两本书，非常值得大家看的opengl书籍。
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/opengl/introduction/opengl9.jpg)
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/opengl/introduction/opengles3.jpg)

