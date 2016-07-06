---
title: 使用jekyll+Github搭建个人独立博客
date: 2016-05-27 16:06:02
categories: Jekyll博客搭建
tags:
	- Github
	- Jekyll
---

# step 1.选择GitHub作为博客服务器
*更详细的方法参考[<font color="blue">GitHub Pages官方文档</font>](https://pages.github.com/)


- 注册[<font color="blue">GitHub</font>](https://github.com)
登陆https://github.com，创建属于自己的GitHub帐号（废话）。
![](https://ituku.tk/di/2V1WO/1.jpg)

- 建立一个仓库

<!-- more -->

![](https://ituku.tk/di/NBP6X/1240.png)

> Repository name(仓库名)必须是 your_user_name.github.io

比如我的用户名是admin，那么仓库的名称就必须是`admin.github.io`  ，这点灰常重要。

另外新建仓库时选择`Initialize this repository with a README`和`.gitignore`和`Licenses`，如下图所示

![](http://i4.buimg.com/0aef34487476c2a8.png)

一种简单的方式是，下载GitHub的桌面客户端，安装完成后，粘贴
[<font color="blue">http://github.com/your_user_name/your_user_name.github.io.git</font>](http://github.com/your_user_name/your_user_name.github.io.git) 克隆到本地。

<转自[<font color="blue">“授人以渔”的教你搭建个人独立博客</font>](http://www.jianshu.com/p/8f843034c7ec)>

- 在新建的仓库中新建一个index.html文件或者本地新建采取上传的方式添加到新建的仓库
- 
![](http://i4.buimg.com/1e7a0d67fb80afe1.png)

index.html中应包含如下代码：


```html
<?

<!DOCTYPE html>
	<html>
		<body>
		<h1>Hello World</h1>
		<p>I'm hosted with GitHub Pages.</p>
		</body>
	</html>

?>
```

上传完毕之后仓库中应至少包含如下4个文件：`gitignore`,`LICENSE`，`README.md`和`index.html`

![这里写图片描述](http://i4.buimg.com/240b58bd1d423f3f.png)

至此，your_name.github.io就可以飞起来了。

# step 2.搭建本地运行环境jekyll

以下以windows用户为例进行说明。

- 安装Ruby，移步[<font color="blue">Ruby下载页中国镜像站</font>](https://cache.ruby-china.org/pub/ruby/)。选择高稳定版本即可。安装时勾选如下对话框。

![](https://ituku.tk/di/D9OI8/baidushurufa-2016-5-25-11-17-20.png)

- 安装[<font color="blue">rubygems</font>](https://rubygems.org/pages/download)，解压后运行`setup.rb`文件。

- 安装jekyll：

> cmd运行`gem install jekyll`

- 运行jekyll

进入你的blog文件夹：`cd your-site-name`

运行本地服务： `jekyll s`

![](https://ituku.tk/di/L7KP9/baidushurufa-2016-5-25-11-25-31.png)

在浏览器中运行`http://127.0.0.1:4000/` 即可在本地运行调试。


# step 3.发表文章

文章开头，加上如下所示的代码：

    ---
    layout: post
	title: Welcome to Jekyll!
    date: 2014-01-27
    categorie: Blog
	tags:jekyll
    ---

另外，文件的名称也移动得加上时间标题。

比如该文件的标题可以设置为：


    2016-05-25-build-blog.md

然后将文章放入 **_post**文件夹下即可。

# step 4.定制域名

- 购买域名：万网，Godaddy都可以。万网比较便宜。

- 在github文件夹内新建一个`CNAME`文件，写入你的域名，不加`https://`.

- 域名解析，可以选择[Dnspod](https://www.dnspod.cn/Login?default=email)或者万网的域名解析。
 
 以万网域名解析为例：进入管理控制台>云解析>域名后边有**解析**>进入解析

![](https://ituku.tk/di/TCSSY/baidushurufa-2016-5-25-11-38-10.png)

接着 cmd ，输入

    ping your_name.github.io

得到一个ip地址。

然后在上图域名解析中，点击**<font color="blue">`添加解析`</font>**

![](https://ituku.tk/di/E359N/baidushurufa-2016-5-25-11-49-10.png)

在**<font color="blue">`主机记录`</font>**一栏填写"www"，记录值一栏填写刚才刚才ping到的ip地址。

在添加一个解析，同上，**<font color="blue">`主机记录`</font>**一栏填写"@"，记录值一栏填写刚才ping到的ip地址。

这样，当你输入 `your_name.github.io` 的时候会被解析到你设定的网址。

至此，博客基本搭成。
