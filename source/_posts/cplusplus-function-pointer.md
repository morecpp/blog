---
title: C++ 函数指针
date: 2019-04-27 16:57:01
categories: "C++"
tags:
  - C++
---
函数指针指向的是函数而非对象。和其它指针一样，函数指针指向某种特定类型，函数的类型由它的返回类型和形参类型共同决定，与函数名无关。 把函数指针当作形参传递给某些具有一定通用功能的模块。封装成接口来提高代码的灵活性和后期维护的便捷性。
<!--more-->
例如：
```cpp
//比较两个QString的长度
bool LengthCompare(const QString &, const QString &);
```
该函数的类型是 `bool (const QString &,const QString &)`。要想声明一个指向该函数的指针，只需要用指针替换函数名即可：
```cpp
//pf指向一个函数，该函数的参数是两个const QString的引用，返回值是bool类型。
bool (*pf)(const QString &, const QString &);//未初始化
bool (*pf)(const QString &, const QString &)(LengthCompare);//初始化
```
&emsp;&emsp;从声明的名字开始观察， `pf` 前面有个 `*` ，因此 `pf`是指针，右侧是形参列表，表示 `pf` 指向的是函数，再观察左侧，函数的返回值类型是 `bool` 类型。因此，`pf` 就是一个指向函数的指针，其中该函数的参数是两个 `const QString` 的引用，返回值是 `bool`类型。
## 注意： 
`*pf 两端的括号必不可少，如果少了这对括号，则pf是一个指向返回值为 bool指针的函数：声明一个为pf的函数，该函数返回bool*`
`bool *pf(const QString &,const QString &);`
## 使用函数指针

&emsp;&emsp;当我们把函数名作为一个值使用时，该函数**自动地转换成函数指针**。例如，按照如下形式我们可以将 `LengthCompare` 的地址赋给 `pf`：
```cpp
pf = LengthCompare; //pf指向名为LengthCompare的函数
pf = &LengthCompare;//等价的赋值语句：取地址符是可选的
```
此外我们还能直接使用指向函数的指针调用该函数，无需提前解引用指针：
```cpp
bool b1 = pf("hello","xiaoyuren");//调用LengthCompare函数
bool b2 = (*pf)("hello","xiaoyuren");//一个等价的调用
bool b3 = LengthCompare("hello","xiaoyuren");//另一个等价的调用
```
在指向不同的函数类型的指针间**不存在转换规则**。但是和往常一样，我们可以为函数指针赋一个 `nullptr` 或者为 `0` 的整形常量表达式，表示该指针没有指向任何一个函数：
```cpp
int SumLength(const QString &, const QString &);
bool StringCompare(const char*,const char*);
pf = 0;//正确：pf不指向任何函数
pf = nullptr;//正确：同上
pf = SumLength;//错误：返回类型不匹配
pf = StringCompare;//错误：形参类型不匹配
pf = LengthCompare;//正确：函数指针和类型精确匹配
```
## 重载函数的指针
&emsp;&emsp;当我们使用重载函数时，上下文必须清晰地界定到底该选用哪个函数。如果定义了指向重载函数的指针
```cpp
void F(int *);
void F(unsigned int);
void (*pf1)(unsigned int) = ff;//pf1指向F(unsigned int)
```
编译器通过指针类型决定选用哪个函数，指针类型必须与重载函数中的某一个精确匹配
```cpp
void (*pf2)(int) = F;//错误：没有任何一个F与该形参列表匹配
double (*pf3)(int *) = F;//错误：F和pf3的返回值类型不匹配
```
## 函数指针形参
&emsp;&emsp;和数组类似，虽然不能定义函数类型的形参，但是形参可以是指向函数的指针。此时，形参看起来是函数类型，实际上却是当成指针使用：
```cpp
//第三个形参是函数类型，它会自动地转换成指向函数的指针
void UserMax(const QString &s1,const QString &s2,bool pf(const QString &, const QString &));
//等价声明，显示地将形参定义成指向函数的指针
void UserMax(const QString &s1,const QString &s2,bool (*pf)(const QString &, const QString &));
//我们还可以直接把函数作为实参使用，此时它会自动转换成指针，这个我平时用的比较多。
UserMax(s1, s2, LengthCompare);
``` 
正如 `UserMax` 的声明语句所示，直接使用函数指针类型显得冗长而繁琐，类型别名能让我们简化使用：
```cpp
//Func和Func2是函数类型
typedef bool Func(const QString &, const QString &);
typedef decltype(LengthCompare) Func2;//等价类型
//FuncP和FuncP2是指向函数的指针
typedef bool (*FuncP)(const QString &,const QString &)；
typedef decltype(LengthCompare) *FuncP2;//等价类型
```
我们使用 `typedef` 定义自己的类型。 `Func` 和 `Func2` 是函数类型，而 `FuncP` 和 `FuncP2`是指针类型。需要注意的是， `decltype` 返回函数类型，此时不会讲函数类型自动转换成指针类型。所以只有在结果前面加上 `*` 才能得到指针。可以使用如下的形式重新声明UseMax：
```cpp
//UseMax的等价声明，其中使用了类型别名
void UseMax(const QString &, const QString &, Func);
void UseMax(const QString &, const QString &, FuncP2);
```
这两个声明语句声明的是同一个函数，在第一条语句中，编译器自动地将Func表示的函数类型转换成指针。
## 返回指向函数的指针
&emsp;&emsp;和数组类似，虽然不能返回一个函数，但是能返回指向函数类型的指针。然而，我们必须把返回类型写成指针形式，编译器不会自动地将函数返回类型当成对应的指针类型处理。与往常一样，要想声明一个返回函数指针的函数，最简单的办法是使用类型别名：
```cpp
using F = int(int* ,int);      //F是函数类型，不是指针
using PF = int(*)(int *, int); //PF是指针类型
```
其中我们使用类型别名，将 `F` 定义成函数类型，将 `PF` 定义成指向函数类型的指针。必须时刻注意的是，和函数类型的形参不一样，返回类型不会自动地转换成指针。我们必须显式地将返回类型指定为指针：
```CPP
PF f(int);//正确：PF是指向函数的指针，f返回指向函数指针
F f(int); //错误：F是函数类型，f不能返回一个函数类型
F *f(int);//正确：显示地指定返回类型是指向函数的指针
```
当然，我们也能用另外一种形式直接声明f：
```cpp
int (*f(int))(int *, int);
```
&emsp;&emsp;按照由内向外的顺序阅读这条声明语句：我们看到f有形参列表，所以 `f`是个函数； `f` 前面有 `*` ，所以  `f` 返回一个指针；进一步观察发现，指针的类型本身也包含形参列表，因此指针指向函数，该函数的返回类型是 `int`。
出于完整性考虑，有必要提醒大家还可以使用尾置返回类型的方式，声明一个返回函数指针的函数：
```cpp
auto f(int) -> int (*)(int *, int);
```