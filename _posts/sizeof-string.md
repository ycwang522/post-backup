---
title: 关于sizeof（string）的问题
date: 2016-06-30 10:33:13
categories: C++
tags:
	- sizeof
	- string
	- 位域
---



# 关于sizeof(string)
偶然间测试sizeof(string)的时候，在32位+win10+vs2013机子上测试如下一段代码：
```cpp
string str1 = "Hello world";
string str2[] = { "a","b","c" };

cout<<sizeof(str1)<<" "<<sizeof(str2)<<" "<<sizeof(string)<<endl;

```
得到结果分别为：28，84,28

查阅资料时发现，输出结果和不同的编译器有关。

string的实现在各类库中可能有所不同，但是在同一库中相同一点是，无论你的string里放多长的字符串，sizeof()都是固定的，字符串所占空间是从堆中动态分配的，与sizeof()无关。

<!-- more -->

# 关于sizeof的一些小细节 #

sizeof并不是一个函数，或者可以说是一元操作符，甚至像是一个特殊的宏，因为是在编译阶段求值的。
```cpp
cout<<sizeof(int)<<endl; // 32位机上int长度为4

cout<<sizeof(1==2)<<endl; // == 操作符返回bool类型，相当于 cout<<sizeof(bool)<<endl;

    在编译阶段已经被翻译为：

cout<<4<<endl;

cout<<1<<endl;

    这里有个陷阱，看下面的程序：

int a = 0;

cout<<sizeof(a=3)<<endl;

cout<<a<<endl;
```

输出为什么是4，0而不是期望中的4，3？？？<font color="green">就在于sizeof在编译阶段处理的特性。由于sizeof不能被编译成机器码，所以sizeof作用范围内，也就是()里面的内容也不能被编译，而是被替换成类型。=操作符返回左操作数的类型，所以a=3相当于int，而代码也被替换为</font>：

```cpp
int a = 0;

cout<<4<<endl;

cout<<a<<endl;
```

所以，sizeof是不可能支持链式表达式的，这也是和一元操作符不一样的地方。

> 结论：不要把sizeof当成函数，也不要看作一元操作符，把他当成一个特殊的编译预处理。

## sizeof求函数的字节

例如

```cpp
int f1(){return 0;}
double f2(){return 0;}
void f3(){return 0;}
cout<<sizeof(f1())<<endl; // f1()返回值为int，因此被认为是int
cout<<sizeof(f2())<<endl; // f2()返回值为double，因此被认为是double
cout<<sizeof(f3())<<endl; // 错误！无法对void类型使用sizeof
cout<<sizeof(f1)<<endl;   // 错误！无法对函数指针使用sizeof   
```
 结论：**对函数使用sizeof，在编译阶段会被函数返回值的类型取代，**


## 位域

　在结构体和类中，可以使用位域来规定某个成员所能占用的空间，所以使用位域能在一定程度上节省结构体占用的空间。

考虑以下代码
```cpp
struct s1
{
   int i: 8;
   int j: 4;
   double b;
   int a:3;
};
struct s2
{
   int i;
   int j;
   double b;
   int a;
};
struct s3
{
   int i;
   int j;
   int a;
   double b;
};
struct s4
{
   int i: 8;
   int j: 4;
   int a:3;
   double b;
};
cout<<sizeof(s1)<<endl;   // 24
cout<<sizeof(s2)<<endl;   // 24
cout<<sizeof(s3)<<endl;   // 24
cout<<sizeof(s4)<<endl;   // 16
```

其中
s1中，i和j总共占用12位，不足8字节，按照8字节对其，然后再加上double的8个字节，最后一个a占3位，也不足8字节，按照8字节对其，总共就是24字节。
在s4中，i、j、a总共占用15位，不足8字节，按照double 8字节对齐，再加上double的8个字节，总共占用16个字节。