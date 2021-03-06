---
title: VS中的反汇编代码学习
date: 2016-06-02 11:06:16
categories: 汇编
tags:
	- 汇编
	- Visual Studio
---


# 反汇编  #



## 程序在内存中的模型 ##

编译器为程序分配的内存在逻辑上可分为：<font color="brown">代码段、数据段、堆、栈</font>

    代码段： 保存程序文本
    数据段： 保存初始化的全局变量和静态变量
    BSS：	未初始化的全局变量和静态变量
    堆：		动态内存分配，向地址增大的方向增长
    栈：		存放局部变量，向地址减小的方向增长

![](http://images.cnitblog.com/i/432341/201403/250854505149178.png)

<!-- more -->

##  寄存器的作用 ##

- <font color="red">EAX：累加(Accumulator)寄存器，常用于函数返回值</font>
- EBX：基址(Base)寄存器，以它为基址访问内存
- ECX：计数器(Counter)寄存器，常用作字符串和循环操作中的计数器
- EDX：数据(Data)寄存器，常用于乘除法和I/O指针
- ESI：源变址寄存器
DSI：目的变址寄存器
- <font color="green">ESP：堆栈(Stack)指针寄存器，指向堆栈顶部
- EBP：基址指针寄存器，指向当前堆栈底部
- EIP：指令寄存器，指向下一条指令的地址</font>


## 汇编代码常用指令 ##

- add：加法指令，第一个是目标操作数，第二个是源操作数，格式为：目标操作数 = 目标操作数 + 源操作数；

- sub：减法指令，格式同 add；

- call：调用函数，**一般函数的参数放在寄存器中**；

- ret：跳转会调用函数的地方。对应于call，返回到对应的call调用的下一条指令，若有返回值，则放入eax中；

- push：把一个32位的操作数压入堆栈中，这个操作在32位机中会使得esp被减4（字节），esp通常是指向栈顶的

- pop：与push相反，esp每次加4（字节），一个数据出栈。pop的参数一般是一个寄存器，栈顶的数据被弹出到这个寄存器中；

- mov：数据传送。第一个参数是目的操作数，第二个参数是源操作数，就是把源操作数拷贝到目的一份。

- xor：异或指令，这本身是一个逻辑运算指令，但在汇编指令中通常会见到它被用来实现清零功能。
用 xor eax,eax这种操作来实现 mov eax,0，可以使速度更快，占用字节数更少。

- lea：取得第二个参数地址后放入到前面的寄存器（第一个参数）中。
然而lea也同样可以实现mov的操作，例如：`lea edi,[ebx-0ch]`
方括号表示存储单元，也就是提取方括号中的数据所指向的内容，然而lea提取内容的地址，这样就实现了把（ebx-0ch）放入到了edi中，但是mov指令是不支持第二个操作数是一个寄存器减去一个数值的。

- stos：串行存储指令，它实现把eax中的数据放入到edi所指的地址中，同时edi后移4个字节，这里的stos实际上对应的是stosd，其他的还有stosb,stosw分别对应1，2个字节。

- jmp：无条件跳转指令，对应于大量的条件跳转指令。

- jg：条件跳转，大于时成立，进行跳转，通常条件跳转之前会有一条比较指令（用于设置标志位）。

- jl：小于时跳转


### move ax,[bx]

<font color="red">[]表示间接寻址</font>。

bx和[bx]的区别是：
> <font color="red">前者操作数就是bx中存放的数，后者操作数是以bx中存放的数为地址的单元中的数</font>。

比如：
bx中存放的数是`40F6H，40F6H、40F7H`两个单元中存放的数是22H、23H，则：

	move ax，[bx]；//2223H传送到ax中
	move ax，bx；//40F6H传送到ax中

ILT 是静态函数跳转表。

作用是记录了一些函数的入口然后跳过去，每个跳转jmp占一个字节，然后就是一个四字节的内存地址，加起来为五个字节。

### 函数的参数传递方式 ###

函数调用规则指的是调用者和被调用函数间传递参数及返回参数的方法，在Windows上，常用的有
> Pascal方式、WINAPI方式（_stdcall）、C方式（_cdecl）。


1. _cdecl C调用规则

    参数从右到左进入堆栈
    在函数返回后，调用者要负责清除堆栈。
	这种调用方式通常会生成较大的可执行程序。

2. _stdcall又称为WINAPI，调用规则为

	参数从右到左进入堆栈
	被调函数在返回前自行清理堆栈
	
	这种方式生成的代码比cdecl小

3. Pascal调用规则（现在基本不用）

	参数从左到右进入堆栈
	被调函数在返回前自行清理堆栈
	不支持可变参数的函数调用

## 参考文章 ##

1. 浅析VS2010反汇编，[寒山拾得 的专栏]，[http://blog.csdn.net/u013467442/article/details/47060261]()