---
title: ESSENTIAL C++ 读书笔记
date: 2016-05-27 12:46:36
categories: C++
tags:
	- C++
---


# 1.条件语句
if条件语句，如果括号内的运算式结果为true，那么紧接着if之后的那一条语句会被执行。
当if后接多条语句时，则必须在if语句之后以大括号`｛｝`的形式将这些执行语句括起来，成为一个语句块。

```cpp
if(statement=true)
{
		num++;	
		std::cout<<num<<std::endl;
}
```
<!-- more -->

常见的错误是多条执行语句时忘记加大括号。例如

```cpp

//忘记加语句块
//两条 语句当中，只有num++受if条件的控制
//std::cout<<num<<std::endl;这条语句，无论if条件是否成立，它都会被执行。
if(statement=true)
		num++;	
		std::cout<<num<<std::endl;


```
另外，当面临两种选择，但是其中的一种选择又嵌套了多种选择的情况下，可以采取如下的处理方式：

```cpp

if(user_guess == next_elem)
{
	//用户猜对
}
else
{
	//用户猜错
	if( num_tries == 1 )
	//...
	else
	if( num_tries == 2 )
	//...
	else
	if( num_tries == 3 )
	//...
	...
	else //...

	cout<<"Want you try again? (Y/N)"
	char user_rsp;
	cin >> user_rsp;
	if ( user_rsp == 'N' || user_rsp == 'n' )
		go_for_it = false;
}

```

# 2.vector赋初值

```cpp

int seq_size = 18;
std::vector<int> elem_seq(seq_size);	//申明一个长度为seq_size的vector数组
//对其初始化时，其一的方式是：
elem_seq[0]=1;
elem_seq[1]=2;
...
elem_seq[17]=22;

//另外一个方式是：利用一个已经初始化的array作为该vector的初值
int elem_vals[seq_size]={1,2,3,3,4,7,2,5,12,3,6,10,4,9,16,5,12,22};

//然后以elem_vals的值来初始化vector
vector<int> elem_seq(elem_vals,elem_vals+seq_size);//括号中的两个值都是实际内存，标出了用以将vector初始的范围。


```

# 3.数组指针与指针数组

部分转自[Romi-知行合一 ](http://www.cnblogs.com/Romi/archive/2012/01/10/2317898.html)的博客。

	对于这两个概念的中文翻译太过于拗口，或许其英文名可能更加直白一些。
	
　　指针数组：array of pointers，即用于存储指针的数组，也就是数组元素都是指针。本质是数组，其中的每一个元素都是指针。
　　数组指针：a pointer to an array，即指向数组的指针。本质是指针，指针指向数组的起始地址。

还要注意的是他们用法的区别，下面举例说明。

`int* a[4]`     指针数组     

                 表示：数组a中的元素都为int型指针    

                 元素表示：*a[i]   *(a[i])是一样的，因为[]优先级高于*

`int (*a)[4]`   数组指针     

                 表示：指向数组a的指针

                 元素表示：(*a)[i]  


# 4.指针
有以下的vector对象：

`vector<int>fibonacci,lucas,pell,triangular,square,pentagonal;`
当需要一个指针，指向“元素类型为int型的”vector时，指针应该是什么样？
一般，指针的形式为：

```cpp
type_of_object_pointed_to * name_of_pointer_object
```
由于要的指针是指向vector<int>，因此将其命名为pv，并给定初值。

```
vector<int> *pv=0;
//pv可以依次指向每一个表示数列的vector
pv=&fibonacci
pv=&lucas;
```
同时，还可以采取另一种方式访问这些数列

```cpp
const int seq_cnt = 6;
//定义一个指针数组，数组的长度为seq_cnt;
//每个指针都指向vector<int>对象
vector<int> *seq_addrs[seq_cnt]={
	&fibonacci,&lucas,&pell,
	&triangular,&square,&pentagonal
};
```

# 5.文件的读写

包含头文件`#include<fstream>`

## 5.1 写

定义一个ofstream对象，并将文件名传入：

```cpp
ofstream outfile("seq_data.txt")
```
使用该命令写入文件时，如果不存在该文件，则新建->写入。如果存在该文件，则写入时会丢弃原来的数据。
如果文件已经存在，我们不希望丢弃原有的数据，而是希望将新数据增加到该文件中，那么必须以追加模式(append mode)打开这个文件 。应该提供第二个参数ios_base::app给ofstream对象，命令为`ofstream outfile("seq_data.txt",ios_base::app);`

文件有可能打开失败。
//如果outfile求值结果为false，表示该文件并未成功打开

```cpp

if(!outfile)
```

```cpp

if(!outfile)
	//因为某种原因，文档未能打开
	cerr<<"Unable to save session data!\n";
	//cerr:标准错误设备，和cout一样，cerr将其输出结果定向到用户终端，差别是cerr的输出结果并无缓冲情形，会立即显示到用户终端。
else
	outfile<<user_name<<" "
			<<num_tries<<" "
			<<num_right<<endl;
```

## 5.2 读

```cpp

ifstream infile("seq_data.txt")
int num_tries = 0;
int num_cor = 0;

if(!infile)
{
	//由于某种原因，文件无法打开
}

else
{
	string name;
	int nt;//the number of tries
	int nc;//the number of correct
	
	while(infile >> name)
	{
			infile >> nt >> nc;//会将用户猜对的总数存入nt中，正确的次数读入nc中。
		if(name == user_name)
		{
			cout<<"Welcome back,"<<user_name
				<<"\nYour currents score is"<<nc
				<<" out of "<<nt<<"\nGod luck!\n";

				num_tries = nt;
				num_cor = nc;
		}
	}
}

```

# 6.fibonacci数组

```cpp

bool fibo_elem(int pos, int &elem)
	{
		//**检查位置是否合理
		//1024是人为设定的一个值，因为计算机的数值类型表示的数范围有限，超过上限时会溢出，造成异常。
		if(pos<=0 || pos > 1024)
		{
			elem=0;
			return false;
		}

		//位置在1和2时，直接返回结果为1，而不进行遍历
		int n_1 = 1;
		int n_2 = 1;
		elem = 1;
		for(int ix=3;ix<=pos;++ix)
		{
			elem=n_1 + n_2;
			n_1 = n_2;
			n_2 = elem;
		}
		return true;
	}


```

# 7. pass by value && pass by reference
pass by value(传值)：将对象进行拷贝到被调用函数中，当被调用函数中的值发生变化时，仅仅变化的是这份拷贝，调用函数中的实参并未发生变化。
pass by reference(传址)：
	1>希望得以对传入的对象直接进行修改。
	2>降低复制大型对象的额外负担。
例如：
[2.2 调用函数 例题分析](http://blog.csdn.net/wyc12306/article/details/50975753)一节中，display函数采用的是值传递的方式，虽然这样也能达到显示的目的，但是，如果我们采用如下方式传入vector的地址时，速度可能会更快：

```cpp

void display(const vector<int> &ivec)//函数之中并不会更改vector的内容，加上const加深对程序的理解。
{
	for(int ix=0;ix<ivec.size();++ix)
	{
		std::cout<<ivec[ix]<<std::endl;
	}
}
```
另外，如果有必要，我们也可以选择指针传递的方式来进行

```cpp


void display(std::vector<int> *ivec)
{
	if(!ivec)
	{
		std::cout<<"the vector pointer is 0\n";
		return ;
	}
	for(int ix=0;ix<ivec->size();ix++)
	{
		std::cout<<(*ivec)[ix]<<std::endl;
	}
}

int main()
{
	int a[8]={8,32,3,13,1,21,5,2};
	vector<int >vec(a,a+8);
	
	std::cout<<"vector before sort:";
	display(&vec);//传入vector数组的地址。
}

```

## 7.1 动态内存管理
dynamic extent（动态范围）：内存系由程序的空闲空间分配而来，有时也称为堆内存（heap memory）。由程序员管理，分配：new，释放：delete。
例如：

```cpp


int *pi;
pi=new int;
//heap分配出一个int类型的对象，然后将其地址赋值给pi，默认情况下，未被初始化。
pi=new int(1024);//将分配的内存用1024初始化。

//分配数组
int *pia=new int[24];//指针指向数组第一个元素的地址，也就是数组的首地址，数组未被初始化。

//释放分配的内存
delete pi;

//如果要删除数组中的所有对象，则应该在delete和数组指针之间加上一个空的下标运算符
delete [] pia;

//因为某种原因，程序员并未使用delete表达式，由heap分配而来的对象就永远不会被释放，这称为内存泄漏（memory leak）


```

## 7.2 局部静态对象
在求Fibonacci数组的函数当中，每次调用`fibona_seq(int size)`函数时，都会从头开始递归计算。
例如：

```cpp

fibona_seq(24);
fibona_seq(8);
fibona_seq(16);
//每一次调用时，都从头开始计算
//如上，如果能将第一次调用计算的值储存，则调用后两个函数时则不需要再调用，直接得出结果即可。
```
基于以上问题，引入局部静态变量的使用。
例如，如下代码：

```cpp


#include<vector>
#include<iostream>

using namespace std;

const vector<int> *fibona_seq(int size)
{
	const int max=100;
	//局部静态变量（静态容器）。每次被调用时不会像局部非静态变量那样破坏又重新建立
	static vector<int>ivec;
	
	if(size<=0 || size>100)
	{
		cout<<"Error input.\n";
		return 0;
	}
	
	//如果传入的size比当前静态容器中元素的个数要多，说明需要计算的数据不包含在当前的静态容器中
	//需要继续往后计算，并将计算得到的结果存入静态容器当中。
	for(int ix>ivec.size();ix<size;++ix)
	{
		if(ix==0 || ix==1)
		{
			ivec.push_back(1);
		}
		else
		{
			ivec.push_back(ivec[ix-1]+ivec[ix-2]);
		}
	}
	
	//如果①输入的数值合理&&②当前调用函数传入的size小于当前静态容器中的长度
	//说明，所求的数值包含于当前的静态容器当中，此时不需要再计算，直接返回当前静态容器的地址即可。
	return &ivec;
}


```

## 7.3 设定头文件
头文件中，包含与程序相关的所有函数申明都放在该文件中。
函数的定义只能有一份，倒是可以有许多分声明。不将函数的定义放到头文件，是因为同一个程序的多个代码文件可能都会包含这个头文件。

	原因：
	首先必须了解编译的过程，编译的第一步是把所有的CPP文件编译成为点O文件，而且每一个点CPP文件都是单独编译的，该点CPP文件中用到的类型必须在它所include 的头文件当中找到，相当于把它所有include的文件中的代码都加到该CPP文件的前面，但是声明的部分将不会出现在编译后的点O文件，相当于每个CPP文件都是单独编译，因此它的ifndef在一个文件里是没有用的，两个CPP文件里如果包含同一个有ifndef的头文件，效果是两个CPP文件都把该头文件加到它的前面，但不会把声明的部分放到点O文件中，而会把头文件中定义的部分都输出到编译后的点O文件当中
	因此如果在头文件当中有一个定义，那么如果有两个CPP文件当中include了它，那么将会出现重定义错误，multiple definition of
 
	由于模板不是真实的定义，所以可以放在头文件当中
 
	声明作用只是在链接的时候进行查找，定位，如果出现对一个声明的两个定义，则会出错


**但是内联函数的定义必须要放到头文件中**
		
		其实原理很简单，就是当用#include 包含一个文件得时候，预处理得时候会直接展开这一个文件，如果文件中放有某个函数的定义，事实上就相当于把该函数定义放在了这个包含这个文件（上面得例子中得print_inline.h）的文件(main.c)中，这样就可以在main中将print_inline函数内联展开
		在很多时候，由于某些函数需要经常被调用，为了加快程序的执行速度，经常要用到inline，但是如果inline函数的定义和声明是分开的，而在另外一个文件中需要调用这些inline函数得时候，内联是无法在这些调用函数内展开的（上面得第二个例子），只能调用。这样内联函数在全局范围内就失去了作用。解决的办法就是把内联函数得定义放在头文件中，当其它文件要调用这些内联函数的时候，只要包含这个头文件就可以了
——转自[sunshine的博客](http://blog.sina.com.cn/s/blog_6648c114010130ry.html)

# 8.基于对象的编程风格

## 8.1 构造函数和析构函数
首先，构造函数constructor不应指定返回类型，亦不用任何返回值。可以被重载

最简单的构造函数是默认构造函数default constructor。不需要任何参数，其包含了两种情况

- 不接受任何参数

```cpp

Triangular::Triangular
{
	//default constructor
	_length=1;
	_beg_pos=1;
	_next=0;
}
```

- 这种情况更常见，它为每个参数提供了默认值：

```cpp


class Triangular
{
	public:
	//default constructor
	Triangular(int len=1,int bp=1);
};

Triangular::Triangular(int len,int bp)
{
	_length = len > 0 ? len : 1;
	_beg_pos = bp > 0 ? bp : 1;
	_next = _beg_pos - 1;
}


```

### 8.1.1 构造函数的第二种初始化方法——成员初始化列表

```cpp

Triangular::Triangular( const Triangular &rhs ) : _length(rhs._length),_beg_pos(rhs._beg_pos),_next(rhs._beg_pos-1)
{ }//空格
//成员初始化列表紧跟在参数列表最后的冒号后边，是个以逗号分隔的列表。
//其中，欲赋值给member的数值被放在member名称后边的小括号中。

```

### 8.1.2 析构函数（destructor）

绝对不会 有返回值，也没有任何参数。
由于参数列表为空，所以绝对不可能被重载。

考虑如下的Matrix class，其constructor使用new表达式从heap中分配double数组所需的空间。
destructor则负责释放这些内存：

```cpp

class Matrix
{
public:
	Matrix( int row , int col ) : _row(row), _col(col)
	{
		_pmat = new double[row*col];
	}

	~Matrix()
	{
		delete [] _pmat;
	}
private:
	int _row,_col;
	double *_pmat;
};


```

### 8.1.3 成员逐一初始化

```cpp

Triangular tri1(8);
Triangular tri2=tri1;

```
class data member会被依次复制。
本例中的`_length,_beg_pos,_next`都会依次从tri1复制到tri2，此即**成员的逐一初始化操作**
这样逐一初始化有一个潜在的问题，如下

```cpp


{
	Matrix mat(4,4);
	//该处，constructor作用
	{
		Matrix mat2=mat;
		//该处，进行逐一初始化
		//使用mat2
		//调用mat2的析构函数释放数据与内存
	}
	//该处使用mat
	//该处调用mat的析构函数释放内存与数据
}
//本例中，其中有一项初始化是mat2._pmat = mat._pmat;
//两个对象的_pmat指针都指向heap内的同一个数组，当调用mat2的析构函数时，数组空间被释放，而mat的指针仍然指向那个数组，对已经释放掉的数组进行操作时极为危险的行为。

?>
```

这种情况下，可以提供一个copy constructor，其可以改变“成员逐一初始化”的默认行为。我们可以产生一个独立的数组副本，这样便可以使某个对象的析构操作不至于影响到另一个对象

```cpp

Matrix::Matrix(cosnt Matrix &rhs) : _row(rhs._row),_col(rhs._col)
{
	//对rhs._pmt所指的数组产生一份完全复本
	int elem_cnt = row * col;
	_pmt = new double [elem_cnt];
	
	for( int ix = 0; ix < elem_cnt ; ix ++)
		_pmat[ix] = rhs._pmat[ix];
}

```

类中的**const常量必须在构造函数的初始化列表中初始化**，而**不能在构造函数的函数体中初始化**。


## 8.2 类的静态成员
部分转自[MoreWindows Blog](http://blog.csdn.net/morewindows/article/details/6721430)。
		
	在C++中，静态成员是属于整个类的而不是某个对象，静态成员变量只存储一份供所有对象共用。所以在所有对象中都可以共享它。使用静态成员变量实现多个对象之间的数据共享不会破坏隐藏的原则，保证了安全性还可以节省内存。
 
eg1：
通过类名调用静态成员函数和静态成员变量：

```cpp

class Point
{
public:	
	void init()
	{  
	}
	static void output()
	{
	}
};
void main()
{
	Point::init();
	Point::output();
}


```

编译出错：error C2352: 'Point::init' : illegal call of non-static member function

结论1：**不能通过类名来调用类的非静态成员函数。**

eg2:通过类的对象调用静态成员函数和静态成员变量

```cpp


void main()
{
	Point pt;
	pt.init();
	pt.output();
}

?>
```
编译通过
结论2：**类的对象可以使用静态成员函数和非静态成员函数。**

eg3：在类的静态成员函数中使用类的非静态成员

```cpp


#include <stdio.h>
class Point
{
public:	
	void init()
	{  
	}
	static void output()
	{
		printf("%d\n", m_x);
	}
private:
	int m_x;
};
void main()
{
	Point pt;
	pt.output();
}

?>
```

编译出错：error C2597: illegal reference to data member 'Point::m_x' in a static member function

因为静态成员函数属于整个类，在类实例化对象之前就已经分配空间了，而类的非静态成员必须在类实例化对象后才有内存空间，所以这个调用就出错了，就好比没有声明一个变量却提前使用它一样。

结论3：**静态成员函数中不能引用非静态成员。**

eg4:在类的非静态成员函数中使用类的静态成员。

```cpp


class Point
{
public:	
	void init()
	{  
		output();
	}
	static void output()
	{
	}
};
void main()
{
	Point pt;
	pt.output();
}


```
编译通过，理由如上所述。

但是反过来却不行，不能在类的静态成员函数中使用类的非静态成员。


结论5：**类的静态成员变量必须先初始化再使用。**

结合上面的五个例子，对类的静态成员变量和成员函数作个总结：

一。静态成员函数中不能调用非静态成员。

二。非静态成员函数中可以调用静态成员。因为静态成员属于类本身，在类的对象产生之前就已经存在了，所以在非静态成员函数中是可以调用静态成员的。

三。静态成员变量使用前必须先初始化，而且只能在类体外进行。(如int MyClass::m_nNumber = 0;)，否则会在linker时出错。



