---
title: Essential C++ 课后习题
date: 2016-05-30 10:23:35
categories: C++
tags:
	- C++
	- Essential C++
---

##Exercise 1.6

```
/*
***********************************************
**Name：Wang
**Data:2016-3-24 21:06:34
**Readme:从标准设备读取一串整数，并将读入的整数依次放到array及vector
		然后遍历这两种容器，求取数值的总和。
		将总和及平均值输出至标准输出设备。
vertirion：vector
***********************************************
*/

#include<vector>
#include<iostream>
#include<string>
#include<stdlib.h>

using namespace std;

#define SIZE 10

int main()
{
	//int a[SIZE];
	int test;
	int cnt = 1;

	std::vector<int> ivec;
	std::cout << &ivec << endl;
	for (int i = 0; i < SIZE; i++)
	{
		std::cout << "请输入第" << cnt << "个数:";
		std::cin >> test;
		ivec.push_back(test);
		++cnt;
	}

	int sum = 0;
	for (int i = 0; i < SIZE; i++)
	{
	
		int num = ivec[i];
		std::cout << num << endl;
		sum += num;		
	}
	cout << "数列的和为" << sum << endl;
	cout << "数列的平均值为" << sum / SIZE << endl;
	system("pause");
}
```

<!-- more -->

```
/*
***************************************
Author:Wang
data:2016-3-24 17:22:13
Readme:从标准设备读取一串整数，并将读入的整数依次放到array及vector
		然后遍历这两种容器，求取数值的总和。
		将总和及平均值输出至标准输出设备。
vertirion：array
***************************************
*/

#include<iostream>
#include<string>
#include<stdlib.h>
#include<vector>

#define SIZE 10

using namespace std;

int main()
{
	int a[SIZE];
	int test;
	int cnt = 1;
	for (int i = 0; i < SIZE; i++)
	{
		std::cout << "请输入第" << cnt << "个数:";
		std::cin >> test;
		a[i] = test;
		++cnt;
	}

	int sum = 0;
	for (int i = 0; i < SIZE; i++)
	{
		sum += a[i];	
	}

	cout << "数列的和为" << sum << endl;
	cout << "数列的平均值为" << sum / SIZE << endl;
}

```
##Exercise 1.7 

```
/*
***********************************************
**Name：Wang
**Data:2016-3-24 22:14:04
**Readme:输入string类型的数据，然后存盘。
		写一个程序，打开该文本文件，将每个字都读取到vector<string>对象中。
		遍历该vector，将内容显示到cout。
		使用泛型算法sort()，对所有文字进行排序
		#include<algorithm>
		sort(container.begin(),container.end());
vertirion：vector
***********************************************
*/

#include<iostream>
#include<fstream>
#include<vector>
#include<string>
#include<algorithm>

#define SIZE 5

using namespace std;

int main()
{
	std::ofstream outfile1("text.txt");
	std::string str1;
	std::cout << "Please enter a string:" << std::endl;
	for (int i = 0; i < SIZE; i++)
	{
		std::cin >> str1;
		outfile1 << str1 << std::endl;
	}

	std::vector<string> str2;
	std::ifstream infile1("text.txt");
	/*for (int i = 0; i < 2; i++)
	{*/
	string name;
	for (int i = 0; i < SIZE; i++)
	{
		infile1 >> name ;
		str2.push_back(name);
		//std::cout << name << endl;
	}
	for (int i = 0; i < SIZE; i++)
	{
		std::cout << str2[i] << std::endl;
	}

	sort(str2.begin(), str2.end());
	std::cout << "sort排序之后：" << std::endl;
	ofstream outfile2("sort.txt");
	for (int i = 0; i < SIZE; i++)
	{
		outfile2 << str2[i] << std::endl;
	}
	system("pause");

}
```
##2.2 调用函数  例题分析

```
#include<iostream>
#include<vector>
#include<fstream>
//#include"27.h"

void swap(int &a, int &b)
{
	int temp = a;
	a = b;
	b = temp;
}


void display(std::vector<int>ivec)
{
	for (int ix = 0; ix < (ivec.size()); ++ix)
	{
		std::cout << ivec[ix] << std::endl;
	}
}

void Bubble(std::vector<int>vec1)
{
	std::ofstream outfile("1.txt");
	for (int ix = 0; ix < (vec1.size()-1 );ix++)
		for (int jx = ix + 1; jx < vec1.size(); jx++)
		{
			if (vec1[ix]>vec1[jx])
			{
				outfile << "交换之前：about to call swap! " << "ix=" << ix << " jx=" << jx << " swaping " << vec1[ix] << "  with " << vec1[jx] << "\n";
				swap(vec1[ix], vec1[jx]);
				outfile << "交换之后：about to call swap! " << "ix=" << ix << " jx=" << jx << " swaping " << vec1[ix] << "  with " << vec1[jx] << "\n";

			}
		}
}

int main()
{
	int a[8] = { 8, 2, 56, 65, 54, 6, 2, 10 };
	std::vector<int>vec(a, a + 8);//为vector对象赋值。
	std::cout << "vector before sort:\n";
	display(vec);

	Bubble(vec);

	std::cout << "vector after sort:\n";
	display(vec);

	system("pause");
}
```
代码分析：
1.对array进行赋值初始化，然后将array赋值给vector数组。
2.调用display函数，打印出排序之前的数列。
3.调用排序函数（冒泡排序法实现）
4.打印出排序之后的序列。

结果：排序前后数列的顺序并未发生改变。说明程序有错误。

```
vector before sort:
8
2
56
65
54
6
2
10
vector after sort:
8
2
56
65
54
6
2
10
```

错误分析：
1.在Bubble函数中将函数中变化的数据输出至文件，查看其变化。在if语句中，打印出数列的序号以及对应的数据是否发生变化，如下

```
交换之前：about to call swap! ix=0 jx=1 swaping 8  with 2
交换之后：about to call swap! ix=0 jx=1 swaping 2  with 8
交换之前：about to call swap! ix=1 jx=5 swaping 8  with 6
交换之后：about to call swap! ix=1 jx=5 swaping 6  with 8
交换之前：about to call swap! ix=1 jx=6 swaping 6  with 2
交换之后：about to call swap! ix=1 jx=6 swaping 2  with 6
交换之前：about to call swap! ix=2 jx=4 swaping 56  with 54
交换之后：about to call swap! ix=2 jx=4 swaping 54  with 56
交换之前：about to call swap! ix=2 jx=5 swaping 54  with 8
交换之后：about to call swap! ix=2 jx=5 swaping 8  with 54
交换之前：about to call swap! ix=2 jx=6 swaping 8  with 6
交换之后：about to call swap! ix=2 jx=6 swaping 6  with 8
交换之前：about to call swap! ix=3 jx=4 swaping 65  with 56
交换之后：about to call swap! ix=3 jx=4 swaping 56  with 65
交换之前：about to call swap! ix=3 jx=5 swaping 56  with 54
交换之后：about to call swap! ix=3 jx=5 swaping 54  with 56
交换之前：about to call swap! ix=3 jx=6 swaping 54  with 8
交换之后：about to call swap! ix=3 jx=6 swaping 8  with 54
交换之前：about to call swap! ix=4 jx=5 swaping 65  with 56
交换之后：about to call swap! ix=4 jx=5 swaping 56  with 65
交换之前：about to call swap! ix=4 jx=6 swaping 56  with 54
交换之后：about to call swap! ix=4 jx=6 swaping 54  with 56
交换之前：about to call swap! ix=4 jx=7 swaping 54  with 10
交换之后：about to call swap! ix=4 jx=7 swaping 10  with 54
交换之前：about to call swap! ix=5 jx=6 swaping 65  with 56
交换之后：about to call swap! ix=5 jx=6 swaping 56  with 65
交换之前：about to call swap! ix=5 jx=7 swaping 56  with 54
交换之后：about to call swap! ix=5 jx=7 swaping 54  with 56
交换之前：about to call swap! ix=6 jx=7 swaping 65  with 56
交换之后：about to call swap! ix=6 jx=7 swaping 56  with 65

```
以上数据说明
	1>swap函数能够实现其功能，完成数据的交换。
	2>if的条件语句书写正确，无错误。
所以，现在的问题是，被调用的Bubble函数中的值只在该副本上发生变化，并没有传递到调用函数中，因此，为了使Bubble()函数的参数和传入的实际对象产生关联，应该使用pass by reference的方式传递参数。

程序修正方法：
在Bubble（）函数的参数列表中，将程序第22行参数(pass by value)修改成**引用(pass by reference)**的形式即可。

```
void Bubble(std::vector<int> &vec1)
```
修改以后，程序的结果为。

```
vector before sort:
8
2
56
65
54
6
2
10
vector after sort:
2
2
6
8
10
54
56
65
```

##Exercise 2.3



```
/*
***********************************************
**Name：Wang
**Data:2016-03-27 10:42:23
**Readme:将2.2中的Pentagonal数列求值拆分为两个函数
		其中之一为inline，用来检验元素个数是否合理。
		如果的确合理，而且尚未被计算，便执行第二个函数，执行实际的求值工作。
vertirion：vector，inline
***********************************************
*/

#include<vector>
#include<iostream>
#include<string>
#define MAX 100

using namespace std;

vector<int>ivec1;

inline bool NumberIsOk(const int &number)
{
	if (number <= 0 || number > MAX)
	{
		std::cout << "Error Input.\n" << endl;
		return false;
	}
	return true;
}

void pentagonal(const int &number)
{
	if (!NumberIsOk(number))
	{
		return;
	}

	for (int ix = 1; ix <= number; ++ix)
	{
		ivec1.push_back(ix*(3*ix-1)/2);
	}
}

void display_message(vector<int> &ivec1,const string &str1)//
{
	for (int ix = 0; ix < ivec1.size(); ix++)
	{
		std::cout << ivec1[ix] << endl;
	}
	std::cout << "TypeName of vector is "<<str1;
	std::cout << std::endl;
}


int main()
{
	//vector<int>vec;
	int number1;
	std::cout << "Please enter a int number:";
	std::cin >> number1;
	pentagonal(number1);
	display_message(ivec1,"int");
	
	system("pause");
	return 0;
}
```
##Exercise 2.6
以template形式完成函数的重载。

鉴于2.5中传入的参数有不同的类型，因此，可以定义template带有不同类型参数的模板。

```
//对于传入两个数的参数类型
//传入的两个参数类型相同
//同为int，float，或者string类型
template<typename T1>
void max(T1 a,T1 b)
{
	statement;
}

//对于传入数组和整数类型的模板
template<typename T2>
void max(T2 a[],T2 b)
{
	statement2;
}

//传入单个参数
template<typename T3>
void max(T3 a)
{
	statement3;//
}

//以下，如此。
```



