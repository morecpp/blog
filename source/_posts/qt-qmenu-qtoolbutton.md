---
title: Qt 利用QToolButton实现菜单按钮的效果
date: 2019-07-29 20:06:32
categories: "Qt"
tags:
	- Qt
---
最近刚好遇到一个需求，点击button弹出一个菜单，这个功能很常见，但是我却还是第一次用，查阅资料，可以有两种方法可以选择，一个是利用QPushButton，另一个是QToolButton，他们都可以将按钮变成菜单按钮。
<!-- more -->
## 效果图
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/button-menu/menu.gif)
## 原理void QToolButton::showMenu()
Shows (pops up) the associated popup menu. If there is no such menu, this function does nothing. This function does not return until the popup menu has been closed by the user.
同样QPushButton也有同样的接口函数。
## 实现
```cpp
	//注册表
	QSettings *psettings = new QSettings(QSettings::NativeFormat, QSettings::UserScope, "Qt", "appname"));
	/*******设置菜单栏*******/
	//主菜单
	QMenu *psetmenu = new QMenu;
	//子菜单语言，包含中文跟英文
	QMenu *planguage = new QMenu;
	//独占执行的容器，功能跟QButtonGroup相似
	QActionGroup *planguagegroup = new QActionGroup(this);

	//中文
	QAction *pchinese = new QAction(tr("Chinese"), planguage);
	//添加到语言菜单
	planguage->addAction(pchinese);
	pchinese->setCheckable(true);
	//设置信号与槽
	connect(pchinese, &QAction::triggered, this, &MainWindow::OnLanguageChinese);
	//添加到actiongroup管理
	planguagegroup->addAction(pchinese);
	
	//英文
	QAction *penglish = new QAction(tr("English"), planguage);
	//添加到语言菜单
	planguage->addAction(penglish);
	penglish->setCheckable(true);
	//设置信号与槽
	connect(penglish, &QAction::triggered, this, &MainWindow::OnLanguageEnglish);
	//添加到actiongroup管理
	planguagegroup->addAction(penglish);

	//根据注册表信息设置默认选中语言
	if ("chinese" == psettings->value("language", "").toString())
	{
		pchinese->setChecked(true);
	}
	if ("english" == psettings->value("language", "").toString())
	{
		penglish->setChecked(true);
	}
	//将子菜单语言添加到设置菜单中
	psetmenu->addMenu(planguage)->setText(tr("language"));
	psetmenu->addMenu(planguage)->setIcon(QIcon(":/MainWindow/Resources/leaftwo.png"));

	//日志
	QAction *plog = new QAction(tr("log"), psetmenu);
	plog->setCheckable(true);
	if (psettings->value("islogworking").toBool())
	{
		plog->setChecked(true);
	}
	//设置信号与槽
	connect(plog, &QAction::triggered, this, &MainWindow::OnIsLogWorking);
	psetmenu->addAction(plog);
	//主题
	psetmenu->addAction(new QAction(tr("theme"), psetmenu));

	//将toolbutton变为菜单按钮
	this->ui->setButton->setMenu(psetmenu);
```
## QMenu的样式设置
```
QMenu
{
    background-color:rgb(27,27,28);
}
QMenu::item
{
    color:rgb(250,250,250);
    font-family: "微软雅黑";
    padding: 2px 25px 2px 20px;
    margin:2px,2px;
}
QMenu::item:selected
{
    background-color:rgb(9,71,113);
    /*background-color:rgb(70,115,144);*/
}
QMenu::separator 
{
    height: 2px;
    background: lightblue;
    margin-left: 10px;
    margin-right: 5px;
}
QMenu::icon:checked
{
    height:24px;
    width:24px;
}
```
## 补充
语言菜单里面的中文跟英文，每次只能选中一个，类似QPushButton的checked独占方式。
log可以选中跟不选中，不具备独占的方式，更详细的QMenu使用方法可以去查看QMenu的官方例子。
