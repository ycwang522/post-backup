---
title: jekyll安装过程中可能会遇到的一些错误及解决办法
date: 2016-05-27 16:20:41
categories: Jekyll
tags:
	- Jekyll
	- Github
---

# 错误1

> Dependency Error: Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-paginate' If you run into trouble, you can find helpful resources at Getting Help

> jekyll 3.1.2 | Error: jekyll-paginate

解决办法：安装jekyll时候直接运行`gem install jekyll-paginate`即可解决。

<!-- more -->

# 错误2

    Dependency Error: Yikes! It looks like you don't have redcarpet or one of its
    dependencies installed. In order to use Jekyll as currently configured, you'll n
    eed to install this gem. The full error message from Ruby is: 'cannot load such
    file -- redcarpet' If you run into trouble, you can find helpful resources at ht
    tp://jekyllrb.com/help/!
      Conversion error: Jekyll::Converters::Markdown encountered an error while conv
    erting '_posts/2014-12-28-first-blog.md':
    redcarpet
     ERROR: YOUR SITE COULD NOT BE BUILT:
    ------------------------------------
    redcarpet

解决办法：_config.yml中的markdown改为kramdown
