---
title: Git基本指令
date: 2016-05-27 16:18:08
categories: Git
tags:
	- Git
---

# Git使用教程

- 设置用户名：`git config --global user.name "Your Name Here"`  

- 设置邮箱：`git  config --global user.email "your email"`

在网页上新建`repository`，点击如下图所示的网址
![](http://img.my.csdn.net/uploads/201309/28/1380301612_9211.png)

<!-- more -->

- 克隆项目到本地：
`git clone https://github.com/gavincook/test.git  `

- 提交代码：
通过`git status`可以查看相应的改动
![](http://img.my.csdn.net/uploads/201309/28/1380301997_8293.png)
然后使用`git add`将改动加至缓存区
![](http://img.my.csdn.net/uploads/201309/28/1380302204_4558.png)

当有多项改动需要add时，输入

    git add -A

即可全部添加至缓存区。

	git commit -a -m 'freehao123.com'

然后使用`git commit -m "备注信息"` 将改动提交到本地仓库
![](http://img.my.csdn.net/uploads/201309/28/1380302204_8950.png)
最后将代码提交到远程服务器
![](http://img.my.csdn.net/uploads/201309/28/1380302204_8928.png)

至此，就完成了一次环境搭建和代码的提交。

- 从远程克隆项目：从ycwang522仓库克隆ycwang522.github.io项目到本地。

	git clone git@github.com:ycwang522/ycwang522.github.io.git

- 创建一个gitcafe-pages的分支，并切换到该分支

	git checkout -b gitcafe-pages

-----
# 创建项目后快速初始化仓库

	mkdir ycwang522
	cd ycwang522
	git init
	echo "# ycwang522" >> README.md
	git add README.md
	git commit -m "first commit"
	git remote add origin git@git.coding.net:ycwang522/ycwang522.git
	git push -u origin master


# 创建版本库 #
版本库，即仓库（repository），这个文件夹目录中的所有文件夹都可以被Git管理起来，其中的每个文件可以修改、删除、Git都能追踪。

- 选择文件夹，创建一个空目录

    $mkdir learngit	//新建名为learngit的仓库
    $cd learngit	//进入该仓库
    $pwd 			//查看该仓库在本地的路径

`pwd`命令用于显示当前目录

- 新建一文件，比如说readme.md
- 用命令`git add`将文件添加到仓库

`$git add readme.md`

- 用命令git commit告诉git，将文件提交到仓库。

`$git commit -m "add a readme file"`

- &git add 添加多个文件时，可以写成"git add *"或者"git add -A"

# 版本管理及文件控制 #

- 查看当前仓库的状态：$git status 
- 查看被修改的文件： $git diff readme.md 

## 版本回退

- 显示提交日志：git log
- 版本回退： git reset

## 工作区和暂存区

- git add 命令实际上就是要提交所有的修改到暂存区，然后git commit就可以一次性把暂存区的修改提交到分支。
- 一旦提交后，如果对工作区未做修改，那么工作区就是干净的。
- 丢弃工作区的修改：git checkout --file

## 删除文件
- 直接在文件管理器中把没用的文件删掉：rm test.md
- 从版本库中删除该文件：git rm test.md，然后git commit
- 如果删错了，但是版本库里还有，所以可以将误删掉的文件恢复到最新版本： git checkout -test.txt

# 远程仓库
- 创建SSH密钥。

	ssh-keygen -t rsa -C "youremail@example.com"
- 在.ssh文件夹中可以找到id_rsa和id_rsa.pub两个密钥。
- 登陆Github，打开SSH key配置页面，将id_rsa.pub中的内容粘贴进去即可。
- Github允许多个SSH。


# 版本增加及提交 #
- 本地仓库添加到远程仓库中：git remote add origin git@github.com:ycwang522/ycwang522.git
- 将本地库的所有内容推送到远程仓库上：git push -u origin master

将本地的内容推送到远程，用git push命令，实际上是将当前分支master推送到远程。
由于远程是空的，第一次推送master分支时，加上了-u参数，Git不但会将本地的master分支内容推送到远程的master分支，还会将两个master分支关联起来，在以后的推送或者拉取时就可以简化命令。

- 然后，在本地提交：git push origin master

将本地的master分支的最新修改推送至github。




# 分支管理 #

## 创建与合并分支

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

-----
<!--
git branch 查看本地所有分支
git status 查看当前状态 
git commit 提交 
git branch -a 查看所有的分支
git branch -r 查看远程所有分支
git commit -am "init" 提交并且加注释 
git remote add origin git@192.168.1.119:ndshow 增加一个远程服务器端
git push origin master 将文件给推到服务器上 
git remote show origin 显示远程库origin里的资源 
git push origin master:develop
git push origin master:hb-dev 将本地库与服务器上的库进行关联 
git checkout --track origin/dev 切换到远程dev分支
git branch -D master develop 删除本地库develop
git checkout -b dev 建立一个新的本地分支dev
git merge origin/dev 将分支dev与当前分支进行合并
git checkout dev 切换到本地dev分支
git remote show 查看远程库
git add . 添加当前目录
git rm 文件名(包括路径) 从git中删除指定文件
git clone git://github.com/schacon/grit.git 从服务器上将代码给拉下来
git config --list 看所有用户
git ls-files 看已经被提交的
git rm [file name] 删除一个文件
git commit -a 提交当前repos的所有的改变
git add [file name] 添加一个文件到git index
git commit -v 当你用－v参数的时候可以看commit的差异
git commit -m "This is the message describing the commit" 添加commit信息
git commit -a -a是代表add，把所有的change加到git index里然后再commit
git commit -a -v 一般提交命令
git log 看你commit的日志
git diff 查看尚未暂存的更新
git rm a.a 移除文件(从暂存区和工作区中删除)
git rm --cached a.a 移除文件(只从暂存区中删除)
git commit -m "remove" 移除文件(从Git中删除)
git rm -f a.a 强行移除修改后文件(从暂存区和工作区中删除)
git diff --cached 或 $ git diff --staged 查看尚未提交的更新
git stash push 将文件给push到一个临时空间中
git stash pop 将文件从临时空间pop下来
---------------------------------------------------------
git remote add origin git@github.com:username/Hello-World.git
git push origin master 将本地项目给提交到服务器中
-----------------------------------------------------------
git pull 本地与服务器端同步
-----------------------------------------------------------------
git push (远程仓库名) (分支名) 将本地分支推送到服务器上去。
git push origin serverfix:awesomebranch
------------------------------------------------------------------
git fetch 相当于是从远程获取最新版本到本地，不会自动merge
git commit -a -m "log_message" (-a是提交所有改动，-m是加入log信息) 本地修改同步至服务器端 ：
git branch branch_0.1 master 从主分支master创建branch_0.1分支
git branch -m branch_0.1 branch_1.0 将branch_0.1重命名为branch_1.0
git checkout branch_1.0/master 切换到branch_1.0/master分支
------------------------------------------------------------------
git branch 删除远程branch
git push origin :branch_remote_name
git branch -r -d branch_remote_name

-->
