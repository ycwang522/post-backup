---
title: QT生成的exe发布方式——windeployqt
date: 2016-05-30 10:20:18
categories: Qt
tags:
	- Qt
	- windeployqt
---

- 使用release方式编译生成*exe文件
- 新建一文件夹，将该exe文件放入该文件夹中。
- 从开始菜单打开命令行E:\Qt\Qt5.6.0\5.6\mingw49_32>
- 输入命令 ：cd /d  E:\Qt Project\1
         然后使用 windeployqt 工具命令：
     <font color=red>**windeployqt DuoluDesktop.exe**</font>
- 新建的文件夹中就会有一系列exe运行所需的文件。

<!-- more -->