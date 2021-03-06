---
title: 操作系统学习（三）：线程
date: 2016-06-30 17:07:22
categories: 操作系统
tags:
	- 操作系统
---



# 线程 #

同一个地址空间里面的所有线程构成了进程

线程模式下，一个进程至少有一个线程，也可以有多个线程。

## 线程管理 ##

线程管理：线程控制表/线程控制块

能被线程共享的资源存放在进程控制块中，不能被线程共享的资源存放在线程控制块中。

为了提高协作，希望线程间可以共享的资源越多越好。

一般的标准是：如果某资源独享会导致线程运行错误，则该资源就由线程独享，反之，如果不发生错误，就共享。

<!-- more -->

表 一般情况下线程共享和独享资源划分

| 线程共享资源 | 线程独享资源 |
|:---:|:---:|
|地址空间|程序计数器|
|全局变量|寄存器|
|打开的文件|栈|
|子进程|状态字|
|闹铃||
|信号及信号服务程序||
|记账信息||

## 线程的实现方式 ##

线程的管理出现两种形式

- 进程管理线程：用户态线程实现
- OS管理线程：内核态线程实现

这也是线程实现的两种方式。

既然每个线程是不同的执行序列，说明线程应该是CPU调度的基本单位。

### 内核态线程实现

优点：用户编程保持简单；如果一个线程执行阻塞操作，OS很容易的可以调度另一个线程去执行。

缺点：
- 效率低。（每次的线程调度时陷入内核态实现的，由操作系统进行调度）；内核态实现会占用稀缺的内存资源。随着线程数的增加，OS的内核空间将迅速耗尽；
- 内核空间满了：采取的错误最好是让它“停止运行”。
- 内核态实现需要修改操作系统

基于以上可怕的缺点，就引入用户态线程实现。

### 用户态线程实现

# 线程通信
### 管道
管道创建：
使用系统调用popen()或者pipe()

popen()需要提供一个目标进程做为参数，然后在调用该函数的进程和给出的目标进程之间创建一个管道。

一个重要特点：使用管道的两个线程之间存在某种联系 

### 记名管道

使用场景：两个不相关的线程，比如两个不同进程中的线程，之间进行管道通信，则需要使用记名管道。

使用方式：一个线程通过创建一个记名管道后，另外一个线程可使用open来打开这个管道（无名管道则不能使用open操作），从而与另外一端进行交流。

### 套接字

套接字（socket）	是另外一种可用于进程间通信的机制。

socket的功能非常强大，可以支持不同层面、不同应用、跨网络的通信。

使用方法：需要双方均创建一个套接字，一方作为服务器方，另外一方作为客户方。服务器方必须先创建一个服务区套接字，然后再该套接字上进行监听，等待远方的连接请求。

### 信号

可以改善管道和套接字的一些缺点，比如连接需要确认和消耗系统资源，连接需要对方确认。

信号的实质：内核对象 & 内核数据结构

使用方式：发送方将数据结构填好，指明信号的目标进程后，发出特定的软中断。OS收到中断请求之后，只要有进程要发送信号，于是到特定的内核数据结构里查找信号接收方，并进行通知。

### 信号量

实质是一个简单整数。

一个进程在信号变为0或者1的情况下推进，并且将信号变为1或者0来防止别的进程推进。当进程完成任务后，则将信号再改变为0或1，从而允许其他进程执行。

通信机制 && 同步机制


### 共享内存

两个进程share共享同一片内存。这篇内存中的任何内容，二者均可以访问。

缺点：
- 管理复杂，且两个进程须在同一台物理机器上才能使用这种通信方式。
- 安全性脆弱。

---