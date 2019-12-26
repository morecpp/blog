---
title: Git 基础（一）
date: 2019-11-06 18:13:45
categories: "Git"
tags:
	- Git
---
Git学习基础
<!--more-->
## 获取Git仓库
&emsp;&emsp;建立Git项目的方法主要有两种。第一种是把现有的项目或者目录导入到Git中，第二种是从服务器上克隆现有的Git仓库。
## 在现有目录中初始化Git仓库
&emsp;&emsp;要想在Git中对现项目进行跟踪管理，只需进入项目目录并输入：
```
$ git init
```
&emsp;&emsp;这会创建一个名为.git的子目录。这个子目录包含了构成Git仓库骨架的所有必需文件。但此刻Git尚未跟踪项目的任何文件。如果需要跟踪文件并提交。对需要跟踪的文件执行几次`git add`命令，也可以`git add*`一次添加当前目录的所有文件，然后输入`git commit`命令即可：
```
$ git add *.c
$ git add LICENSE
$ git commit -m "initial project version"
```
## 克隆现有仓库
  &emsp;&emsp;如果需要获取现有仓库的一份副本（比如你想参与的一个项目），可以使用git clone命令。如果你熟悉其它的版本控制系统，就会注意到这个命令是"clone"而不是"check out"。这是一个很重要的差异。因为Git会对服务器仓库的几乎所有数据进行完整复制，而不只是复制当前的工作目录。git clone默认会从服务器上把整个项目历史中的每个文件的所有历史版本都拉取下来。如果服务器磁盘坏了，数据会丢失，但是可以通过本地Git项目的副本恢复服务器。
  克隆仓库需要用到git clone [url]命令。例如我们常会在Github上面下载开源项目，
```
git clone https://github.com/xiaoyuren8/assimp.git
```
这样会在本地创建一个名为assimp的新目录，并在其中初始化.git目录，然后将远程仓库中的所有数据拉取到本地并检出最新版本的可用副本。如果想将项目克隆到其它名字的目录中，可用把目录名作为命令行选项传入：
```
$ git clone https://github.com/xiaoyuren8/assimp.git myassimp
```
&emsp;&emsp;这一条命令与上一条命令功能相同，只是目标目录的名称变成了myassimp。
Git可用用几种不同的协议传输数据。上一个例子使用的是https://协议，除此之外也可以使用git://协议或者是SSH传输协议（如user@server:path/to/repo.git）。
## 在Git仓库中记录变更
&emsp;&emsp;你现在拥有了一个正真的Git仓库并检出了项目文件的可用副本。下一步就是做出一些更改，当项目到达某个需要记录的状态时间向仓库提交这些变更的快照。请记住，工作目录下的每一个文件都处于两种状态之一：已跟踪（tracked）或未跟踪（untracked）。已跟踪的文件是指上一次快照中包含的文件。这些文件可以分为未修改、已修改或已暂存三种状态。而未跟踪的文件则是工作目录中除去已跟踪文件之外的所有文件，也就是既不在上一次快照中，也不在暂存区中文件。当你刚刚完成仓库克隆时， 所有文件的状态都是已跟踪未修改的，因为你刚把它们检出，而没有做过任何改动。如果修改了文件，他它们在Git中的状态就会变成已修改，这意味着自从上次提交以来文件已经发生了变化。你接下来要把这些已修改的文件添加到暂存区，提交所有已暂存的变更，随后重复这个过程。
<img src="../image/git/base/lifecycle.png">
## 查看当前文件状态
&emsp;&emsp;检查文件所处状态的主要工具是git status命令。如果在克隆仓库后立即执行这个命令，就会看到类似下面的输出：
```
$ git status
On branch master
nothing to commit, working directory clean
```
  上述输出说明你的项目工作目录是干净的。也就是说，工作目录下没有任何已跟踪的文件被修改过。Git也没有找到任何未跟踪的文件，否则这些文件会被列出。最后该命令还会显示当前所处的分支，告诉你现在所处的本地分支与服务器上的对应分支没有出现偏离。就目前而言，我们一直处于master分支。现在不需要担心分支的问题。后面会做介绍。
  现在，让我们把一个简单的README文件添加到项目中。如果之前不存在这个文件，那么这次执行git status就会看到这个未跟踪的文件：
```
$ echo 'My Project' > README
$ git status
On branch master
Untracked files:
 (use "git add <file>..." to include in what will be committed)
	
 	README

  nothing added to commit but untracked files present (use "git add" to track)
```
&emsp;&emsp;可以看到，新的README文件处于未跟踪的状态，因为git status输出时把这个文件显示在"Untracked files"（未跟踪的文件）条目下。未跟踪的文件就是在Git在上一次快照（提交）中没有发现的文件。Git并不会主动把这些文件包含到下一次提交的文件范围中，除非你明确告诉Git你需要跟踪这些文件。这样做就是为了避免你不小心把编译生成的二进制文件或者其它你不想跟踪的文件包含进来。需要让Git跟踪该文件，才能将README加入。
## 跟踪新文件
&emsp;&emsp;可以使用git add命令Git开始跟踪新的文件。执行以下命令来跟踪README文件：
```
$ git add README
```
此时如果重新执行查看项目状态的命令，就可以看到README文件已处于跟踪状态，并被添加到暂存区等待提交：
```
$ git status
On branch master
Changes to be committed:
 (use "git reset HEAD <file>..." to unstage)

	new file: README

```
&emsp;&emsp;在"Changes to be commited"（等待提交的更改）标题下列出的就是已暂存的文件。如果现在提交，那么之前执行git add时的文件版本就会被添加到历史快照中。回想一下，在早先执行git init时，你执行的下一个命令就是git add （files），这个命令就是让Git开始跟踪工作目录下的文件。git add命令接受一个文件或目录的路径名作为参数。如果提供的参数是目录，该命令会递归地添加该目录下的所有文件。
## 暂存已修改的文件
&emsp;&emsp;这次让我们来更改一个已跟踪的文件。加入你更改了之前已经被Git跟踪的helloworld.md文件，此时再执行git status命令，会看到类似下面的输出：
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   helloworld.md

no changes added to commit (use "git add" and/or "git commit -a")

```