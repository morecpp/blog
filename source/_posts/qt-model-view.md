---
title: QTableView获取当前选中行的字段内容
date: 2019-03-20 08:58:06
categories: "Qt"
tags:
	- Model/View
---
tips:
被TM大佬狠批一顿，东厂需要你这样的人才，西厂也需要你这样的人才
<!-- more -->
![](https://raw.githubusercontent.com/xiaoyuren8/xiaoyuren8.github.io/master/image/Qt/ModelView/tm.png)

获取当前行号：
<pre>
int row = ui-> tableView ->currentIndex().row();
</pre>
获取当前行的指定字段信息：
<pre>
QModelIndex index = pmode->index(row,0);//pmode类型为 QSqlQueryModel* 0为指定字段
int id = pmodel->data(index).toInt();   //已知字段0类型为int了，如果是QString就转换为QString类型
</pre>
