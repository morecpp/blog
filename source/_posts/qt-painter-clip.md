---
title: Qt 绘图裁剪
date: 2019-05-31 21:02:38
categories: "Qt"
tags:
	- Qt
---
最近遇到个让人脑阔疼的问题，怎么把QPainerPath部分路径不进行绘制，这个问题一直困扰我很久，之前也一直不曾解决，这次我想再尝试去解决，毕竟以前菜，以前搞不定现在去试试说不定还有转机，最后终于把问题解决了。我把这个过程给记录下来了。
<!-- more -->
比如像这张图，曲线都已经跑到亮白区的下面了，妥妥的越界不老实，想要治治它怎么办
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/painter-clip/crossing.png)
## 方案一
按照我目前的思维，曲线是基于QPainterPath类来绘制的，肯定先去QPainterPath类里面查看接口，试了大半天好像都没有找到合适的接口，肿么办，自己搞不定肯定是能力不够，要不就接口没看懂要不就哪里出漏字，总之自己来查阅接口失败！
## 方案二
就这么放弃，我当然不甘心，Qt问题我就想到了脏脏鱼大佬跟TM大佬，很快得到TM大佬的建议：
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/painter-clip/operator.png)
心里当时第一反应就是：“卧槽！还有这种操作”。这也许就是我与大佬们之间的差距。既然是重载了 `operator-`，那必然可以减去路径，代码思路：
```cpp
//
QPainterPath path;
//给路径增加点
for (int i = 0；i <= 10; i++)
{
	path << QPointF(i, 2 * i);
}
//减去这条路径上的最后3个点的路径
QPainterPath pathsubtract;
for (int i = 8; i <= 10; i++)
{
	pathsubtract << QPointF(i, 2 * i);
}
//路径相减
path = path - pathsubtract;
//绘制路径
p->darwPath(path);
```
上面只是个范例，除了减去一个开环路径，还可以减去一个闭环路径（例如矩形路径）
可不管怎么样尝试，之后得到这样的效果：
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/painter-clip/bug.gif)
超出的部分虽然被减去了，可是它强行把原来的路径变为闭环了，我想去查看重载减号的说明是什么，最后得到的是请参考Qt 4.5，网上搜来搜去都没搜到这个版本的help文档。也许Qt公司就不想让我知道。
好吧 这个只能算成功了一半，最终无法达到效果，宣判它死刑。嘿嘿，天无绝人之路，下面介绍脏脏鱼大佬给的建议。
## 方案三
裁剪，没错就是裁剪画图。大佬给的截图，裁剪。
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/painter-clip/clip.png)
找了半天我没找到这个接口，“大佬没有这个接口啊，咋回事咧”，答复：“QPainter”，当时心里还是这种反应：“卧槽还有这种操作，大佬果然就是大佬”，当我还这一个围墙里面的时候，大佬已经在墙外找到了答案。直接利用绘图工具设置裁剪区域，代码思路：
```cpp
//设置裁剪区域，也就是你期望的显示区域
p->setClipRect(QRect(0, 0, mlength, mlength));
//把裁剪功能开启
p->setClipping(true);
//绘制你需要绘制的路径
p->drawPath(path);

```
效果图：
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/painter-clip/clip.gif)
这样就达到了你想显示哪些区域就哪些区域，对于哪些越界的曲线，谁敢不听话，管的它妥妥的。






