---
title: python爬虫实战
date: 2016-10-15 10:46:11
categories: python
tags:
	- python
	- 爬虫
---

# 爬虫分析过程

![爬虫分析过程](https://ituku.tk/di/3J7VW/-2.png)

## 待爬取目标分析

- 目标：百度百科python词条的相关此条网页——标题及简介


- 入口页：http://baidu.baidu.com/view/21087.htm


- URL格式：

  - 词条页面URL：/view/125370.htm

- 数据格式：

  - 标题：

  ```python
  <dd class="lemmaWgt-lemmaTitle-title"><h1>***</h1></dd>
  ```

  - 简介：


```python
<div class="lemma-summary">***<div>
```
- 页面编码：UTF-8

# 实例代码

