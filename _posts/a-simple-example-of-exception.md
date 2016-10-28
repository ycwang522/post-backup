---
title: 一个简单的异常处理的例子
date: 2016-08-28 16:51:33
categories: C++
tags:
	- 异常处理
---

C++异常处理的处理思想是：将异常的检测与处理分离。

当在一个函数体中检测到异常条件存在，但无法确定相应的处理方法时，将引发一个异常，并由函数的直接或者间接调用检测并处理这个异常。

异常处理通过三个关键字来完成：<font color="blue">throw，try，catch</font>





```cpp
#include<iostream>

using namespace std;

int Div(int x, int y)
{
	if (y == 0)
		throw y;//异常检测
	return x / y;
}

int main()
{
	try		//将可能有异常的执行代码放入try语句块中
	{
		cout << "7/3=" << Div(7, 3) << endl;
		cout << "5/0=" << Div(5, 0) << endl;
	}
	catch (int)//括号中的数据类型为遇到异常时抛出的数据类型
	{
		cout << "除数为0，错误！" << endl;	//异常处理
	}
	cout << "END" << endl;
	return 0;
}
```

```cpp

#include<iostream>

using namespace std;

int main()
{
	int i;
	char ch;
	cout << "请输入一个整数和一个字符:\n" << endl;
	try
	{
		cin >> i >> ch;
		if (i == 0) throw 0;
		if (ch == '!') throw '!';
	}

	catch (int)
	{ cout << "错误：输入为0"<<endl; }
	catch (char)
	{ cout << "错误：输入了字符！"<<endl; }

	cout << "END" << endl;
	return 0;
}

```