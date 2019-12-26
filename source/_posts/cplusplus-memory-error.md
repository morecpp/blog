---
title: C++ 内存错误信息
date: 2019-09-04 19:03:20
categories: "C++"
tags:
  - C++
---
今天偶尔看到寸衫大佬贴出了一个博客，内存错误对照表，感觉不错，把它记录下来。在进行C++开发时候，由于野指针、空指针、未初始化内存等原因，很容易导致内存错误，并报出特定错误码。
<!--more-->
## Google对上述错误码的解释如下：

* **0xCDCDCDCD** - Created but not initialised 未初始化的堆内存

* **0xDDDDDDDD** - Deleted 引用的内存已经/对象被删除

* **0xFEEEFEEE** - Freed memory set by NT's heap manager 

* **0xCCCCCCCC** - Uninitialized locals in VC6 when you compile w/ /GZ 未初始化的栈内存

* **0xABABABAB** - Memory following a block allocated by LocalAlloc()

&emsp;&emsp;VC++在Debug编译方式编译的程序中，会跟踪用new分配的内存。新分配的内存会用0xcd(助记词为Cleared Data)填充，防止未初始化；当它被delete后，又会被0xdd(Dead Data)填充，防止再次被使用。这样有利于调试内存错误。之所以选这样的填充模式，是因为：

* 大数，若被当成指针就会越界 

* 奇数，指针通常指向偶数地址  

* 非0，这样不会和NULL混淆，在Release版中不会有这些字节填充
## 另外附上~大佬提供的微软 System Error Codes连接
[**https://docs.microsoft.com/en-us/windows/win32/debug/system-error-codes**](https://docs.microsoft.com/en-us/windows/win32/debug/system-error-codes)