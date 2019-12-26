---
title: C++ 友元
date: 2019-04-09 19:54:33
categories: "C++"
tags:
  - C++
---
有时候我们可能需要访问其它类的成员数据，这时候我们可以将其设置为类的友元。类可以允许其它类或者函数访问它的非公有成员，方法是令其它函数或者类成为它的友元（Friends）。
<!--more-->
## 函数作为类的友员

```cpp
#include <QDialog>
class MainWindow :public QDialog
{
	Q_OBJECT
	//为MainWindow的非成员函数所做的友员声明
	friend void SetText(const MainWindow&);
	friend void SetMainWindow(const MainWindow&);
	friend MainWindow& GetMainWindow(const MainWindow&);
public:
	explicit MainWindow(QDialog* parent = 0);
	MainWindow(const MainWindow&) = default;
	MainWindow& operator = (const MainWindow&) = default;
	~MainWindow();
private:
	QPoint point;
};
//MainWindow接口的非成员组成部分的声明
void SetText(const MainWindow&);
void SetMainWindow(const MainWindow&);
MainWindow& GetMainWindow(const MainWindow&);
```

**注意：**
&emsp;&emsp;友元声明只能出现在类定义的内部，但是在类内的具体位置不限。也就是说可以在public、protected和private中的任意范围。但是一般我们都写在前面。友元不是类的成员也不受它所在区域访问控制级别的约束。

**友元的声明：**
&emsp;&emsp;友元的声明仅仅指定了访问权限，而并非一个通常意义上的函数声明，所以我们必须在友元声明之外再专门对友元函数进行一次声明，为了使友元对类的用户可见，我们通常把友元的声明与类本身放置在同一个头文件中（类的外部 这里是MainWindow类的外部）。

## 其它类作为类的友元
```cpp
#include <QDialog>
class QLabel;
class MainWindow :public QDialog
{
	Q_OBJECT
	//为MainWindow的外部类所做的友员声明
	friend class LoadWidget;
public:
	explicit MainWindow(QDialog* parent = 0);
	MainWindow(const MainWindow&) = default;
	MainWindow& operator = (const MainWindow&) = default;
	~MainWindow();
	void SetText(const QString&);
private:
	QPoint point;
	QLabel *plabel;
};
```
&emsp;&emsp;如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。
在此类LoadWidget的成员函数可以访问类MainWindow的SetText成员函数和数据成员point跟plabel。

## 成员函数作为友元
&emsp;&emsp;我们还可以仅仅只把类的某个特定成员函数定义成某个类的友元。
```cpp
#include <QDialog>
class LoadWidget
{
public:
	LoadWidget();
	LoadWidget(const LoadWidget&) = default;
	LoadWidget& operator = (const LoadWidget&) = default;
	~LoadWidget();
	void CleanRect(QPoint mpoint, int width, int length);
};
class MainWindow :public QDialog
{
	Q_OBJECT
	//为MainWindow的外部类的成员函数所做的友员声明
	friend void LoadWidget::CleanRect(QPoint mpoint, int width, int length);
public:
	explicit MainWindow(QDialog* parent = 0);
	MainWindow(const MainWindow&) = default;
	MainWindow& operator = (const MainWindow&) = default;
	~MainWindow();
private:
	QPoint point;
};
```

&emsp;&emsp;这样类MainWindow只为LoadWidget的成员函数CleanRect提供访问权限，当把一个成员函数声明成友元时，我们必须明确指出该成员函数属于哪个类。想要令某个成员作为友元，我们必须仔细组织程序的结构以满足声明和定义的彼此依赖关系：
* 首先定义LoadWidget类，其中声明CleanRect函数，但是不能定义它，在CleanRect使用MainWindow的成员之前必须先声明MainWindow。
* 接下来定义MainWindow，包括对于CleanRect的友元声明。
* 最后定义CleanRect，此时它才可以使用MainWindow的成员。