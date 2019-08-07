---
title: Qt 利用QSS在button里面显示多张图
date: 2019-08-05 14:45:27
categories: "Qt"
tags:
	- Qt-qss
---
之前用绘制手段实现了这个效果，操作麻烦，费时费力。现在直接用QPushButton + qss就达到了这样的效果。<!-- more -->

## 效果图
静态图：
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/qss/programSwitch.png)
动图：
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/qss/programSwitch.gif)
## qss实现
```
QPushButton#programButton1
{
    text-align: top;
    background-repeat:norepeat;
    background-position:left center;
    background-origin:content;
    background-image: url(:/new/svg/Resources/FineTuning/pictoMaster.png);
    background-position:center;
    background-color: qlineargradient(x1:0.5, y1:0, x2:0.5, y2:1,
    stop:0 rgb(232, 232, 232),
    stop:0.5 rgb(210,210,210),
    stop:1 rgb(232, 232, 232));
    border: 1px solid rgb(180, 180, 180);
    border-radius:4px;
    padding-top:4px;
}

QPushButton#programButton2
{
    text-align: top;
    background-repeat:norepeat;
    background-position:left center;
    background-origin:content;
    background-image: url(:/new/svg/Resources/FineTuning/pictoPhoneone.png);
    background-position:center;
    background-color: qlineargradient(x1:0.5, y1:0, x2:0.5, y2:1,
    stop:0 rgb(232, 232, 232),
    stop:0.5 rgb(210,210,210),
    stop:1 rgb(232, 232, 232));
    border: 1px solid rgb(180, 180, 180);
    border-radius:4px;
    padding-top:4px;
}

QPushButton#programButton3
{
    text-align: top;
    background-repeat:norepeat;
    background-position:left center;
    background-origin:content;
    background-image: url(:/new/svg/Resources/FineTuning/pictoMusic.png);
    background-position:center;
    background-color: qlineargradient(x1:0.5, y1:0, x2:0.5, y2:1,
    stop:0 rgb(232, 232, 232),
    stop:0.5 rgb(210,210,210),
    stop:1 rgb(232, 232, 232));
    border: 1px solid rgb(180, 180, 180);
    border-radius:4px;
    padding-top:4px;
}

QPushButton#programButton4
{
    text-align: top;
    background-repeat:norepeat;
    background-position:left center;
    background-origin:content;
    background-image: url(:/new/svg/Resources/FineTuning/pictoZen.png);
    background-position:center;
    background-color: qlineargradient(x1:0.5, y1:0, x2:0.5, y2:1,
    stop:0 rgb(232, 232, 232),
    stop:0.5 rgb(210,210,210),
    stop:1 rgb(232, 232, 232));
    border: 1px solid rgb(180, 180, 180);
    border-radius:4px;
    padding-top:4px;
}
QPushButton#programButton1::hover,QPushButton#programButton2::hover,
QPushButton#programButton3::hover,QPushButton#programButton4::hover
{
    background-color: qlineargradient(x1:0.5, y1:0, x2:0.5, y2:1,
    stop:0 rgb(212, 212, 212),
    stop:0.5 rgb(180,180,180),
    stop:1 rgb(212, 212, 212));
    color:rgb(0,158,206);
    border: 2px solid rgb(0,158,206);
    border-radius:4px;
    padding-bottom:3px;
    padding-right:3px;
}
QPushButton#programButton1::pressed,QPushButton#programButton2::pressed,
QPushButton#programButton3::pressed,QPushButton#programButton4::pressed
{
    background-color: qlineargradient(x1:0.5, y1:0, x2:0.5, y2:1,
    stop:0 rgb(162, 162, 162),
    stop:0.5 rgb(130,130,130),
    stop:1 rgb(162, 162, 162));
    padding-top:3px;
    padding-right:3px;
}
QPushButton#programButton1::checked,QPushButton#programButton2::checked,
QPushButton#programButton3::checked,QPushButton#programButton4::checked
{
    background-color: qlineargradient(x1:0.5, y1:0, x2:0.5, y2:1,
    stop:0 rgb(212, 212, 212),
    stop:0.5 rgb(180,180,180),
    stop:1 rgb(212, 212, 212));
    color:rgb(255,255,255);
    border: 2px solid rgb(0,158,206);
    border-radius:4px;
    color:rgb(0,158,206);
    padding-right:12px;
    padding-bottom:12px;
    image:url(:/MainWindow/Resources/checkfive.png);
    image-position:bottom right;
}

```
## 解析
主要利用了background-image跟image不在同一层的原理来实现的。



