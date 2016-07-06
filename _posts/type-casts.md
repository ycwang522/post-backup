---
title: 强制类型转换
date: 2016-06-02 11:03:29
categories: C++
tags:
	- C++
	- 类型转换
---

首先看一道例子：

```cpp
#include <stdio.h>
#include <stdlib.h>
int main()
{
	int a =6336;
	char *b = (char *)&a;
	printf("%d",*b);
	getchar();
	return 0;
}
```
输出结果为：-64。

暂且不分析上边的题目，从内存模型入手分析。

C中，每一个变量都对应一个地址。地址空间中的值即0或1的高低电平。

<!-- more -->

在内存中，存储的值不是0就是1.而变量解析时，是通过起始地址和变量类型来解析的。在计算机内存中，是没有「类型」这个概念的，类型如何定义只是语言层面的事情。

所以数据在内存中的存储是没有区别的，有区别的仅仅是对数据的处理方式不同。

例如，有如下变量：
```cpp
int a;
float b;
double c;
```

假设起始地址是100，那么机器并不知道100-203是整型或者104-107是float型变量b。当有(float)a时，则是先按照int类型取出该变量在地址中的值，然后再将其按照int to float的规则转换成float型，编译器在取出int a对应的值时知道规则是从a的起始地址开始往后取4个字节的值。

对于指针之间的转换，指针 to 指针的强制类型转换是指针所指内容的类型由原来的类型转换为后面的类型。
```cpp
int a =1;
int *p=&a;
float *p1=(float *)p;
```

则p和p1的值都是&a，但是`*p`是将&a地址中的值按照int型变量进行解释，而`*p1`则是将&a地址中的值按照float型变量进行解释。

例：指针类型强制转换
```cpp
int m;
int *pm = &m;
char *cp = (char *)&m;
```
pm指向一个整型，cp指向整型数的第一个字节

回到文章一开始的题目：

题目是将一个int类型数的地址强制转换成char类型指针，然后输出char类型指针指向的值。

从以上的分析可知，转换后的char类型指针指向int类型的第一个字节（实际上是低地址位的一个字节）。

如此，可以计算：6336的二进制数是：`1 1000 1100 0000`，char类型指针指向低地址位的一个字节，

那么char类型的指针指向的是：`1100 0000`，

而我们知道，计算机中正数是二进制的补码表示，所以将其转换成补码（负数的补码是在原码的基础上除符号位不变之外按位取反然后再加1）就是：`1100 0000`，即为-64。

## 参考文章 ##

1. C语言指针强制类型转换，[mhjcumt的专栏].[http://blog.csdn.net/mhjcumt/article/details/7355127](http://blog.csdn.net/mhjcumt/article/details/7355127)
2. C语言指针强制类型转换,[jltxgcy的专栏].[http://blog.csdn.net/jltxgcy/article/details/8766537](http://blog.csdn.net/jltxgcy/article/details/8766537)
3. 指针类型的强制转换问题,[知乎].[https://www.zhihu.com/question/24174125](https://www.zhihu.com/question/24174125)





