---
title: Qt-设置Qt应用程序图标
date: 2016-05-30 10:18:17
categories: Qt
tags:
	- Qt
---

- 准备一个icon文件，放置到project文件夹下，例如：First_Qt.ico
- 新建一文件，后缀改为*rc文件，文件内容为：`IDI_ICON1 ICON DISCARDABLE "First_Qt.ico"`
- 在Qt工程的*pro文件中最后新添加一行：`RC_FILE += First_Qt.rc`
- 最后，在`mainwindow.cpp`文件中的构造函数中，添加一行代码`setWindowIcon(QIcon("First_Qt.ico"));`
- 保存，运行，即可。此时，程序运行时的图标就变成我们设置的图标了。

<!-- more -->