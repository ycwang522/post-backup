---
title: C++中的强制类型转换
date: 2016-06-14 22:14:16
categories: C++
tags:
	- C
	- C++
	- 类型转换
---

本节介绍四个强制类型转换运算符：`const_cast`、`reinterpret_cast`、`static_cast`和`dynamic_cast`。

## 1. const_cast转换符

使用表达式`const_cast<T>(v)`可以更改指针或者引用的`const或者volatile限定符`。T必须是指针、引用或者指向成员的指针类型。

```cpp
class A
{
public:
	virtual void f();
	int i;
};

extern const volatile int *cvip;
extern int *ip;

void use_of_const_cast()
{
	const A a1;
	const_cast<A&>(a1).f();		//remove const 
	ip=const_cast<int *>(cvip);	//remove const and volatile
}
```
<!-- more -->

## 2. reinterpret_cast

例如pointer和int 的无关类型的转换。
reinterpret_cast<T>(v)用于更改表达式v值的解释。
1. 转换的类型必须是一个指针、引用、算术类型、函数指针或者成员指针。
2. 在比特位级上进行转换。它可以把一个指针转换成一个整数，反过来亦可。
3. 最普通的用法就是函数指针类型之间进行转换。
4. 很难保证移植性。此强制转换运算符不是通用的，因此，它不能保证可移植到其它编译器。

```cpp
const char* str = "hello";
int i = static_cast<int>(str);//error C2440: 'static_cast' : cannot
                              // convert from 'const char *' to 'int'
int j = (int)str; // C-style cast. Did the programmer really intend
                  // to do this?
int k = reinterpret_cast<int>(str);// Programming intent is clear.
                                   // However, it is not 64-bit safe.
```

## 3. static_cast转换

用法：
	static_cast <type-id> ( expression )

`static_cast`可用于将指向基类的指针转换为指向派生类的指针等操作，该类型转换并非始终安全。

- 基类和父类之间的转换。子类指针转换成父类指针时安全的，但是父类指针指向子类指针时不安全的。（基类和子类的动态类型转换建议使用dynamic_cast）
- 基本类型转换。其中static_cast不能进行无关类型(如非基类和子类)指针之间的转换。
- 将空指针转换成目标类型的空指针
- 任何类型的表达式转换成void类型。
- static_cast不能去掉类型的const，volitale属性。 

```cpp
int n =6;
double d = static_cast<double>(n); // 基本类型转换
int*pn =&n;
double*d = static_cast<double*>(&n) //无关类型指针转换，编译错误
void*p = static_cast<void*>(pn); //任意类型转换成void类型

```

## 4. dynamic_cast

有条件转换，动态类型转换，运行时类型安全检查。（转换失败返回NULL）
- 安全的基类和子类的转换
- 基类必须要有虚函数：保持多态性才能使用dynamic_cast转换
- 相同基类不同子类之间的交叉转换。但结果是NULL。

# 参考文章

- [博客园-C++类型转换总结](http://www.cnblogs.com/goodhacker/archive/2011/07/20/2111996.html)
- [ORACLE-强制类型转换](https://docs.oracle.com/cd/E19205-01/820-1214/bkahi/index.html)
- [MSDN-强制转换运算符](https://msdn.microsoft.com/zh-cn/library/c36yw7x9.aspx?sentenceGuid=382b448e98672018cf59a6b9a3847391#mt7)