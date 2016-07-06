---
title: Gitcafe使用笔记
date: 2016-06-03 20:45:44
categories: Gitcafe
tags:
	- Git
	- Gitcafe
---

# gitcafe使用小记

这是原来的域名，https://gitcafe.com/。
后来gitcafe被合并到 Coding.net。
所以新的域名是：https://coding.net/

## 注册帐号 ##
- 新建项目，可以选择public或者private。我选择的是public。

## 配置SSH ##

1. 下载[Git客户端](https://git-scm.com/download/)，安装。
2. 在Git上配置SSH密钥。
3. 启动Git，生成一个存放SSH的文件夹。
	`mkdir ~/.ssh`
4. 生成Gitcafe的SSH密钥。

<!-- more -->
```git
ssh-keygen -t rsa -C "your_email@email.com" -f ~/.ssh/gitcafe
```
将`your_email@email.com`换成你的邮箱。

5. 生成过程中，如果你不输入passphrase口令，可以直接回车。
 ![](https://ituku.tk/di/P3VV9/baidushurufa-2016-6-3-10-20-7.png)

6. SSH密钥生成结束后，打开存放SSH的文件夹，可以看到私钥gitcafe和公钥gitcafe.pub这两个文件。

![](https://ituku.tk/di/J2AR1/baidushurufa-2016-6-3-10-24-24.png)

7. 生成配置文件。

	touch ~/.ssh/config

![](https://ituku.tk/di/PCMFA/1.png)

8. 打开该配置文件，复制一下内容到该config文件，保存。

```
Host git.coding.net
    IdentityFile ~/.ssh/gitcafe
```
![](https://ituku.tk/di/YZUWU/baidushurufa-2016-6-3-10-28-15.png)

## 链接Gitcafe使用Git管理代码 ##

1. 打开之前生成的公钥文件gitcafe.pub，复制里边的内容。
2. 进入Gitcafe>账户>SSH公钥。

将复制的内容添加到`SSH-RSA公钥内容`下的框里。
![](http://i.imgur.com/jwAHl8H.png)

3. 测试是否连接到Gitcafe服务器。

```
ssh -T git@git.coding.net -i ~/.ssh/gitcafe
```
如果设置成功，则有如下提示
![](http://i.imgur.com/Nj4rPM1.png)

4. 设置用户名和邮箱，并且touch-add-commit-push

<font size="3" color="green">全局设置</font>:

```
	git config --global user.name "yourname"
	git config --global user.email youremail@email.com
```

接下来:在本地创建新的 Git 仓库 //如果你想在特定的文件夹下新建的话可以先cd进入文件夹，然后再执行以下命令。

    mkdir ycwang522
    cd ycwang522
    git init
    touch README.md
    git add README.md
    git commit -m 'first commit'
    git remote add origin https://git.coding.net/ycwang522/ycwang522.git
    git push -u origin master


这样，将新建的README.md文件就推送到了Gitcafe上ycwang522仓库了。

![](http://i.imgur.com/wR4F6eH.png)

使用Gitcafe Pages搭建个人空间

1. 创建一个gitcafe-pages的分支，并切换到该分支。

> git checkout -b gitcafe-pages

	
