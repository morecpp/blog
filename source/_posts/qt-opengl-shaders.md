---
title: qt-opengl（三）着色器
date: 2019-07-13 20:51:27
categories: "qt-opengl"
tags:
	- qt-opengl
---
着色器是opengl用来实现图像渲染的，着色器是运行在GPU上面的小程序。这些小程序为图形渲染的某个特定部分而运行。如果不适用着色器，那么用opengl可以做到的事情基本只有清除窗口内容了，可见着色器对于opengl的重要性。opengl 3.0版本以前（包含该版本），opengl还包含一个固定管线，它可以在不适用着色器的情况下处理几何像素数据。从3.1版本开始，固定功能管线从核心模式中去除，因此我们必须使用着色器来完成工作。<!-- more -->

&emsp;&emsp;无论是opengl还是其它图形API的着色器，通常是通过一种特殊的编程语言去编写的。对于opengl来说，用的是GLSL语言，也就是opengl shading language，它是在opengl 2.0版本左右发布的（之前属于扩展功能）。它与opengl的发展同时进行的，并通常会与每个新版本的opengl一起更新。虽然GLSL是一种专门为图形开发设计的编程语言，但是它却跟C语言非常类似，还有一点C++的影子。
任何一种opengl程序本质上都可以被分为两部分：CPU端运行部分，采用C++之类的语言进行编写；以及GPU端的运行部分，使用GLSL语言编写。

## opengl的可编程管线
&emsp;&emsp;下面详细的介绍渲染管线的处理阶段（4.5版本）

* 顶点着色阶段（vertex shading stage）将接收你在顶点缓存对象中给出的顶点数据，独立处理每个顶点。这个阶段对于所有的opengl程序都是唯一且必需的，并且oepgnl程序在绘制时必须绑定着色器。
* 细分着色阶段（tessellation shading stage）是一个可选的阶段，与应用程序中显式地指定几何图元的方法不同，它会在opengl管线内部生成新的几何体。这个阶段启用之后，会受到来自顶点着色器的输出数据，并且对收到的顶点进一步处理。
* 几何着色阶段（geometry shading stage）也是一个可选阶段，它会在opengl管线内部对所有几何图元就行修改。这个阶段会作用于每个独立的几何图元。此时你可以选择从输入图元生成更多的几何体，改变几何图元的类型（例如将三角形转换为线段），或者放弃所有的几何体。如果这个阶段被启用，那么几何着色阶段的输入可能会来自顶点着色阶段完成几何图元的顶点处理之后，也可能来自细分着色器阶段生成的图元数据（如果它也被启动）。
* opengl着色管线的最后一个部分是片元着色阶段（fragment shading stage）。这个阶段会处理opengl光栅化之后生成的独立片元（如果启用了采样着色模式，就是采样数据），并且这个阶段也必须绑定一个着色器。在这个阶段中，计算一个片元的颜色与深度值，然后传递到管线的片元测试和混合模块。
* 计算着色阶段（compute shading stage）和上诉阶段不同，它并不是图形管线的一部分，而是在程序中相对独立的一个阶段。计算着色阶段处理的并不是顶点和片元这类图形数据，而是应用程序给定范围内容。计算着色器在应用程序中可以成处理其它着色器程序所创建和使用的缓存数据。这其中也包括帧缓存后的处理效果，或者我们所期望的任何事物。

注意：我们经常用到的一般是顶点着色器跟片元着色器。
## GLSL	
&emsp;&emsp;着色器是使用一种叫GLSL的类C语言写成的。GLSL是为图形计算量身定制的，它包含一些针对向量和矩阵操作的有用特性。着色器的开头总是要声明版本，接着是输入和输出变量、uniform和main函数。每个着色器的入口点都是main函数，在这个函数中我们处理所有的输入变量，并将结果输出到输出变量中。如果你不知道什么是uniform也不用担心，我们后面会进行讲解。

一个典型的着色器有下面的结构：
```
#version version_number
in type in_variable_name;
in type in_variable_name;

out type out_variable_name;

uniform type uniform_name;

int main()
{
  // 处理输入并进行一些图形操作
  ...
  // 输出处理过的结果到输出变量
  out_variable_name = weird_stuff_we_processed;
}
```
GLSL的语法知识就不在这里赘述了，内容太多可以查看相关数据。

## 顶点着色器
```
//用来声明所使用的版本
#version 330 core 

// layout (location = 0)布局限定符，目的为变量提供元数据。可以用布局限定符来设置很多不同的属性
//在这里 aPos的位置属性为0 vec3数据类型 in表示输入 out表示输出
layout (location = 0) in vec3 aPos; 
 
// 指定一个颜色输出到片元着色器
out vec4 vertexColor; 

void main()
{
	//see how we directly give a vec3 to vec4's constructor
    gl_Position = vec4(aPos, 1.0); 
	//set the output variable to a dark-red color
    vertexColor = vec4(0.5, 0.0, 0.0, 1.0); 
}
```
## 片元着色器
```
#version 330 core
out vec4 FragColor;  
  
in vec4 vertexColor; //这里的输入要跟之前顶点着色器的颜色输出名字一致。 

void main()
{
    FragColor = vertexColor;
} 
```