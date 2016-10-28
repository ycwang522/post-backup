---
title: 笔试中应该注意的坑之基础篇
date: 2016-08-31 09:13:18
categories: C++面试
tags:
	- C++
---

# 1. if语句

if语句中的坑主要是if语句与各种零值的比较语句写法：
## 布尔变量与零值的比较

布尔变量的语义，零值为“假”（记为FALSE），任何非零值都是“真”（记为TRUE）。TRUE的值究竟是什么并没有统一标准。例如VC++中将TRUE定义为1，而Visual Basic则将TRUE定义为-1.所以不能将布尔变量直接与TRUE，FALSE或者1、0进行比较。

比较良好的写法如下：

比如有一bool型变量flag，bool flag，判断flag与零值的比较的语句如下：

    if(flag)	//如果flag为真则执行if语句

以下写法均为不良风格，例如：

	if (flag == TRUE)   
	if (flag == 1 )   
	if (flag == FALSE)   
	if (flag == 0) 

## 整型变量与零值比较

例如 int value与零值的比较可以写成如下：

	if(value == 0)
	if(value != 0)

而不可魔方布尔变量风格写成

	if(value)
	if(!value)

## 浮点变量与零值比较
如果有一double型变量x，其与零值的比较可以写为如下语句：

	if(x-0.0>1e-6)	//1e-6是允许的精度范围
	if ((x>=-EPSINON) && (x<=EPSINON)) 

其中EPSINON 是允许的误差（即精度）

<!-- more -->

# 关于循环语句

- for循环多重循环中，如果有可能，<font color="red">应将最长的循环放在最内层，最短的循环放在最外层，减少CPU跨切循环层的次数。</font>

- 如果循环体内存在逻辑判断，并且循环次数很大，宜将逻辑判断移到循环体的外面。如下图右框内的程序，简洁度略有下降，但是执行效率却大幅度提高。

![](http://img.blog.csdn.net/20130829143532000?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWFvcWlhbmcyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

- 建议for 语句的循环控制变量的取值采用“半开半闭区间”写法。

比如：`int i=0; i< N; i++`

# switch 语句

- 每个case结尾的break不要忘记，否则导致多个分支重叠执行。
- 不要忘记最后的default分支

![](http://img.blog.csdn.net/20130829143610859?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWFvcWlhbmcyMDEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 常量

- 宏定义常量#define
	- 宏常量没有数据类型，编译器不进行类型检查
- const常量
	- const常量有数据类型 ，编译器可以进行类型舰船

- 类中的常量
	- 不能在类声明中初始化const数据成员，**const数据成员的初始化只能在类构造函数的初始化列表中进行。** 


<font color=red>const数据成员只在某个对象生存期间是常量，而对于整个类而言确是可变的，因为类可以创建多个对象，不同的对象其const数据成员的值可以不同。</font>

# 函数

## 函数内部的实现

- 函数体的“入口处”，对参数的有效性进行检查。
- 函数的“出口处”，对return语句的正确性和效率进行检查。
	- <font color="red">return 语句不可返回指向“
	- 
	- 
	- ”的“指针”或者“引用”，因为该内存在函数体结束时被自动销毁</font>
	- 搞清楚返回的究竟是“值”，“指针”还是“引用”
	
## 关于断言

<font color=red>断言assert是仅在debug版本下起作用的宏。</font>它用于检查**不应该**发生的情况。运行过程中，如果assert的参数为假，程序就会中断。

## 引用和指针

关于引用一些非常重要的规则<区别与指针>：
- 引用被创建的同时必须被初始化
- 不能有NULL引用，引用必须与合法的存储单元关联
- 一旦被初始化，就不能改变引用的关系

# 内存管理

## 常见的内存错误

- 内存分配并未成功，却使用了它。
 - 在函数入口处使用assert进行检查。
- 内存分配成功，但是并未初始化
	- 无论使用何种方式创建数组，都应该赋初值
- 分配成功且初始化，但操作越过了内存边界
	- 数组遍历时多1或者少1的操作，注意边界条件
- 忘记了释放内存
- 释放了内存仍然使用它
	- 程序中的对象调用过于复杂，在难以搞懂是否释放的情况下使用
	- return返回指向栈上的内存
	- **使用free或者delete释放内存后，没有将指针设置为NULL，导致产生“野指针”**

## 指针和数组

不少地方指针和数组可以相互替换使用。但是两者绝不是等价的。

<font color=red>数组要么在静态存储区被创建，要么在栈上创建。数组名对应着一块内存，其地址与容量在生命周期内保持不变，只有数组的内容可以改变。指针可以随时指向任意类型的内存块，它的特征是“可变”，所以我们常用指针来操作动态内存。指针远比数组灵活，但也更危险。</font>

- 关于两者的内容修改
```cpp
char a[] = “hello”;   
a[0] = ‘X’;   
cout << a << endl;   
char *p = “world”; // 注意p指向常量字符串  
p[0] = ‘X’; // 编译器不能发现该错误  
cout << p << endl;
```

字符数组a的容量是6个字符，其内容为hello\0（位于<font color=red>栈</font>上）。a的内容可以改变，如a[0]=‘X’。

指针p指向常量字符串“world”（位于**静态存储区**，内容为world\0），**常量字符串的内容是不可以被修改的**。从语法上看，编译器并不觉得语句p[0]= ‘X’有什么不妥，但是该语句企图修改常量字符串的内容而导致运行错误。

- 关于两者进行内容复制



<font color="red">不能对数组名进行直接复制与比较。</font>若想把数组a的内容复制给数组b，不能用语句b = a ，否则将产生编译错误。应该用标准库函数**strcpy**进行复制。

同理，比较b和a的内容是否相同，不能用if(b==a) 来判断，应该用标准库函数**strcmp**进行比较。

语句p =a 并不能把a的内容复制指针p，而是把a的地址赋给了p。**要想复制a的内容，可以先用库函数malloc为p申请一块容量为strlen(a)+1个字符的内存，再用strcpy进行字符串复制。**同理，语句if(p==a) 比较的不是内容而是地址，应该用库函数strcmp来比较。

## 指针参数如何传递内存

<font color="red">如果函数的参数是一个指针，不要指望用该指针去申请动态内存。</font>

如下例中，Test函数的语句GetMemory(str,200)并没有使str获得期望的内存，str依旧是NULL。
```cpp
void GetMemory(char *p, int num)   
{   
p = (char *)malloc(sizeof(char) * num);   
}   
void Test(void)   
{   
char *str = NULL;   
GetMemory(str, 100);  // str 仍然为NULL   
strcpy(str, "hello"); // 运行错误  
}   
 ```

	我们来分析一下，其实上例的问题出在函数GetMemory中。
	编译器总是要为函数的每个参数制作临时副本，指针参数p的副本是_p，编译器使 _p= p。
	如果函数体内的程序修改了_p的内容，就导致参数p的内容作相应的修改。这就是指针可以用作输出参数的原因。
	在本例中，_p申请了新的内存，只是把_p所指的内存地址改变了，但是p丝毫未变（形参有时候就是很害人）。
	所以函数GetMemory并不能输出任何东西。
	事实上，每执行一次GetMemory就会泄露一块内存，因为没有用free释放内存。

而以下的方案确实可行的

```cpp
char *GetMemory3(int num)   
{   
char *p = (char *)malloc(sizeof(char) * num);   
return p;   
}  
void Test3(void)   
{   
char *str = NULL;   
str = GetMemory3(100);   
strcpy(str, "hello");   
cout<< str << endl;   
free(str);   
} 
```

以上代码的可执行性不用质疑。

用函数返回值来传递动态内存这种方法虽然好用，但是常常有人将return语句用错。
强调的是<font color="red">不要使用return语句返回指向“栈内存”的指针，因为该指针在函数结束时自动消亡！</font>而上述程序的指针p指向由malloc分配的**堆内存**，因此可以放心return。

而以下的代码则不可取
```cpp
char *GetString(void)   
{   
char p[] = "hello world";   
return p;  // 编译器将提出警告  
}   
void Test4(void)   
{   
char *str = NULL;   
str = GetString(); // str 的内容是垃圾  
cout<< str << endl;   
} 
```

此处，指针p是一个栈的指针。

# 关于free和delete

	delete和free只是将指针指向的内存释放掉了，
	但是指针本身却被留下来了
	如果不将free/delete之后的指针指向NULL，
	该指针会变成一野指针。


```cpp
char *p = (char *) malloc(100);   
strcpy(p, “hello”);   
free(p);  // p  所指的内存被释放，但是p所指的地址仍然不变  
…  
if(p != NULL)  // 没有起到防错作用  
{   
strcpy(p, “world”); // 出错  
}
```

如果跟踪调试上述例子，会发现指针p被free以后其地址仍然不变，不是NULL，只是该地址对应的内存时垃圾，p变成了野指针。
如果此时不将p设置为NULL，会让人误以为p是个合法的指针。

# 关于野指针

野指针不是NULL指针，是指向垃圾内存的指针。
人们一般不会错用NULL指针，引用使用if语句可以判断。
但是if语句对其不起作用。

野指针出现的原因

- **指针变量没有被初始化**	

- **指针p被free或者delete之后，没有设置为NULL**
- **指针操作超越了变量的作用范围**

# malloc/free

malloc的原型如下；
    void *malloc(size_t size)

用malloc申请一块长度为length的整数类型的内存，

	int *p = (int *) malloc(sizeof(int) * length)

这儿有两点需要注意：类型转换和sizeof

- 由于malloc的返回值类型是void *，无类型指针。所以在调用malloc时要显示地进行类型转换
- malloc函数本身并不识别申请的内存是什么类型，它只关心的是内存的字节总数。于是用malloc申请的时候使用int，float等数据类型的确切字节值传递给函数作为参数




