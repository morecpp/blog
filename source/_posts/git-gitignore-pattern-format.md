---
title: Git 忽略文件
date: 2019-04-12 20:54:21
categories: "Git"
tags:
	- Git
---
一般总会有些文件无需纳入Git的管理，也不希望它们总出现在未跟踪的列表。通常都是些IDE自动生成的文件，比如编译过程的临时文件，日志文件等等。这种情况我们可以创建一个名为.gitignore的文件，列出要忽略的文件，
<!--more-->
# 格式规范（PATTERN FORMAT）
* 空行不匹配任何文件，增加可读性，会被自动忽略
* 支持标准的glob模式
* 以 `#` 开头的行作为注释行会被自动忽略
* 以 `！`开始的模式表示取反 ，要忽略指定模式以外的文件或目录。
* 以 `/` 开头的模式可用于禁止递归匹配
* 以 `/` 结尾的模式表示目录，匹配指定目录，`build/` 表示忽略目录 `build` 下的所有文件
* `/` 与路径名的开头匹配, `/*.c`忽略 `cat.c`，不忽略  `build/cat.c` 
* `**/`表示匹配所有的目录，例如：`**/foo`在任何地方匹配

glob模式类似于shell所使用的简化版正则表达式。具体讲，具体讲 `*` 匹配零个或多个字符。`[abc]` 匹配方括号内的任意单个字符（在这个例子里是a,b,c）,而 `?` 则匹配任意单个字符。在方括号使用短划线分隔两个字符（例如 `[0-9]`）的模式能够匹配在这两个字符范围内的任何单个字符（在这个例子里是0到9之间的任何数字0）。还可以用两个 `**` 匹配嵌套目录，比如 `a/**/z`能够匹配`a/z`、`a/b/z`和`a/b/c/z`等。

## 下面是一个.gitignore例子:
```
*.a             #忽略.a类型文件
!lib.a          #仍然跟踪lib.a,即使上一行指令要忽略.a类型文件
/TODO           #只忽略当前目录的TODO文件，而不忽略子目录下的TODO
build/          #忽略build/目录下所有文件
doc/*.txt       #忽略doc/notes.txt,而不忽略doc/server/name.txt
doc/**/*.pdf    #忽略doc/目录下的所有.pdf文件

```
** 再给出一个C++的.gitignore**
```html
# Prerequisites
*.d

# Compiled Object files
*.slo
*.lo
*.o
*.obj

# Precompiled Headers
*.gch
*.pch

# Compiled Dynamic libraries
*.so
*.dylib
*.dll

# Fortran module files
*.mod
*.smod

# Compiled Static libraries
*.lai
*.la
*.a
*.lib

# Executables
*.exe
*.out
*.app
```
## 温馨提示：
**如果你想知道更多种语言的.gitignore怎么写，可以去github网站上查看，这里收集了几十种忽略文件模板。
一组非常有用的.gitignore模板：**[https://github.com/github/gitignore](https://github.com/github/gitignore)