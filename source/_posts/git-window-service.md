---
title: Win10系统下基于gitblit搭建git服务器
date: 2019-03-10 18:28:21
categories: "Git"
tags: 
   - Git
---
最近刚好有需求需要在windows下搭建一个git服务器，参考了百度上很多篇搭建方法，最后选择用基于java写的gitblit来创建git服务器。
<!-- more -->
## Gitblit的获取:
传送门[http://www.gitblit.com/](http://www.gitblit.com/)(Gitblit官网)，
可以看到1.8.0版本，貌似还是2016年发布的。点击下载windows版本，下载后不用安装，直接解压，我这里是放在：
``D:\Git\gitblit\gitblit-1.8.0`` 路径下
<img src="../image/gitblitcom.png">

## Gitblit所需环境：
### 安装Java JDK
&emsp;&emsp;确保电脑安装java运行时环境，下载地址：[http://www.java.com/zh_CN/](http://www.java.com/zh_CN/)，默认安装记住自己路径即可。

## Java系统环境配置
&emsp;&emsp;“计算机”=>“属性”=>“高级系统设置”=>“高级”=>“环境变量”

<img src="../image/systemenvironment.png">
 
* 新建：<br/> 
变量名： JAVA_HOME <br/> 
变量值： ``C:\Program Files (x86)\Java\jdk1.8.0_172``  [视你的安装路径，我这里是装在C盘]

<img src="../image/java_home.png">

* 新建：<br/> 
变量名： ClassPath 
变量值：%JAVA_HOME%/lib/dt.jar;%JAVA_HOME%/lib/tools.jar （win7可以win10不行）<br/> 
win10下写法，增加两个全路径：

<img src="../image/classpath.png">

* 添加：<br />
找到Path变量：把%JAVA_HOME%/bin;%JAVA_HOME%/jre/binfe;添加到”变量值”的结尾处。用分好隔开。（Win7下）<br />
win10下写法：

<img src="../image/path.png">

* 验证Java运行环境是否安装成功<br />
打开CMD，输入命令 javac 如果弹出如下信息：则表示安装成功！<br />

<img src="../image/cmd.png">

## 创建Git服务器资料存储文件夹
&emsp;&emsp;创建文件夹为：``D:\Git\GitRepository``
<img src="../image/gitfolder.png">

## 配置Gitblit属性
&emsp;&emsp;1、找到``D:\Git\gitblit\gitblit-1.8.0\data\defaults.properties``文件，我这里用“vscode”打开，你也可以用其它可编辑软件打开<br />将``git.repositoriesFolder = D:/Git/GitRepository`` （我们前面创建的Git服务器的资料存储路径）<br />
注意!其中的D:\Git\GitRepository 中的"\"一定要用"/"。

<img src="../image/repositoriesFolder.png">

2、找到server.httpPort，设定http协议的端口号: 8442
<img src="../image/port.png">

3、找到server.httpBindInterface，设定服务器的IP地址。这里就设定你电脑的IP地址：<br />
我电脑的IP地址为（192.168.3.15）
<img src="../image/ip.png">

4、找到server.httpsBindInterface，设定为localhost

<img src="../image/localhost.png">

5、 Ctr + s保存文件，关闭。

## Gitblit批处理文件的修改与执行
&emsp;&emsp;进入文件夹gitblit，找到gitblit.cmd文件，双击运行。
<img src="../image/gitblitcmd.png">

<img src="../image/gitblitcmdrun.png">



## 设置以	Windows Service方式启动Gitblit
&emsp;&emsp;1、在文件夹gitblit\gitblit-1.8.0中，找到installService.cmd文件，打开文件（我这里用记事本)。
<img src="../image/service.png">

2、用“记事本”打开。
修改 ARCH

　　　　32位系统：``SET ARCH=x86``

　　　　64位系统：``SET ARCH=amd64 <br />``
3、 添加 CD 为程序目录<br />
``SET CD=D:\Git\gitblit``(你的实际目录)
<img src="../image/configservice.png">

4、修改StartParams里的启动参数，给空就可以了.
<img src="../image/startp.png">

5、Ctr + s保存，关闭文件。

6、以管理员权限运行installService.cmd。
在计算机服务里面查看有gitblit服务这一栏：
<img src="../image/serviceinfo.png">

ps(如果要卸载这个服务,可以以管理员权限运行CMD，在命令行中输入sc delete gitblit即可卸载)

## 在浏览器中查看gitblit服务器情况：
&emsp;&emsp;在浏览器地址栏中输入：``192.168.3.15:8442``
<img src="../image/show.png">

可以看到安装成功，初始默认账号：admin 密码：admin
