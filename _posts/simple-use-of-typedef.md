---
title: typedef用法小结
date: 2016-06-07 10:33:27
categories: C++
tags:
	- C++
	- C
	- C++小结
---

# typedef使用小结

stackoverflow上看到的一个问题。

    typedef void(*FunctionFunc)();

# Three Question:

1.Why is `typedef` used?
2.The syntax looks odd;after `void` should there not be a function name or something? It looks like an anonymous function.
3.Is a function pointer created to store the memory address of a function ?

三个问题概括一下就是：

- 使用typedef有什么好处
- 语法和常见的语法不同
- 创建一个函数指针是不是来存储函数的内存地址的？
<!-- more -->

# Answer1：

typedef 是一个联系变量类型和变量名称的构造语言

你可以以类型初始化的方式使用它，比如：

    typedef int myinteger;
    typedef char *mystring;
    typedef void (*myfunc)();

可以使用以下的方式对上面的定义加以使用：

    myinteger i;	// is equivalent to		int i;
    mystring s;		// is the same as		char *s;
    myfunc f;		//compile equally as 	void(*f)();     

如上所示，可以使用如上所示的定义来替换typedef定义的名称。

主要的难点在于C和C++中函数指针语法的可读性，使用`typedef`可以提高这类声明的可读性。
然而，虽然在语法上是行得通的，但是函数不像其他简单的类型，函数可能会有返回值和参数，这样有时候会有复杂的函数指针。
对于以上的三个问题，回答如下：

- Why is typedef used?

为了简化代码的可读性-尤其是函数指针或者是结构体名称

- The syntax looks odd--

使得代码的可读性更高。

- Is a function pointer created to ...

是的，函数指针存储函数的地址。
然而，当typedef用于结构体中时，仅仅是为了提高程序的可读性，程序在编译时编译器会在typedef使用的地方使用实际代码替换。

Example：

    typedef int (*t_somefunc)(int,int);
    
    int product(int u, int v) {
      return u*v;
    }
    
    t_somefunc afunc = &product;
    ...
    int x2 = (*afunc)(123, 456); // call product() to calculate 123*456



  


