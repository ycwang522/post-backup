---
title: 虚函数表详解
date: 2016-06-21 14:44:48
categories: C++
tags:
	- 虚函数
	- C++
	- virtual function table
---

假如有如下类：

```cpp
class Base
{
public:
	virtual void f()
	{
		cout << "Base::f()" << endl;
	}
	virtual void g()
	{
		cout << "Base::g()" << endl;
	}
	virtual void h()
	{
		cout << "Base::h()" << endl;
	}
};

```
通过Base的实例得到虚表。
<!-- more -->

```cpp
typedef void(*Fun)(void);

Base b;

Fun pFun = NULL;

cout << "虚函数表地址：" << (int *)(&b) << endl;
cout << "虚函数表-第一个函数地址：" << (int *)*(int *)(&b) << endl;

pFun = (Fun)*((int*)*(int*)(&b));
pFun();

```

输出：

	虚函数表地址：0014FD10
	虚函数表-第一个函数地址：0105DA58
	Base::f()

解析：强行将&b转换成`int *`,取得`虚函数表的地址`，然后，对其再次取地址就可以得到第一个虚函数的地址了，也就是Base::f().
同理，如果要调用剩下的两个函数，代码为：

    (Fun)*((int*)*(int*)(&b)+1);  // Base::g()
    (Fun)*((int*)*(int*)(&b)+2);  // Base::h()

虚表的对象模型如下表所示：
![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_vtable1.jpg)

## 一般继承的对象模型（无虚函数覆盖）

![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_Drawing3.jpg)

该继承关系中，子类没有重载父类的任何一个函数。则派生类实例中，虚函数的表如下所示：

对于实例：Derived d的虚函数表如下所示：

![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_vtable2.JPG)

有两点需要注意
- 虚函数按照其声明顺序放于表中。
- 父类的虚函数在子类的虚函数前面。

## 有虚函数覆盖的一般继承

有如下的继承关系：
![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_Drawing4.jpg)

如上的继承关系中，只有一个虚函数f，派生类的实例中对象模型如下图所示：
![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_vtable3.JPG)

同样，有两点需要注意：
- 覆盖的f()函数被放到了虚表中原来父类虚函数的位置。
- 没有被覆盖的函数依旧。

这样，对于如下代码：

    Base *b = new Derive();
     
    b->f();
 
由b所指的内存中的虚函数表的f()的位置已经被Derive::f()函数地址所取代，于是在实际调用发生时，是Derive::f()被调用了。这就实现了多态。

## 多重继承（无虚函数覆盖）

![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_Drawing1.jpg)

有如上图所示的继承关系，子类实例中的虚函数表，对象模型如下图所示：

![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_vtable4.JPG)

两点注意
- 每个父类都有自己的虚表
- 子类的成员函数被放到了第一个父类的表中（第一个父类是按照声明顺序来说得）

## 多重继承（有虚函数覆盖）

![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_Drawing2.jpg)

子类实例中的虚函数表：

![](http://p.blog.csdn.net/images/p_blog_csdn_net/haoel/15190/o_vtable5.jpg)

三个父类虚函数表中的f()的位置被替换成了子类的函数指针。这样，我们就可以任一静态类型的父类来指向子类，并调用子类的f()了。

## 参考文章

- [陈皓-C++ 虚函数表解析](http://blog.csdn.net/haoel/article/details/1948051)
