---
title: memcpy, memset,memmove区别分析
date: 2016-08-28 21:29:53
categories: C++
tags:
	- C++
---

# 1. memcpy

C和C++使用的内存拷贝函数，mencpy函数的功能是从源src所指的**内存地址的起始位置**开始拷贝**n个字节**到目标**dest所指的内存地址的起始位置**中。

头文件：
```cpp
#include<string.h>
#include<cstring>
```

返回值
返回指向dest的指针
```cpp
#include <stdio.h>
#include <string.h>
int main()
{
	char* s = "GoldenGlobalView";
	char d[20];
	//clrscr();
	memcpy(d, s, (strlen(s) + 1));
	printf("%s", d);
	getchar();
	return 0;
}
```
输出结果：GoldenGlobalView
<!-- more -->

```cpp
#include<stdio.h>
#include<string.h>
int main()
{
	char* s = "GoldenGlobalView";
	char d[20];
	memcpy(d, s + 12, 4);//从第13个字符(V)开始复制，连续复制4个字符(View)
	d[4] = '\0';//memcpy(d,s+12*sizeof(char),4*sizeof(char));也可
	printf("%s", d);
	getchar();
	return 0;
}
```
输出：View
作用：将s中第13个字符开始的4个连续字符复制到d中。(从0开始)

## 1.1 strcpy与memcpy的区别

- 复制的内容不同。strcpy只能复制字符串，而memcpy可以复制任意内容，例如字符数组、整型、结构体等。
- 复制的方法不同。strcpy不需要指定长度，它遇到被复制字符串的结束符`'\0'`才结束，所以容易溢出。memcpy则是根据其第3个参数决定复制的长度
- 用途不同。strcpy一般复制字符串，其他类型一般用memcpy

# 2. memset

函数原型：`void *memset(void *s,int ch,size_t n);`

将s中当前位置后面的n个字节用ch替换并返回s

或者是

将s所指向的某一块内存中的前n个字节的内容全部设置为ch指定的ASCⅡ值，第一个值为指定的内存地址，块的大小由第三个参数决定。

该函数通常为新申请的内存做初始化工作，其返回值为指向s的指针。
```cpp
#include <string.h>
#include <stdio.h>
#include <memory.h>
 
int main(void)
{
    char buffer[]="Helloworld\n";
    printf("Buffer before memset:%s\n",buffer);
    memset(buffer,'*',strlen(buffer));
    printf("Buffer after memset:%s\n",buffer);
    return 0;
}
```

	输出结果：
	Buffer before memset:Helloworld
	 
	Buffer after memset:***********


# 3. memmove

memmove用于从src拷贝count个**字节**到dest，<font color="red">如果目标区域和源区域有重叠的话，memmove能够保证源串在被覆盖之前将重叠区域的字节拷贝到目标区域中。但复制后src内容会被更改</font>。但是当目标区域与源区域没有重叠则和memcpy函数功能相同。

# 4. memccpy

表头文件: `#include <string.h>`
定义函数:` void *memccpy(void *dest, const void *src, int c, size_t n);`
函数说明: memccpy()用来拷贝src所指的内存内容前n个字节到dest所指的地址上。与memcpy()不同的是,memccpy()如果在src中遇到某个特定值(int c)立即停止复制。
返回值:   返回指向dest中值为c的下一个字节指针。返回值为0表示在src所指内存前n个字节中没有值为c的字节。

