---
title: Git 移动文件或目录操作
date: 2019-11-29 09:42:20
categories: "Git"
tags: 
   - Git
---
随着我项目文件的移动，Git也会因此变动，我不想移除，再把这个文件作为一个新的文件跟踪监视。我不想这样操作，不过 Git 非常聪明，它会推断出究竟发生了什么。改变名字轻松的识别，轻松的移动文件位置。
<!-- more -->
## git mv
`git-mv - Move or rename a file, a directory, or a symlink（符号链接）`
## 概要
```
git mv <options>…​ <args>…​
```
## 描述
```
git mv [-v] [-f] [-n] [-k] <source> <destination>
git mv [-v] [-f] [-n] [-k] <source> ... <destination directory>

-f
--force
Force renaming or moving of a file even if the target exists

-k
Skip move or rename actions which would lead to an error condition. An error happens when a source is neither existing nor controlled by Git, or when it would overwrite an existing file unless -f is given.

-n
--dry-run
Do nothing; only show what would happen

-v
--verbose
Report the names of files as they are moved.
```
## 子模块
```
Moving a submodule using a gitfile (which means they were cloned with a Git version 1.7.8 or newer) will update the gitfile and core.worktree setting to make the submodule work in the new location. It also will attempt to update the submodule.<name>.path setting in the gitmodules[5] file and stage that file (unless -n is used).

```
## BUGS
```
Each time a superproject update moves a populated submodule (e.g. when switching between commits before and after the move) a stale submodule checkout will remain in the old location and an empty directory will appear in the new location. To populate the submodule again in the new location the user will have to run "git submodule update" afterwards. Removing the old directory is only safe when it uses a gitfile, as otherwise the history of the submodule will be deleted too. Both steps will be obsolete when recursive submodule update has been implemented.

```

## 使用
```
$ git mv  a.cpp  b.cpp           //将a.cpp 文件改名为 b.cpp
$ git mv  a.cpp  inlucde/a.cpp   //将a.cpp 移动到目录include下
$ git mv  src    include         //把src目录下的文件移动到include目录下，并且会将src目录文件名也删除掉

```
