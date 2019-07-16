---
title: C++ 静态成员
date: 2019-04-14 11:33:38
categories: "C++"
tags:
	- C++
---
在类的内部定义，关键字 `static` 未绑定到类实例的成员；
在类的外部定义，它有不同的含义，具体将在“**存储持续时间**”讲解。
<!--more-->
## 语句
* `static data_menber`      //声明一个静态数据成员    
* `static member_function`  //声明一个静态成员函数 
## 说明
类的静态成员不与类的对象关联：**它们是**具有静态或线程（since C++11）存储持续时间或常规函数的**独立变量**。关键字 `static` **仅与**类定义中的静态成员的声明一起使用，但**不与**该静态成员的定义一起使用：
```cpp
class X { static int n;};  //声明 加static
int X::n = 1;              //定义 不加static
```
类内部的声明不是定义，可以声明该成员是不完整类型（除了 `void` 类型），包括声明成员的类型：
```cpp
struct Foo;
struct S
{
static int a[];  //声明，不完全类型
static Foo x;    //声明，不完全类型
static S s;      //声明，不完全类型（在自己的定义内）
}；

int S::a[10];    //定义，完全类型
struct Foo {};	 //定义类Foo
Foo S::x;        //定义，完全类型
S S::s;          //定义，完全类型
```
然而，如果声明用 `constexpr` 或 `inline(since c++17)` 指定，成员声明必须是完整类型。
使用 `class T` 的成员 `m` ，这里提供了两种访问形式：限定名 `T::m` 或成员访问表达式 `E.m` 或 `E->m` **E**是 `class T` 的对象或者 `class T` 类型的指针。
```cpp
struct X
{
    static void f();  //声明
    static int n;     //声明
};
 
X g() { return X(); } //一些函数返回X
void f()
{
    X::f();           //X::f是静态成员函数的限定名称
    g().f();          //g().f是成员访问表达式引用静态成员函数
}
 
int X::n = 7;         //定义
void X::f()           //定义 
{ 
    n = 1;            // X::n
}
```
静态成员遵守类成员的访问规则 `(public,protected,private)`。
## 静态成员函数
静态成员函数不与任何对象绑定，当被调用时，它们是没有 `this` 指针；
静态成员函数不可以是 `virtual,const及volatile`;
静态成员函数的指针可以存储在指向函数的常规指针，但不存储在指向成员函数的指针。
## 静态数据成员
静态数据成员不与任何对象绑定，即使没有定义任何类的对象它们也是存在的，除非使用关键字 `thread_local`，否则程序的一个运行周期内仅仅只有一个静态数据成员，如果用了该关键字，每个线程有个具有线程存储持续时间的对象 `(since c++11)`。
静态数据成员不可以是 `mutable`，
在`namespace scope` 的含有静态数据成员的类具有外部链接`（external linkage）`性，如果类本身有外部链接`（external linkage）`性即不是未命名的命名空间。`Local class`(类定义在函数内)或者 `unnamed class`(未命名的类)，包括未命名的成员类，不能具有静态数据成员和。
  
一个静态数据成员可以声明称内联 `(inline)` ,一个内联的静态数据成员可以在类内定义，指定默认初始化，不需要在类外定义：
```cpp
struct X
{
    inline static int n = 1;
};
```
## Constant静态成员
如果一个 `int` 或者枚举类型的静态数据成员被声明成 `const` （不是  `volatile` ）,则可以在类内部初始化，其中每个表达式都是一个常量表达式：
```cpp
struct X
{
    const static int n = 1;
    const static int m{2}; // since C++11
    const static int k;
};
const int X::k = 3;
```
如果 `LiteralType` (文本类)的静态数据成员声明为 `constexpr` ，则必须在类内初始化它，其中每个表达式都是一个常量表达式。
```cpp
struct X {
    constexpr static int arr[] = { 1, 2, 3 };        // 正确
    constexpr static std::complex<double> n = {1,2}; // 正确
    constexpr static int k; // 错误: constexpr static k 要求是一个常量
};

```
如果使用 `const` 非内联（since c++17）静态数据成员或 `constexpr` 静态数据成员（since c++11），则仍需要命名空间范围内的定义，但它不能具有初始化程序。 对于 `constexpr` 数据成员，不推荐使用此定义（since c++17）。
```cpp
struct X {
    static const int n = 1;
    static constexpr int m = 4;
};
const int *p = &X::n, *q = &X::m; // X::n and X::m are odr-used
const int X::n;            		  // … 必须要再进行一次定义，但不能再进行赋值。
constexpr int X::m;         	  // … (except for X::m in C++17)
```