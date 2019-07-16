---
title: C++ final关键字
date: 2019-05-05 20:26:45
categories: "C++"
tags:
  - C++
---
有时候我们会定义这样一种类，我们不希望其它类继承它，或者不想考虑它是否适合作为一个基类。 在 `c++11` 标准提供了一种防止继承发生的方法，即在类名后跟一个关键字 `final` ：
<!--more-->
```cpp
class NoDerived final {/*...*/}; //类NoDerived不能作为基类
class Base {/*...*/};
//Last 是final的；我们不能继承Last
class Last final : Base {/*...*/}; //Last不能作为基类
class Bad : NoDerived {/*...*/};   //错误：NoDerived是final的
class Bad2 : Last {/*...*/};       //错误：Last是final的
```