---
title: 一道结合了小端模式+栈+printf+数据类型转换的题目
date: 2016-06-13 11:39:09
categories: C++
tags:
	- C
	- C++
	- 小端模式
---



题目如下：

> 假设在一个 32 位 little endian 的机器上运行下面的程序，结果是多少？

```cpp
#include <stdio.h>
int main(){
  long long a = 1, b = 2, c = 3; 
  printf("%d %d %d\n", a, b, c);  
 return 0;
}

```  
正确答案：1 0 2

<!-- more -->

解析：
感谢牛客网网友@千江乐 提供解决思路

1. printf()是一个库函数，C以及C++中函数的参数是从右往左入栈的；
2. 和堆的生长方向相反，栈的生长反响是从高往低的。
3. 小端模式是低位存储低字节
4. %d格式输出的是4个字节大小，而long long 为8个字节。


![](https://ituku.tk/di/UP9UO/423501-1444290678513-aced241801e307ee7a39612f85a94ebf.png) 




