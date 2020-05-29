---
title: Qt 使提升控件类仍然支持QSS
date: 2020-05-05 20:29:25
categories: "Qt"
tags:
	- qss
---
有时候我们将控件提升后，发现QSS不生效了，很纳闷，后来才知道要重写paintEvent事件。插播一条广告，今天长征5号B型火箭首次发射成功。真棒！<!-- more -->
## 核心代码片段
```cpp
QStyleOption styleOpt;
styleOpt.init(this);
QPainter painter(this);
style()->drawPrimitive(QStyle::PE_Widget, &styleOpt, &painter, this);
```
## .h文件
```cpp
#ifndef SERIESITEM_H
#define SERIESITEM_H
#include <QWidget>
namespace Ui
{
	class SeriesItem;
}
class QPaintEvent;
class QHideEvent;
class SeriesItem : public QWidget
{
	Q_OBJECT
public:
	explicit SeriesItem(QWidget* parent = Q_NULLPTR);
	~SeriesItem();
signals:
	void Hide();//隐藏前发射信号
protected:
	virtual void paintEvent(QPaintEvent* event) override;
public:
	Ui::SeriesItem* ui;
};
#endif // !SERIESITEM_H
```
## .cpp文件
```cpp
#include "seriesitem.h"
#include "ui_SeriesItem.h"
#include <QEvent>
#include <QPainter>
#include <QStyleOption>
SeriesItem::SeriesItem(QWidget* parent):
	QWidget(parent),
	ui(new Ui::SeriesItem)
{
	ui->setupUi(this);
}
SeriesItem::~SeriesItem()
{
	if (ui)
	{
		delete ui;
		ui = nullptr;
	}
}
void SeriesItem::paintEvent(QPaintEvent* event)
{
	QStyleOption styleOpt;
	styleOpt.init(this);
	QPainter painter(this);
	style()->drawPrimitive(QStyle::PE_Widget, &styleOpt, &painter, this);
    QWidget::paintEvent(event);
}
void SeriesItem::hideEvent(QHideEvent* event)
{
	emit Hide();
	QWidget::hideEvent(event);
}

```
