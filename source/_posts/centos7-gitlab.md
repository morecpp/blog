---
title: Centos7 + Gitlab源代码管理
date: 2020-01-16 15:45:46
categories: "Git"
tags:
	- Git
---
自己搭建一个私服来管理源代码，一直是我想实现的愿望，经常在群里问大佬怎么弄快一年半了，好在最近终于如愿以偿。终于搭建好了自己的源代码管理服务器。
这真是太棒了！之前尝试用过gitblit在windows下搭建过源代码管理器，但始终觉得不是正统，不主流，这次主流了一回。感谢那些在我遇到问题帮我解决问题的
大佬们！现在我决定记录下我这次的安装过程。希望对你我都能有帮助。
<!-- more -->
## 前述
看到网上很多都是虚拟机的，其实我内心是很反感的，要就来一个真实的环境，干嘛要虚拟啊，虚拟毕竟只是模拟，能跟真实一样吗，不喜勿喷！仅代表我个人观点。
所以我决定不装虚拟机。
## 准备工作
* 台式机电脑一台（用来作为服务器，我的这台电脑配置很差，i3很老的。但足够用了）
* U盘一个（8G以上吧，主要是用来制作U盘启动的Centos系统）
## 失败案例
Centos8 + gitlab，很遗憾！失败了！Centos8最近出来的，安装过程跟界面感觉都要比7好很多，总之就是个人体验感觉要好一点，包括它的终端界面黑色的比7白
色的我感觉要更人性化。个人更倾向越高版本越好，但太新了东西也不一定是好事，比如gitlab就没有与之匹配的版本，还有像波音的新飞机，最新的福特级航空母
舰，还有滨海战斗舰都是太先进了，反而问题比较多要去解决。所以有时候太新了也不一定是好事。废话不多说了，开始干活。
## 第一步 制作Centos7系统U盘启动
* 1、进到Centos Download下载界面[**https://www.centos.org/download/**](https://www.centos.org/download/)
<image src="../image/sys/gitlab/centos.png">
<br/>
* 2、点击“CentOS Linux DVD ISO”，之所以选择这个，跟我们U盘要刻录的系统有关，旁边的stream是干什么的我不知道，感兴趣的自己去查一查
* 3、点击`+ others, see the full list of mirrors`后面的连接，看到这里这么多连接，不点是不是很可惜啊，都是Centos8的下载地址，用不了。虽然很馋8代
	Centos，暂且放过它吧。
<image src="../image/sys/gitlab/centos1.png">
<br/>
* 4、进来以后往下翻，选择`Asia china Alibaba Cloud Computing`，也就是阿里巴巴镜像网站，当然你也可以选择其它的镜像网站，殊途同归。
<image src="../image/sys/gitlab/centos2.png">
<br/>
* 5、点进去后，选择`7.7.1908/`版本进行下载，这个是Centos7里面的最新一代。
<image src="../image/sys/gitlab/centos3.png">
<br/>
* 6、百度下载一个Rufus U盘启动制作工具，这个工具小巧，挺好用的。`Free download fow windows`关键字Free，Free的我都挺欣赏的。
	[**https://rufus.en.softonic.com/**](https://rufus.en.softonic.com/)
* 7、把U盘的数据备份好！U盘制作过程中数据会被清空掉。
* 8、从网上偷了一张制作图，自己可以百度制作教程，很简单的。
<image src="../image/sys/gitlab/centos4.png">
<br/>
## 第二步 Centos7安装过程
**可以参照这篇博客来安装[**https://linux.cn/article-11438-1.html?pr**](https://linux.cn/article-11438-1.html?pr)
我这里简单说下：
* 1、将要安装为服务器的电脑设置开机U盘启动
* 2、插入U盘，开机启动电脑会进到如下界面，这个是之前装的centos8拍的照片，电脑屏幕很脏，细节请自动忽略，选第一个安装，"Install CentOS Linux..."
<image src="../image/sys/gitlab/centos5.png">
<br/>
	剩下的照片我当时没有用手机记录，我就从网上找了下别人的图片来冒充，其过程大致是一样的。
* 3、选择中文语言，默认选择中文，这里是安装过程的语言，选择英文也ok
<image src="../image/sys/gitlab/centos6.png">
<br/>
* 4、选择时区，这里记得选一下，选上海，不然默认时间跟北京时间对不上了，不过你选错了也没关系，后面是有办法改的，就像你装windows系统后面也可以改时区的。
* 5、选择安装规模，他这里是选择了“最小安装+带GUI的服务器”，我自己的操作是选择了“最小安装+GNOME界面”有人说这个界面很丑，我也觉得他很丑，不要问我
	为什么要装界面，新手上路，没界面很不习惯，拷贝一个文件都要命令我真的很不习惯，所以界面对我来说还是很大帮助。
<image src="../image/sys/gitlab/centos8.png">
<br/>
<image src="../image/sys/gitlab/centos9.png">
<br/>
* 6、分区操作，比较麻烦对我来说，这个地方就是给操作系统配置一些内存磁盘目录之类的，咱也不太懂也不敢问，就不敢误人子弟。这步配置大家多参考网上的博客。
* 7、开始安装，创建一个根用户，同时会需要你输入密码，这个密码就是最高权限的密码了，root密码。
<image src="../image/sys/gitlab/centos10.png">
<br/>
<image src="../image/sys/gitlab/centos11.png">
<br/>
<image src="../image/sys/gitlab/centos12.png">
<br/>
	安装完成后会有重启向导提示的，当然安装过程中会有ip地址配置的，我当时没有在这里配置，是后面才慢慢学会了怎么在centos配置的，就不在这里赘述。
* 8、拔掉你的u盘媒介，重启电脑后进入到这个画面，选择第一个centos进行启动。
<image src="../image/sys/gitlab/centos13.png">
<br/>
	同意许可：
<image src="../image/sys/gitlab/centos14.png">
<br/>
<image src="../image/sys/gitlab/centos15.png">
<br/>
	完成配置，进入到登录界面，至此安装完成。
<image src="../image/sys/gitlab/centos16.png">
<br/>
## 第三步 Gitlab安装
gitlab下载地址：**[https://packages.gitlab.com/gitlab/gitlab-ce](https://packages.gitlab.com/gitlab/gitlab-ce)**
像如下路的这种6/7/8是对应的操作系统版本，别下错了，一定要配对，不然白费力气，我就吃过这个亏，具体应该下载哪个版本，我也忘记了，这个是难不倒聪明你。
<image src="../image/sys/gitlab/centos20.png">
<br/>
这里说两点：
1、gitlab分为ce版免费（Community Edition）跟ee版（Enterprise Edition）这个企业版好像要收费，我用的是ce版，总之免费的太爽了。
2、gitlab可以在线安装跟离线安装，如果没被（wall）可以在线安装，否则建议离线安装。

**附上官方安装教程：**
**[https://about.gitlab.com/install/](https://about.gitlab.com/install/)**
这里可以看到好多种操作系统下的，有Ubuntu,Debian,Centos6,Centos7,OpenSUSE Leap15.1,Raspberry Pi2
这里点Centos7，多期望它能支持Centos8,主要是我觉得centos8界面更好看。要不了多久一定可以的。
<image src="../image/sys/gitlab/centos21.png">
<br/>
<image src="../image/sys/gitlab/centos22.png">
<br/>
注意它这里的是ee版，我们需要的是ce版。
<image src="../image/sys/gitlab/centos23.png">
<br/>
最后你应该能看到一张类似这样的图了，恭喜你！真正的入坑才开始。
<image src="../image/sys/gitlab/centos24.png">
<br/>
## 用后感
centos7+gitlab真香！gitlab真适合团队开发，源代码又放在自己的服务器上，真的是太喜欢用这个了，目前还用的不太熟悉，比如说我创建了第三个管理员权限的账户失败了，
还有目前不支持路由的访问，如果这些问题都能解决，那真是太棒了，总之感谢gitlab团队的贡献！
附上一些图
<image src="../image/sys/gitlab/centos25.jpg">
<br/>
<image src="../image/sys/gitlab/centos26.jpg">
<br/>
<image src="../image/sys/gitlab/centos27.png">


















