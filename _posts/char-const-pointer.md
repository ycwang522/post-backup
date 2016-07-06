---
title: 企图修改常量区的字符串所引发的错误
date: 2016-06-25 17:04:14
categories: C++
tags:
	- C++
	- 字符串常量
---
经常会采用char类型的指针来存储字符串，即为将字符串的首地址值赋给基类型为char的指针变量，从而可以从字符串首元素开始对字符串进行操作，其中有一些常见的问题。

有如下例子：
```cpp
int main()
{
	char *p = "hello world";
	p[0] = 'H';
	printf("%s\n",p);
	return 0;
}
```

运行该程序会出现错误，因为`*p="hello world"`这句仅仅**声明了一个指针变量**，<font color="green">指向字符串"hello world",而"hello world"这个字符串程序没有给它分配空间，编译器将它分配到常量区。而常量区的字符串的值时不允许修改的。</font>

<!-- more -->

修改如下：
```cpp
int main()
{
	char p[12]="hello world";
	char *p1=p;
	p1[0]='H';
	printf("%s\n",p1);
	return 0;
}
```
原因在于`p[12] = "hello world"`是自己定义的一个长度为12的字符数组，编译器会为他分配栈空间，所以能够修改它的值。

