---
title: Qt Quick 第一个quick程序
date: 2019-08-07 18:06:05
categories: "Qt Quick"
tags:
	- Qt Quick
---
为了在我的QWidget引入qml，直接用VS+Qt来写很麻烦，所以打算直接在QtCreator IDE来先实现然后移植到VS项目下，期间经历了好几次失败，没跑起来，很有挫败感，好在试验了很多次，终于在Qt自带项目的基础上修改，跑起来了我自己写的Qml资源文件。故此把这第一次给记录下来，目前仅仅只是在QtCreator下实现，还没移植。
<!-- more -->
## 说明
我是在Qt5.9.6版本下的Qt Creator下操作的，编译器为微软的MSVC2015。项目是在Qt自带项目Qt Quick wearable项目的基础上修改的。

## 运行图
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/quick/getting-started/run.png)

## wearable.cpp的修改
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/quick/getting-started/wearable.png)
把wearable.cpp源码也贴上吧
```CPP
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQuickStyle>

int main(int argc, char *argv[])
{
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
    QGuiApplication app(argc, argv);

    //! [style]
    //QQuickStyle::setStyle(QStringLiteral("qrc:/qml/Style"));
    //! [style]

    QQmlApplicationEngine engine;
	//这里的main.qml路径不要写错
    engine.load(QUrl(QStringLiteral("qrc:///main.qml")));

    return app.exec();
}
```
注意这个`engine.load(QUrl(QStringLiteral("qrc:///main.qml")));`资源路径不要写错，我一直没写对这个路径，后来翻看了很多自带项目，才发该怎么写，之前一直GG。
## 创建自己的quick.qrc资源文件，并在里面添加main.qml资源
```JS
import QtQuick 2.0
import QtQuick.Window 2.0

Window {
    id: root
    width: 800
    height: 600
    visible: true
    title: qsTr("雯雯：断剑重铸之日，骑士归来之时。")
    Image {
        width:300
        height:300
        id: name
        anchors.horizontalCenter: parent.horizontalCenter
        anchors.bottom: parent.bottom
        source: "images/background.png"
    }
}
```
之前尝试用Rectange，Image都不行，原来这个主窗口，需要用Window来搞。
## 万事开头难
后面好了，可以尝试更多的qml玩法，会发布更多的实际使用效果。
