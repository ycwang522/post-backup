---
title: 范围for(range for)语句(C++11) + cctype头文件
date: 2016-07-19 10:22:26
categories: C++
tags:
	- C++11
	- cctype
---



适用对象：string对象中的字符操作

这种语句遍历给定序列中的每个元素并对序列中的每个值进行某种操作。

## 语法格式

```c++
for(declaration : expression)
  statement;
```

其中，expression部分是一个对象，用于表示一个序列。

declaration负责定义一个变量，该变量将被用于访问序列中的基础元素。

每次迭代，declaration部分的变量会被初始化为expression部分的下一个元素值。

具体到string，一个string对象表示一个字符的序列，<font color="green">因此string对象可以作为范围for语句的expression的部分。</font>

以下举例说明：

```c++
string str("some string");
//每行输出str中的一个字符
for(auto c : str)	//对于当前str中的每个字符
  cout<<c<<endl;	//输出当前字符
```

for循环将变量c和str联系了起来，其中我们定义循环控制变量的方式与定义任意普通变量的方式是一样的。

该例中，使用auto关键字让编译器来决定变量c的类型，当然，这儿变量c的类型是char类型。

每次迭代，str的下一个字符被拷贝给c。

<!-- more -->

再举一个例子：

```c++
string s("Hello World!!!");
//punct_cnt的类型和s.size的返回类型一样；
decltype(s.size()) punct_cnt = 0;
//统计s中标点符号的数量
for(auto c : s)		//遍历s中的每个字符
  if(ispunct(c))	//如果该字符是标点符号
    ++punct_cnt;	//将标点符号的计数值加1
cout << punct_cnt << " punctuation characters in " << s << endl;

//程序的输出结果是：
//3 punctuation characters in Hello World！！！
```


</br>
</br>
</br>

## 附表：cctype头文件中的函数

该头文件能够获取字符串的某个字符并且对其进行处理。



| 头文件中的函数    | 对应的含义                             |
| :--------- | :-------------------------------- |
| isalnum(c) | 当c是字母或数字时为真                       |
| isalpha(c) | 当c是字母时为真                          |
| iscntrl(c) | 当c是控制字符时为真                        |
| isdigit(c) | 当c是数字时为真                          |
| isgraph(c) | 当c不是空格但可打印时为真                     |
| islower(c) | 当c是小写字母时为真                        |
| isprint(c) | 当c是可打印字符时为真（空格或具有可视形式）            |
| ispunct(c) | 当c是标点符号时为真                        |
| isspace(c) | 当c是空白时为真（空格/横向制表符/纵向制表符/回车符/换行符/） |
| isupper(c)  | c是大写时为真                 |
| isxdigit(c) | c是十六进制数字时为真             |
| tolower(c)  | 如果c是大写，则输出对应的小写；否则原样输出c |
| toupper(c)  | 如果c是小写，则输出对应的大写；否则原样输出c |



