---
title: 类中构造函数的顺序
date: 2016-06-08 15:29:08
categories: C++
tags:
	- C
	- C++
	- 构造函数
---


在一个class及其继承类中，不但有父类，还有类的成员，还有子类。那么父类的构造函数，成员变量的构造函数以及子类的构造函数的顺序是怎么样的，有以下例题：
```cpp
# include <iostream>
using namespace std;
class A  
{  
public:  
    A()  {  cout<<"create A"<<endl;   }  
    A(const A& other){ cout<<"copy A"<<endl;} //复制构造函数
    ~A() {    cout<<"~A"<<endl;   }  
}; 
class C
{
public:
    C()  {  cout<<"create C"<<endl;   } 
    C(const A& other){ cout<<"copy C"<<endl;} //复制构造函数
    ~C() {    cout<<"~C"<<endl;   }  
};
class B:public A  
{  
public:  
    B()
    {  
        cout<<"create B"<<endl;
    }  
    ~B()  
    {  
        cout<<"~B"<<endl;  
    }  
private:  
    C _a; 
};  
       
int main(void)  
{
        B b; 
        cout<<"------------------------"<<endl;
}

```

<!-- more -->
该程序的输出结果为
```cpp
create A
create C
create B
------------------------
~B
~C
~A

```
-感谢@牛客网 网友提供以上题目。

从以上的构造顺序可以看出：

<font color="red">父类的构造函数>成员变量C的构造函数>子类的构造函数</font>

而析构的过程恰恰相反。

