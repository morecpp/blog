---
title: qt-opengl（四）纹理贴图
date: 2019-07-15 20:22:17
categories: "qt-opengl"
tags:
	- qt-opengl
---
发现自己并不适合讲这个，理解的并不深，做成这个纹理贴图要感谢TM大佬的帮助<!-- more -->
## 效果图
如果这个box.gif图不会动，刷新试试。
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/opengl/texture/box.gif)

## carmera.h头文件 
不要在意类的叫法，当时我可能看到相机模块吧，索性还是用这个名字吧。
```cpp
#ifndef CAMERA_H
#define CAMERA_H

#include <QOpenGLWidget>  
#include <QOpenGLFunctions_3_3_Core> 
#include <QTime>
class QOpenGLTexture;
class QOpenGLShaderProgram;
class CameraWidget : public QOpenGLWidget, protected QOpenGLFunctions_3_3_Core
{
	Q_OBJECT
public:
	explicit CameraWidget(QWidget *parent = 0);
	~CameraWidget();
protected:
	virtual void initializeGL() override;
	virtual void paintGL() override;
	virtual void resizeGL(int width, int height) override;
private:
	//着色器程序
	QOpenGLShaderProgram *program;
	//纹理1、纹理2
	QOpenGLTexture *ptexture1;
	QOpenGLTexture *ptexture2;
	//时间
	QTime time;

	//GLuint对象
	GLuint *pVAO;
	GLuint *pVBO;
};

#endif // !CAMERA_H

```

## carmera.cpp头文件
纹理用了两张图叠加的效果
```cpp
#include "camera.h"
#include <QOPenglTexture>
#include <QVector3D>
#include <QOpenGLShaderProgram>

CameraWidget::CameraWidget(QWidget *parent) : QOpenGLWidget(parent),
	program(new QOpenGLShaderProgram),
	ptexture1(nullptr),
	ptexture2(nullptr),
	pVAO(new GLuint[2]{}),
	pVBO(new GLuint[2]{})
{
	//设置OpenGL的版本信息  
	QSurfaceFormat format;
	format.setRenderableType(QSurfaceFormat::OpenGL);
	format.setProfile(QSurfaceFormat::CoreProfile);
	//指定opengl版本为3.3
	format.setVersion(3, 3);
	setFormat(format);
	setCursor(Qt::ArrowCursor);
}
CameraWidget::~CameraWidget()
{
	//删除所有之前添加到program的着色器
	program->removeAllShaders();
	if (program)
	{
		delete program;
		program = nullptr;
	}
	if (ptexture1)
	{
		delete ptexture1;
		ptexture1 = nullptr;
	}
	if (ptexture2)
	{
		delete ptexture2;
		ptexture2 = nullptr;
	}
	if (pVAO)
	{
		delete[] pVAO;
		pVAO = nullptr;
	}
	if (pVBO)
	{
		delete[] pVBO;
		pVBO = nullptr;
	}
}
void CameraWidget::initializeGL()
{

	//初始化OpenGL函数  
	initializeOpenGLFunctions();
	//设置全局变量  
	glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
	//这里不指定父类，我们自己来管理这个类
	//program = new QOpenGLShaderProgram;
	//将文件内容编译为指定类型的着色器，并将其添加到着色器程序program
	if (!program->addShaderFromSourceFile(QOpenGLShader::Vertex, "Resources/shader/shader.vert"))
	{
		//如果执行不成功，打印错误信息
		qDebug() <<"compile error"<< program->log();
	}
	if (!program->addShaderFromSourceFile(QOpenGLShader::Fragment, "Resources/shader/shader.frag"))
	{
		//如果执行不成功，打印错误信息
		qDebug() << "compile error" << program->log();
	}
	//将顶点着色器跟片元着色器链接起来
	if (!program->link())
	{
		//如果连接不成功，打印错误信息
		qDebug() << "link error" << program->log();
	}

	//设置顶点数据
	float vertices[] = {
		-0.5f, -0.5f, -0.5f,  0.0f, 0.0f,
		 0.5f, -0.5f, -0.5f,  1.0f, 0.0f,
		 0.5f,  0.5f, -0.5f,  1.0f, 1.0f,
		 0.5f,  0.5f, -0.5f,  1.0f, 1.0f,
		-0.5f,  0.5f, -0.5f,  0.0f, 1.0f,
		-0.5f, -0.5f, -0.5f,  0.0f, 0.0f,

		-0.5f, -0.5f,  0.5f,  0.0f, 0.0f,
		 0.5f, -0.5f,  0.5f,  1.0f, 0.0f,
		 0.5f,  0.5f,  0.5f,  1.0f, 1.0f,
		 0.5f,  0.5f,  0.5f,  1.0f, 1.0f,
		-0.5f,  0.5f,  0.5f,  0.0f, 1.0f,
		-0.5f, -0.5f,  0.5f,  0.0f, 0.0f,

		-0.5f,  0.5f,  0.5f,  1.0f, 0.0f,
		-0.5f,  0.5f, -0.5f,  1.0f, 1.0f,
		-0.5f, -0.5f, -0.5f,  0.0f, 1.0f,
		-0.5f, -0.5f, -0.5f,  0.0f, 1.0f,
		-0.5f, -0.5f,  0.5f,  0.0f, 0.0f,
		-0.5f,  0.5f,  0.5f,  1.0f, 0.0f,

		 0.5f,  0.5f,  0.5f,  1.0f, 0.0f,
		 0.5f,  0.5f, -0.5f,  1.0f, 1.0f,
		 0.5f, -0.5f, -0.5f,  0.0f, 1.0f,
		 0.5f, -0.5f, -0.5f,  0.0f, 1.0f,
		 0.5f, -0.5f,  0.5f,  0.0f, 0.0f,
		 0.5f,  0.5f,  0.5f,  1.0f, 0.0f,

		-0.5f, -0.5f, -0.5f,  0.0f, 1.0f,
		 0.5f, -0.5f, -0.5f,  1.0f, 1.0f,
		 0.5f, -0.5f,  0.5f,  1.0f, 0.0f,
		 0.5f, -0.5f,  0.5f,  1.0f, 0.0f,
		-0.5f, -0.5f,  0.5f,  0.0f, 0.0f,
		-0.5f, -0.5f, -0.5f,  0.0f, 1.0f,

		-0.5f,  0.5f, -0.5f,  0.0f, 1.0f,
		 0.5f,  0.5f, -0.5f,  1.0f, 1.0f,
		 0.5f,  0.5f,  0.5f,  1.0f, 0.0f,
		 0.5f,  0.5f,  0.5f,  1.0f, 0.0f,
		-0.5f,  0.5f,  0.5f,  0.0f, 0.0f,
		-0.5f,  0.5f, -0.5f,  0.0f, 1.0f
	};
	/*
	函数原型 void glGenBuffers(GLsizei n,GLuint * buffers);
	返回n个缓冲区对象名到数组buffers中
	*/

	glGenBuffers(1, pVBO);
	//创建一个VAO
	glGenVertexArrays(1, pVAO);

	
	//pVAO[0]是glGenVertexArrays返回的顶点数组对象的名称
	glBindVertexArray(pVAO[0]);

	glBindBuffer(GL_ARRAY_BUFFER, pVBO[0]);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

	//位置属性
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 5 * sizeof(float), (void *)0);
	glEnableVertexAttribArray(0);
	//纹理坐标属性
	glVertexAttribPointer(1, 2, GL_FLOAT, GL_FALSE, 5 * sizeof(float), (void *)(3 * sizeof(float)));
	glEnableVertexAttribArray(1);

	glBindBuffer(GL_ARRAY_BUFFER, 0);
	glBindVertexArray(0);//结束记录状态信息 

	//创建纹理对象1，并初始化
	ptexture1 = new QOpenGLTexture(QImage("./Resources/container.jpg").mirrored());
	if (!ptexture1->isCreated())
	{
		qDebug() << "Failed to load texture";
	}
	ptexture1->setWrapMode(QOpenGLTexture::DirectionS, QOpenGLTexture::Repeat);
	ptexture1->setWrapMode(QOpenGLTexture::DirectionT, QOpenGLTexture::Repeat);
	ptexture1->setMinificationFilter(QOpenGLTexture::Nearest);
	ptexture1->setMagnificationFilter(QOpenGLTexture::Linear);
	ptexture1->setWrapMode(QOpenGLTexture::Repeat);

	//创建纹理对象2，并初始化
	ptexture2 = new QOpenGLTexture(QImage("./Resources/yu.jpg").mirrored());
	if (!ptexture2->isCreated())
	{
		qDebug() << "Failed to load texture";
	}
	ptexture2->setWrapMode(QOpenGLTexture::DirectionS, QOpenGLTexture::Repeat);
	ptexture2->setWrapMode(QOpenGLTexture::DirectionT, QOpenGLTexture::Repeat);
	ptexture2->setMinificationFilter(QOpenGLTexture::Nearest);
	ptexture2->setMagnificationFilter(QOpenGLTexture::Linear);
	ptexture2->setWrapMode(QOpenGLTexture::Repeat);

	if (!program->bind())
	{
		qDebug() << "bind error" << program->log();
	}
	//设置上下文中名为"ptexture1"的uniform变量值为0
	program->setUniformValue("ptexture1", 0);
	//设置上下文中名为"ptexture2"的uniform变量值为1
	program->setUniformValue("ptexture2", 1);

	//开启定时器
	time.start();

	//给着色器变量赋值,projextion,view默认构造是生成单位矩阵
	QMatrix4x4 projection, view;
	view.translate(QVector3D(0.0f, 0.0f, -3.0f));
	projection.perspective(45.0f, (GLfloat)width() / (GLfloat)height(), 0.1f, 100.0f);
	/*
	将此程序绑定到active的OPenGLContext，并使其成为当前着色器程序
	先前绑定的着色器程序将被释放，等价于调用glUseProgram(GLuint id)
	id为着色器程序调用programId()接口返回的GLuint类型的值
	*/
	if (!program->bind())
	{
		qDebug() << "bind error" << program->log();
	}
	program->setUniformValue("view", view);
	program->setUniformValue("projection", projection);

	/* 固定属性区域 */
	glEnable(GL_DEPTH_TEST);  //开启深度测试 
}
void CameraWidget::paintGL()
{
	//清理屏幕  
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT); // also clear the depth buffer now!
	//实现参数的刷新  
	glActiveTexture(GL_TEXTURE0);
	ptexture1->bind();
	glActiveTexture(GL_TEXTURE1);
	ptexture2->bind();
	glBindVertexArray(pVAO[0]);
	QMatrix4x4 model;
	model.rotate((float)time.elapsed() / 20, QVector3D(0.5f, 1.0f, 0.0f));
	if (!program->bind())
	{
		qDebug() << "bind error" << program->log();
	}
	program->setUniformValue("model", model);
	glDrawArrays(GL_TRIANGLES, 0, 36);
	glBindVertexArray(0); 
	program->release();
	update();
}
void CameraWidget::resizeGL(int width, int height)
{
	glViewport(0, 0, width, height);
	update();
}

```
## 顶点着色器shader.vert
```
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec2 aTexCoord;

out vec2 TexCoord;

uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main()
{
	gl_Position = projection * view * model * vec4(aPos, 1.0f);
	TexCoord = aTexCoord;
}
```
## 片元着色器shader.frag
```
#version 330 core
out vec4 FragColor;

in vec2 TexCoord;

// texture samplers
uniform sampler2D ptexture1;
uniform sampler2D ptexture2;

void main()
{
	FragColor = mix(texture2D(ptexture1, TexCoord), texture2D(ptexture2, TexCoord), 0.2f);
}
```

## 用到的贴图资源
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/opengl/texture/container.jpg)
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/opengl/texture/yu.jpg)

## 注意事项
上面用到着色器跟纹理贴图的路径都是在项目的相对路径下。可以定义自己的相对路径。
