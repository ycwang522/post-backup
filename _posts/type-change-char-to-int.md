---
title: C中常见的类型转换
date: 2016-06-13 11:35:17
categories: C++
tags:
	- C
	- C++
	- 类型转换
---

# 常见的类型转换<char 与 int 之间的转换>

有这么一道题目：

    32位的机器下，下面说法哪些是正确的？
    
    signed char a = 0xe0;
    unsigned int b = a;
    unsigned char c = a;
    
    A. a>0 && c>0
    B. a==c
    C. b的十六进制的表示是：0xffffffe0   
    D. 以上说法都是错误的

解析：

> A错，signed char比较时候类型要提升成int，符号位是补最高位的1，所以a应该是负数
> 
> B错：当进行==比较时，char类型的a和c都要要先提升成int类型才进行比较，a转换成int后是0xffffffe0,它是一负数，而c转化成int之后是0x000000e0,是一正数，很明显不相等。
> 
> C对：signed char先转化成int类型再转化成unsigned int类型，signed char转化成int时候要进行符号扩展，因为是有符号类型，所以最高位就是补1.

<!-- more -->

1. char转换为int型，即signed char转换为signed int型：

char类型占一个字节内存大小，int类型占用4个字节，当char转换成int型时，在int类型变量高位前3个字节填充，填充位的二进制位为**char类型最高位的比特**，例如：

`char a = 0x11;` //0001 0001(占用1个字节，8位)

当发生如下转换时：

`int b = a;` //因为char类型最高位的比特是0，所以char类型的a转换成int类型的b时，前3个字节需要填充0.

即转换后b的十六进制为：`0x00000011`。

同理，如果有如下的char类型a：

`char a = 0x81;` //1000 0001

当发生如下的类型转换时，

`int b = a;`//此时，char类型的最高位比特数位1，所以转换后的int前3个字节均填充1,

转换后的b的十六进制数为 `0xffffff81`

2. unsigned char转为int的情况

例如：
unsigned char a = 0xf1; //或者是 a = 0x11 (最高位是0还是1的差别)

发生如下的类型转换时：

int b = a;

无符号数转换时，无论char类型的最高位是0还是1，转换为int类型时，前3个字节一律填充0.

## 小结：

signed char转换成int类型时，转换后的int类型前3个字节填充对应char类型最高位的比特数。

而unsigned char类型转换成int类型时，无论char类型的最高位是0还是1,在转换后的int类型前3个字节中均填充0.


3. long转换为int类型时，由于都是4个字节，所以无需填充。

4. int类型转换为long long 类型

同理，signed int转换为long long时，需要在高位填充，填充符号位。

而unsigned int转换为long long 时，全部填充0.

-感谢博客园@cyonks 搜集整理。

## 参考文章

- [笔试常见之C类型转换](http://www.cnblogs.com/xrong/archive/2013/04/14/3020240.html)
