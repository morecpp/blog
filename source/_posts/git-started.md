---
title: Git 入门
date: 2019-11-05 20:10:04
categories: "Git"
tags:
	- Git
---
已经一个多月没有写博客，根本原因堕落了，不想为自己找借口。最近因为要团队协同工作了，老的MFC项目是vss管理源代码，这次我们经过讨论想用Git来管理源代码，尝尝新事物未必也是不错的，再说已经久仰Git大名，大名鼎鼎的世界第一搞基网站Gayhub就不用说了吧，为了能尽快用上项目，想在晚上学习下Git的使用。虽然一直在用，但是也就基本会git status、git log、git add*、git remove、git commit -m，看来我还是个git渣渣，很有必要系统的再学一次。因此想把学习过程记录下来，以待将来回顾。
<!--more-->
# Git基础
## 快照，而非差异
&emsp;&emsp;Git与其他版本控制系统最大的不同在于其对待数据的方式。从概念上来说其他大多数版本控制系统以文件变化列表的方式存储信息。Git并没有采用这种方式对待或存储数据。它更像是将数据视为一个微型文件系统的一组快照。每次提交或在Git中保存项目的状态时，Git基本上会抓取一张所有文件当前状态的快照，然后存储一个指向该快照的引用。出于效率的考虑，如果文件并没有发生变动，Git则不会再重新保存文件，而只是留下一个指向先前已保存过的相同文件的链接。Git更多的是将数据作为一个快照流。
<img src="../image/git/started/deltas.gif">
<img src="../image/git/started/snapshots.gif">
这是Git与其他绝大部分版本控制系统的一处主要区别。它使得Git对那些沿袭自上一代版本控制系统的几乎每个方面进行了重新考量。比起单纯的版本控制系统，Git更像是一个微型文件系统。
## 几乎所有的操作都在本地执行
&emsp;&emsp;Git中的大部分操作值需要用到本地文件和资源，一般无需从网络中的其他计算机中获取信息。因为项目完整的历史记录都存放在本地磁盘上，所以绝大多数操作几乎瞬间就能完成。
比如说，浏览项目的历史记录，Git根本不需要到服务器上获取历史信息然后再展示出来，它只需要直接从本地数据库中读取就行了。这意味着我们可以瞬间就能看到项目历史。如果想比较当前本与上个月的版本有什么不同，Git可以找到一个月之前的该文件，然后在本地进行差异计算，既不需要麻烦的远程服务器，也不需要从远程服务器上将旧版本的文件拉取到本地再进行处理。这意味着你在没有网络的地方都可以干活，如果需要上传提交操作，可以等到有网络的时候，完全不会影响工作。
## Git的完整性
&emsp;&emsp;Git中的所有数据在存储前都会执行校验和计算，随后以校验和来引用对应的数据。这意味着不可能在Git不知情的情况下更改文件或目录的内容。这项功能根植于Git的最底层，是其设计理念中不可或缺的一环。只要有Git在，它就能检测出传输过程中丢失的信息或者受损的文件。
Git所采用检验和机制叫SHA-1散列。这是一个由40个十六进制字符（0-9和a-f）所组成的字符串，它是根据文件内容或Git的目录结构计算所得到的。一个SHA-1散列类似于如下的字符串：
  `24b9da6552252987aa493b52f8696cd6d3b00373`
因为用途极广，在Git中到处会看到这种散列值。实际上，Git并不是通过文件名在数据库中存储信息，而是通过信息的散列值。
## Git通常指增加数据
&emsp;&emsp;当你在Git中进行处理时，基本上所有的操作都只是向Git数据库中添加数据。很难让系统无法撤销的操作或把数据搞丢。与其他版本控制系统一样，在Git中你可能会弄丢或弄乱尚未提交的变更，不过一旦向Git提交快照，那就不大可能会丢失了，尤其是在你还会定期向其他仓库推送数据库时。
这使得Git充满了乐趣，因为我们知道自己可以尽情的试验，反正不会有搞砸的风险。
## 三种状态
&emsp;&emsp;现在要注意了，如果你希望一帆风顺地完成余下的学习，一定要记住下面的内容。在Git中，文件可以处于以下的三种状态之一：已提交（committed）、已修改（modified）和已暂存（staged）。已提交表示数据已经被安全地存入本地数据库中。已修改表示已经改动了文件，但尚未提交到数据库。已暂存表示对已经修改的当前版本作出了标识并将其加入下一次要提交的快照中。 
由此便引入了Git项目中三个主要的区域：Git目录、工作目录以及暂存区。
<img src="../image/git/started/areas.gif">
&emsp;&emsp;Git目录是Git保存项目元数据和对象数据库的地方。这是Git最重要的部分，也是从其他计算机克隆仓库时需要复制的内容。
工作目录是项目某个版本的单次检出。这些文件从Git目录下的压缩数据库内被提取出，放置在磁盘上以供使用或修改。
暂存区是一个文件，一般位于Git目录中。它保存了下次所要提交内容的相关信息。有时候它也被称为“索引”，不过通常还是叫作暂存区。
Git的基本工作流程如下：
  （1）修改工作目录中的文件；
  （2）暂存文件，将这些文件的快照加入暂存区；
  （3）提交暂存区中的文件，将快照永久地保存在Git目录中。
&emsp;&emsp;如果一个文件的某个特定版本出现在Git目录中，该版本的文件就被认为处于已提交状态。如果这个文件已被修改，并且已放入暂存区，那么它就处于已暂存状态。如果在上次检出之后文件发生了变更，但并没有被暂存，则处于已修改状态。
## Git安装
&emsp;&emsp;百度吧，直接官网查看怎么操作，实在不行还有一大堆的博客等着你。
# Git的首次配置
## 用户身份
&emsp;&emsp;安装好后的第一件事情就是设置用户名和电子邮件地址。这一步非常重要，因为Git的每一次提交都需要用到这些信息，而且还会被写入到所创建的提交中，不可更改。设置命令如下：
```
$ git config --global user.name "xiaoyuren"
$ git config --global user.email 584699102@qq.com
```
&emsp;&emsp;如果传入--global选项，那只需要设置一次就行了，之后不管在系统中执行什么样的操作，Git都会使用这些已设置好的信息。如果你想在某个项目中使用不同的用户名或电子邮件地址，可以在项目中使用不带--global选项的命令。
## 个人编辑器
&emsp;&emsp;设置好身份信息之后就可以配置默认的文本编辑器了，当Git需要输入消息的时候就会用到这个编辑器。如果没有配置，Git会使用系统默认的编辑器。如果你想使用不同的文本编辑器，比如Emacs，可以执行以下命令。
```
$ git comfig --global core.editor emacs
```
在windows系统中，如果你想使用不同的文本编辑器，例如Notepad++，可以执行下列操作。
在x86系统上：
```
$ git config --global core.editor " 'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession"
```
在x64系统上：
```
$ git config --global core.editor " 'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst"
```
## 检查个人设置
&emsp;&emsp;如果想查看你的设置，可以通过执行`git config --list`命令来列出当前Git可以找出的所有设置，如下所示。
```
C:\Windows\System32>git config --list
core.symlinks=false
core.autocrlf=true
core.fscache=true
color.diff=auto
color.status=auto
color.branch=auto
color.interactive=true
help.format=html
rebase.autosquash=true
http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
http.sslbackend=openssl
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
credential.helper=manager
user.name=xiaoyuren
user.email=584699102@qq.com
```
你可能会多次看到同一个键的输出，这是因为Git会从不同文件（例如/etx/gitconfig和~/.gitconfig）中读取相同的键。这种情况下，Git
会使用这个键最后输出的值。
你可以通过键入git config <key>来查看Git中当前某个键的值，如下所示。
```
$ git config user.name
xiaoyuren
```
## 获取帮助
&emsp;&emsp;如果在使用Git的过程中需要帮助，有以下三种方法可以查看Git任何命令的帮助页面。
```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```
例如，可以通过执行以下命令获得config命令的帮助信息。
```
$ git help config
```
	
	