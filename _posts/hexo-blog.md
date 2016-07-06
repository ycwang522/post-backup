---
title: 使用Github+Hexo搭建博客
date: 2016-05-27 17:08:43
categories: Hexo博客搭建
tags:
	- Github
	- Hexo
	- Node.js
---


## Step0 安装Git

## Step1 安装Node.js
 
该步请自行谷歌，最好选择默认路径，省去配置路径的麻烦。

## Step2 安装Hexo
 
	`npm install -g hexo` 

- 升级hexo到最新版（可以忽略）


	`npm update hexo -g`

这一步时间比较长，听首歌静静等待安装。

- 初始化

    `hexo init <folder>`

指定你的文件夹路径，便会在该文件夹内初始化。

npm会自动安装依赖环境

	 `npm install`

<!-- more -->

执行完这一步，在你的hexo安装的路径下运行

	`hexo s`

生成的静态博客便可在本地运行了。

运行debug可以本地调试。

	`hexo s --debug`

-----


## 改变主题

更换主题以Next主题为例进行说明。

详细过程参考：[Next](http://theme-next.iissnan.com/)

	`hexo new `