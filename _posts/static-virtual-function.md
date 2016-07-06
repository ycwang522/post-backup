---
title: 为什么虚函数(virtual)不能是static函数
date: 2016-06-08 15:22:30
categories: C++
tags:
	- C++
	- C
---



简而言之，**成员函数实例相关，静态函数类相关。**

虚函数，是一种特殊的成员函数，用来实现运行时多态。

- 静态成员函数，可以不通过对象来调用，没有隐藏的this指针。
- virtual函数一定要通过对象来调用，有隐藏的this指针。

<font color="red">所以，关键问题是static成员没有this指针。</font>

static function 是静态决议（编译的时候就绑定了）

而virtual function 是动态决议的（运行时才绑定）

引用stackoverflow网友@Kerrek SB 的回答：

> That would make no sense. The point of virtual member functions is that they are dispatched based on the dynamic type of the object instance on which they are called. On the other hand, static functions are not related to any instances and are rather a property of the class. Thus it makes no sense for them to be virtual. If you must, you can use a non-static dispatcher.

即是说：virtual成员函数的关键是动态类型绑定的实例调用。然而，静态函数和任何类的实例都不相关，它是class的属性。

<!-- more -->