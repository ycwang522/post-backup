---
title: C++面试宝典备忘录
date: 2016-05-30 10:33:19
categories: C++面试
tags:
	- C++
	- 面试
	- 编程语言
---

## 1. 自增自减前操作与后操作的区别

> 前自增自减操作的优先级大于赋值运算符（=），而后自增自减操作的优先级小于赋值运算符，后自增自减操作后表达式的值不会发生改变。

## 2. 指针自增自减的问题分析

> 指针的自增和自减是将指针的所指地址加1或者减1的操作，分前置和后置。
> 例如：p是一个指针变量，++p将p加1，++*p将*p所指单元加1，*p++只将p加1，++*p++将p加1，同时也将*p所指单元加1.

<!-- more -->

## 3. 左值和右值的区别

> 左值可以出现在赋值语句的左边或右边，而右值只能出现在赋值的右边，而不能出现在左边。
> 实际上，左值是一个储存地址，右值是一个具体的数据或者数值。

## 4. C++中的作用域

分为 `全局作用域` 和 `局部作用域(语句作用域)`，定义在所有函数外部的变量具有全局作用域， 全局变量可以在程序中的任何地方访问。

例如

```cpp
#include<isotream>
void main()
{
	int a=0,b=0;//定义全局变量a、b，并初始化
	a++;
	b++;
	cout<<a<<endl;//a=1
	cout<<b<<endl;//b=1;
	{
		float a=0.1;//定义局部变量a
		a++;
		b++;
		cout<<a<<endl;//局部变量a=1.1
		cout<<b<<endl;//全局变量b=2
	}
	a++;
	b++;
	cout<<a<<endl;//a=2
	cout<<b<<endl;//b=3
	cin>>a;
	//不同作用域的变量可以使用相同的变量名，全局变量可以在程序中的任何地方访问。
}

```
## 5. 引用
> 引用就是某一变量（目标）的一个别名，对引用的操作与对变量直接操作完全一样。
引用的声明方法：类型标识符 &引用名=目标变量名；
有关引用详细的说明参考：[C++中引用（&）的用法和应用实例](http://www.cnblogs.com/Mr-xu/archive/2012/08/07/2626973.html%20Mr.xu%E7%9A%84%E5%8D%9A%E5%AE%A2)

## 6.递归
> 递归算法解题的效率相对较低，在递归调用的过程中系统为每一层的返回值，局部量等开辟了栈了储存。
> 使用递归的时候需要消耗较多的栈空间，所以递归过程次数较多时容易造成栈的溢出。
> 在栈的尺寸大小受到限制时，一般避免使用递归。
递归经典问题：汉诺塔问题
```cpp
#include <stdio.h>
//第一个塔为初始塔，中间的塔为借用塔，最后一个塔为目标塔
int i=1;//记录步数
void move(int n,char from,char to) //将编号为n的盘子由from移动到to
{printf("第%d步:将%d号盘子%c---->%c\n",i++,n,from,to);
}
void hanoi(int n,char from,char denpend_on,char to)//将n个盘子由初始塔移动到目标塔(利用借用塔)
{
    if (n==1)
    move(1,from,to);//只有一个盘子是直接将初塔上的盘子移动到目的地
    else
    {
      hanoi(n-1,from,to,denpend_on);//先将初始塔的前n-1个盘子借助目的塔移动到借用塔上
      move(n,from,to);              //将剩下的一个盘子移动到目的塔上
      hanoi(n-1,denpend_on,from,to);//最后将借用塔上的n-1个盘子移动到目的塔上
    }
}
void main()
{
     printf("请输入盘子的个数:\n");
     int n;
     scanf("%d",&n);
     char x='A',y='B',z='C';
     printf("盘子移动情况如下:\n");
     hanoi(n,x,y,z);
}

```
## 7. 异常与错误
> 异常就是程序运行时出现的不正常，它可能会导致系统无法正常运行甚至停止运行等严重的情况，编程者需要实现好的异常处理来保证程序的稳定性。
> C++中，异常机制可以提供程序中错误检测与错误处理部分之间的通信。
> 详细介绍参考[C++异常以及错误处理](http://blog.csdn.net/wangfengwf/article/details/11580817)
> C++中，系统通过try块和异常处理构成了异常机制；其中通过catch语句来捕捉运行时的异常，并且执行异常处理，通过throw语句可以抛出异常。

## 8. 包含头文件时如何查找头文件
> `#include`的使用方式有两种：```#include<>和#include""```，其中以尖括号包含的方式叫做标准头文件，以双引号包含的方式叫做非系统头文件，也就是用户自定义的头文件。
> 系统标准头文件用尖括号括起来，这样编译器会在系统文件目录下查找。
> 用户自定义的文件用双引号括起来，编译器首先会在用户目录下查找，然后再到C++安装目录中查找，最后在系统文件中查找。


## 9. 内存分配(new)和释放(delete)
> new的使用方式：```指针变量=new 数据类型```
> 实例代码：
> `int *p；p=new int;`p直接指向一段由new分配而来的新内存空间。
> 操作时，通过对指针p的操作对p所指向的内存空间进行操作：`int *p=new int; *p=100; cout<<*P<<endl;`
> delete的使用：使用方式为：`delete 指针变量`

```cpp
示例代码：
int *p;
p=new int;
*p=100;
cout<<*p<<endl;
delete p;//释放内存空间
system("pause");
```
## 10. 数组指针域指针数组

> 数组指针：指向一个数组的指针就是数组指针。例如`int (*ap) [2];//该代码定义了一个指向包含两个元素的数组指针`
> 指针数组：如果一个数组的每一个元素都是指针，则这个数组是一个指针数组。例如：`char *chararr[]={"Fortran","C","C++","Basic"}`

## 11. 引用与值传递
> 值传递是指将要传递的值作为一个副本传递。值传递过程中，被调用函数的形参作为被调函数的局部变量处理。值传递的特点是被调函数对形参的任何操作都是作为局部变量进行，不会更改主调函数的实参变量的值。
例如，有如下代码：

```
void func1(int x)//参数为值传递的函数
{
	x=x+10;
}
.
.
.
int n=0;
func1(n);//值传递
cout<<"n="<<n<<endl;

运行结果为：n=0
```
> 结果分析：由于func1函数体内的x采用的是值传递的方式，它只是外部变量n的一个副本，改变x的值不会影响n的值，所以n的值仍然为0.

> 引用传递传递的是引用对象的内存地址。在地址传递过程中，被调函数的形参也作为局部变量在堆栈中开辟了内存空间。

引用传递的示例代码：

```cpp
void func2(int &x)
{
	x=x+10;
}
...
int n=0;
func2(n);
cout<<"n="<<n<<endl;

执行结果为：n=10

//结果分析：由于func2（）函数体内的x采用的是引用传递的方式，x是外部变量n的引用，x和n表示的是相同的对象，所以当x发生改变时n的值也相应改变。
```
#### 总结
值传递传递的是一个值得副本。函数对形参的操作不会影响实参的值，而引用传递触底的引用对象的内存地址，函数对形参的操作会影响实参的值。

## 12. 成员变量的访问方式
> 三种访问方式分别为：private,protected,public三种
> private：只能被该类的方法(成员函数)访问，是私有变量，不能被该类的对象访问。
> protected：可以被该类的方法和其友元函数访问，但不能被该类的对象访问。
> public：可以被该类中的方法和其友元函数访问，是公有变量，也可以被该类的对象访问。

> 在C语言中，如果没有申明访问权限，默认的访问权限是public，在C++中，默认的访问权限是private。所以在类中的成员中如果需要外部调用都需要加上关键词为公有类型（public）。

## 13. 多态
> 是面向对象的重要特性之一，它是一种行为的封装，即“一个接口，多种实现”，也就是同一种事物表现出的多种形态。
> 多态的主要作用表现为以下两点：
> 1：可以不必为每一个派生类编写功能调用，只需要对抽象基类进行处理即可。
> 2：派生类的功能可以被基类的方法或引用变量所调用，称作向后兼容。

#### 纯虚函数
> 纯虚函数是在基类中声明的虚函数，它在<font color=red>基类中没有定义，但要求任何派生类都要定义自己的实现方法</font>。在基类中实现纯虚函数的方法是在函数原型后加“=0”： `virtual void funtion()=0`
> 引入原因：在很多情况下，基类本身生成对象是不合情理的，因此引入虚函数概念，则编译器要求在派生类中必须予以重写以实现多态性。<font color=red>同时含有纯虚拟函数的类称为抽象类，它不能生成对象</font>。
> 多态性：编译时多态性 - 重载函数实现；运行时多态性 - 虚函数实现。
   
## 14. 继承时访问级别的变化
> C++中，继承时可以降低父类的访问级别的
> public（公用继承）：基类成员保持自己的访问级别。
> protected（受保护继承）：基类的public和protected成员在派生类中为protected成员。基类的private成员保持为private。
> private（私有继承）：基类的所有成员在派生类中为private成员。

### 小插曲
#### 覆盖、重载和多态的区别
原文转自博客园_[C++覆盖、重载、多态区别](http://www.cnblogs.com/Yogurshine/archive/2013/01/12/2857738.html#commentform)
 
 覆盖：
在基类中定义了一个<font color=red>非虚拟函数</font>，然后在派生类中又定义了一个同名同参数同返回类型的函数，这就是覆盖了。
在派生类对象上直接调用这个函数名，只会调用派生类中的那个。
例如：

```cpp
//coverage.cpp

 

#include <iostream>
using namespace std;
 
class A
{
public:
  void ShowMessage();
};

class B:public A
{
public:
  void ShowMessage();
};

void A::ShowMessage()
{
  cout<<"Hello,This is A./n";
  return;
}

void B::ShowMessage()
{
  cout<<"Hello,This is B./n";
  return;
}

int main()
{
  A* p;
  B memb;
  *p = memb;
  p->ShowMessage();
  memb.ShowMessage();
  return 0;
}
//输出为：
//Hello,This is A.
//Hello,This is B.
```
重载：
在同一个类中（此处与原文略有不同），有两个或多个函数名相同的函数，但是函数的<font color=red>形参列表不同</font>，virtual关键字可有可无。在调用相同函数名的函数时，根据<font color=green>形参列表确定到底该调用哪一个函数</font>。
例如：

```cpp
//reload
#include <iostream>
using namespace std;

class A
{
public:
  void ShowMessage();
  void ShowMessage(string str);
};

void A::ShowMessage()
{
cout<<"Hi,This is A./n";
  return;
}

void A::ShowMessage(string str)
{
  cout<<str<<endl;
  return;
}

int main()
{
  A mem;
  mem.ShowMessage();
  mem.ShowMessage("Hello.How are you?/n");
  return 0;
}

//输出为：
//Hi,This is A.
//Hello.How are you?
```

多态：
在基类中定义了一个<font color=red>虚拟函数</font>，然后在<font color=red>派生类中又定义一个同名，同参数表的函数</font>，这就是多态。多态是这3种情况中唯一采用<font color=yellow>动态绑定技术</font>的一种情况。也就是说，通过一个基类指针来操作对象，如果对象是基类对象，就会调用基类中的那个函数，如果对象实际是派生类对象，就会调用派生类中的那个函数，调用哪个函数并不由函数的参数表决定，而是由函数的实际类型决定。

```cpp
//poly.cpp

#include <iostream>
using namespace std;

class A
{
public:
   virtual void ShowMessage();//声明基类的虚函数
};

class B:public A
{
public:
  void ShowMessage();
};

void A::ShowMessage()
{
  cout<<"This is A./n";
  return;
}

void B::ShowMessage()
{
  cout<<"This is B./n";
  return;
}

int main()
{
  A* p;
  p=new A();
  p->ShowMessage();
  p=new B();
  p->ShowMessage();
  return 0;
}

//输出为：
//This is A.
//This is B.
```

## 15. 内联函数

> 在类声明的内部声明或定义的成员函数叫做内联（inline）函数。
> 特点： 一般来说，内联机制适用于优化小的，只有几行的而且经常被调用的函数。
> 分类：
> （1）在类声明的内部声明，而在类声明外部定义的叫做显式内联函数。
> （2）在类声明的内部声明定义，叫做隐式内联函数。
> 内联函数的执行机制：当内联函数在调用函数处用内联函数体的代码来替换。
> 局限性：函数中的执行代码不宜过多。

```cpp
class TableClass
{
private:
	int a,b;
public:	
	int add()//定义内联函数，不使用inline关键字
	{
		return a+b;
	};
	
	inline int dec()//定义内联函数，使用inline关键字
	{
		return a-b;
	}

	int GetNum();
}

inline int tableclass::GetNum()
{
	return a;
}
//在上面的代码中，类内部的函数无论是否有inline关键词，都被默认为内联函数。而外部的函数使用inline关键字定义为了类的内联函数。
```
## 16. 静态函数
> 静态函数就是用static关键字修饰的函数，静态成员函数的声明除了在类体的函数生命前加上关键字static，以及不能声明为const之外，与非静态成员函数相同。
> **静态函数没有this指针

## 17. 模板函数

```cpp
template<class T>//定义模板标识
T add(T a,T b)
{
	T result=a+b;
	return result;
}

```
以下代码演示了如何定义一个函数模板，并且如何在程序中调用该函数。

```cpp
#include<iostream>
#include<string>
using namespace std;
template<class T>//声明模板的标识
T add(T a,T b)//函数原型
{
	T result;//使用参数化的类型定义变量
	result=a+b;
	return result;
}

int main(int argc, char *argv[])
{
	cout<<"2+3="<<Add(2,3)<<endl;//输出整型类型的“+”运算结果
	//输出string类型的“+”运算结果
	cout<<"std+123="<<Add(string("std"),string("123"))<<endl;
	return 0;
}

//运行结果：
//2+3=5
//sdf+123=sdf123
```
## 18. 类模板

> 类模板使用时与函数模板有点不一样，调用时需要明确指出使用何种数据类型，而不能由编译器自行指定，如下代码所示使用类模板定义一个变量：
> `TemplateSample<int> demo;//指出针对该模板使用int类型`
> 类模板的模板参数定义除了class进行定义以外，还可以使用其他数据类型，但是至少需要有一个模板形参是使用class定义的，如下所示模板参数列表定义了3个参数：
> `tempplate<class T1,class T2,int num>  //定义多个模板参数，且其中一个直接使用int类型`
