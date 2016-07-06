---
title: static详解
date: 2016-05-30 15:40:42
categories: C++
tags:
	- C++
	- C
---

# static详解

对于static关键字的作用，一般的回答为如下形式

- 1.限制变量的作用域
- 2.设置变量的存储域

static的特点是：**static局部变量的“记忆性”与生存期的“全局性”**

## 记忆性

是指在两次函数调用时，在第二次调用进入时，能保持第一次调用退出时的值。

<font color="blue">static局部变量在运行期仅初始化一次，下次再调用时，不再进行初始化，在上一次调用的基础上进行操作</font>。


<!-- more -->

例如：
```cpp

#include <iostream>

using namespace std;

void staticLocalVar()
{
 static int a = 0; // 运行期时初始化一次, 下次再调用时, 不进行初始化工作
 cout<<"a="<<a<<endl;
 ++a;
}

int main()
{
 staticLocalVar(); // 第一次调用, 输出a=0
 staticLocalVar(); // 第二次调用, 记忆了第一次退出时的值, 输出a=1
 return 0;
}
```
示例程序源于[static用法小结](http://blog.csdn.net/xiaocai0001/article/details/662921)

### 记忆性的应用

利用static局部变量的记忆性可以记录函数调用的次数。

## 全局性

利用static变量生存期的“全局性”，可以改善“return a point/reference to a local object”的问题。

Local object的问题在于退出函数，生存期结束。
利用static的作用，可以延长变量的生存期。

例如
```cpp
// IP address to string format
// Used in Ethernet Frame and IP Header analysis
const char * IpToStr(UINT32 IpAddr)
{
 static char strBuff[16]; // static局部变量, 用于返回地址有效
 const unsigned char *pChIP = (const unsigned char *)&IpAddr;
 sprintf(strBuff, "%u.%u.%u.%u",  pChIP[0], pChIP[1], pChIP[2], pChIP[3]);
 return strBuff;
}
```

## 静态函数

函数前的static不是指储存方式，而是指对函数的作用域仅限于本文件，所以静态函数又称内部函数。

对于全局变量，无论是否有static关键字，其都是存储在静态存储区，生存周期都是全局的。

使用内部函数（静态函数）的好处是：不同的人编写不同的函数，不用担心自己定义的函数，是否会与其他文件中的函数同名。

## 静态数据成员（C++独有）

表示属于一个类而不是属于此类的任何特性的对象的变量和函数。

这儿static是指示变量/函数在此类中的唯一性。

static类型声明符在C语言中主要有三个方面的用途：

- 1.声明静态局部变量
- 2.声明静态外部全局变量
- 3.声明静态外部函数

静态变量的一个例子：
```cpp

static int a=1;
void fun1(void)
{
    a=2;
}
void fun2(void)
{
    int a=3;
}
void fun3(void)
{
    static int a=4;
}
int main()
{
  printf(“%d”,a);
  fun1( );
  printf(“%d”,a);
  fun2( );
  printf(“%d”，a）；   
  fun3( )
  printf(“%d”,a); 
}
```

程序输出的结果是：1222

分析：








	首先定义一个全局变量a。	
	fun1中，并没有定义a，所以fun1中的a是函数外部全局变量，fun1中a被重新赋值为2.	
	fun2中和fun3中都重新定义了a，	
	而fun2中的a是一局部非静态变量，
	当函数调用结束后该局部变量被直接销毁。
	fun3中的a为静态变量（只分配一次内存），	
	虽然函数调用完成之后a不会销毁，	
	但只是在函数内部才会生效，	
	所以main函数打印的是全局变量a。


	