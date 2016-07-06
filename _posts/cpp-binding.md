---
title: 类的早期绑定&迟绑定
date: 2016-05-30 10:38:20
categories: C++
tags:
	- C++
	- 类
	- 动态绑定
---

对于以下的例子：
```cpp
#include<iostream>
using namespace std;
class animal
{
    public:
        void sleep(){
            cout<<"animal sleep"<<endl;
            
        }
    void breathe(){
     cout<<"animal breathe"<<endl;
    }
};
class fish:public animal
{
    public:
        void breathe(){
            cout<<"fish bubble"<<endl;
        }
};

int main()
{
    fish fh;
    animal *pAnimal=&fh;
    pAnimal->breathe();
}
```

<!-- more -->

答案是输出：`animal breathe`
产生原因分析：
<font color=green size=0.5>著作权归作者所有。
商业转载请联系作者获得授权，非商业转载请注明出处。
作者：Shiyang Ao
链接：https://www.zhihu.com/question/25572937/answer/34441503
来源：知乎</font>

派生类和基类的关系并不是两个独立的类型，在派生关系中，派生类型“是一个”基类类型(Derived class is a base class)。
在C++语法里规定：基类指针可以指向一个派生类对象，但派生类指针不能指向基类对象。
用问题里的例子来说DerivedClass is a BaseClass
派生类型之间的数据结构类似于这样：

`BaseClass : [Base Data]
DerivedClass : [Base Data][Derived Data]`
派生类型的数据附加在其父类之后，这意味着当使用一个父类型指针指向其派生类的时候，父类访问到的数据是派生类当中由父类继承下来的这部分数据对比起见，我们再定义一个派生类的派生类class  `DerivedDerivedClass : public DerivedClass`
它的数据结构如下：
`DerivedDerivedClass : [Base Data][Derived Data][DerivedDerived Data]`

而通过**基类指针** BaseClass *pbase
访问每一个类型的数据部分为：
**[Base Data]**
**[Base Data]**[Derived Data]
**[Base Data]**[Derived Data][DerivedDerived Data]

通过**派生类指针** DerivedClass *pderived
访问每一个类型的数据部分为：
[Base Data] 不能访问，派生类型指针不能指向基类对象（因为数据内容不够大，通过派生类指针访问基类对象会导致越界）
**[Base Data][Derived Data]**
**[Base Data][Derived Data]**[DerivedDerived Data]

## 动态多态性
前面输出的结果是因为编译器在编译的时候，就已经确定了对象调用的函数的地址，要解决这个问题就要使用**迟绑定（late binding）技术**。当编译器使用迟绑定时，就会在运行时再去确定对象的类型以及正确的调用函数。而要让编译器采用迟绑定，就要在基类中声明函数时使用virtual关键字（注意，这是必须的，很多学员就是因为没有使用虚函数而写出很多错误的例子），这样的函数我们称为虚函数。一旦某个函数在基类中声明为virtual，那么在所有的派生类中该函数都是virtual，而不需要再显式地声明为virtual。
将上边一段代码修改以后：

```cpp
#include<iostream>
using namespace std;
class animal
{
	public:
		void sleep(){
			cout<<"animal sleep"<<endl;
			
		}
	virtual void breathe(){
     cout<<"animal breathe"<<endl;
    }
};
class fish:public animal
{
	public:
		void breathe(){
			cout<<"fish bubble"<<endl;
		}
};

int main()
{
	fish fh;
	animal *pAnimal=&fh;
	pAnimal->breathe();
}
```
运行结果：`fish bubble`


## 类中内存的分布

基类的内存分布情况：
1)对于无虚函数的类A

```
class A
{
void g(){.....}
};
```

则sizeof(A)=1；

如果改为如下：
class A
{
public:
    virtual void f()
    {
       ......
    }
    void g(){.....}
}
则sizeof(A)=4! 这是因为在类A中存在virtual function,为了实现多态，每个含有virtual function的类中都**隐式包含**着一个**静态虚指针** vfptr指向该类的静态虚表vtable, vtable中的表项指向类中的每个virtual function的入口地址。

## 动态绑定过程的实现

其过程如下：
    程序运行到动态绑定时，通过基类的指针所指向的对象类型，通过vfptr找到其所指向的vtable，然后调用其相应的方法，即可实现多态。
例如：
A c;
B e;
A *pc=&e; //设置breakpoint，运行到此处
pc=&c;
![这里写图片描述](http://images.cnitblog.com/i/448111/201403/281705337502078.png)
