---
title: 华为OJ系列（一）
date: 2016-08-10 16:28:38
categories: C++
tags:
	- 华为OJ
---

# 1. 数字颠倒

描述：

> 输入一个整数，将这个整数以字符串的形式逆序输出
程序不考虑负数的情况，若数字含有0，则逆序形式也含有0，如输入为100，则输出为001

```cpp
#include<iostream>
 
using std::cin;
using std::cout;
using std::endl;
 
void RevertInt(int number)
{  
    int Rt1 = 0;
    int x1 = 10;
    while (number!=0)
    {
        Rt1 = number%x1;
        cout << Rt1;
        number = number / 10;      
    }      
}
 
int main()
{
    int Input1;
    cin >> Input1;
    RevertInt(Input1);
    return 0;
}
```

<!-- more -->

-----

# 2. 字符串反转

描述：

> 写出一个程序，接受一个字符串，然后输出该字符串反转后的字符串。

```cpp
#include<iostream>
#include<stack>
#include<string>

using std::cin;
using std::cout;
using std::endl;
using std::string;
using std::stack;

stack<char>istack;
void RevertChar(string str)
{
	for(auto c : str)		//C++ 11标准
        istack.push(c);
    while(!istack.empty())
    {
    	cout<<istack.top();
        istack.pop();
    }
}

int main()
{
	string str1;
    cin>>str1;
    RevertChar(str1);
    return 0;
}
```

---

# 3. 字符串最后一个单词的长度

题目描述

> 计算字符串最后一个单词的长度，单词以空格隔开。

输入描述

> 一行字符串，非空，长度小于5000。

输出描述:
> 整数N，最后一个单词的长度。

输入例子:
> hello world

输出例子:
> 5


解题思路：
> 将输入的字符串利用栈反转，从栈顶开始计算。
> 1. 当遇到栈顶为空格时，停止计算
> 2. 当整个栈为空时，停止计算。

> 优点：避免计算字符串中空格的数量

</br>

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<stack>
#include<string>
#define MAXNUMBER 5000

using std::string;
using std::stack;
using std::cin;
using std::cout;
using std::endl;

stack<char>istack;
int LengthofLast(string str)
{
	int number = 0;
	for (auto c : str)
	{
		istack.push(c);			
	}

	while (istack.top()!=' ')		
	{
		istack.pop();
		number++;
		if (istack.size() == 0)
			break;
	}
	return number;
}

int main()
{
	char ch[MAXNUMBER];
	gets(ch);
	cout<<LengthofLast(ch);
	return 0;
}



```

---

# 4. 随机数的排序以及去重

题目描述

> 明明想在学校中请一些同学一起做一项问卷调查，为了实验的客观性，他先用计算机生成了N个1到1000之间的随机整数（N≤1000），对于其中重复的数字，只保留一个，把其余相同的数去掉，不同的数对应着不同的学生的学号。然后再把这些数从小到大排序，按照排好的顺序去找同学做调查。请你协助明明完成“去重”与“排序”的工作。
 
 ```
Input Param 
     n               输入随机数的个数     
 inputArray      n个随机整数组成的数组 
     
Return Value
     OutputArray    输出处理后的随机整数


注：测试用例保证输入参数的正确性，答题者无需验证。测试用例不止一组。
 ```
 
 

输入描述:
> 输入多行，先输入随机整数的个数，再输入相应个数的整数


输出描述:
> 返回多行，处理后的结果

输入例子:
> 11
10
20
40
32
67
40
20
89
300
400
15

输出例子:
> 10
15
20
32
40
67
89
300
400

代码

 ```cpp
//去重思路：当前数不等于前一个数时，才输出
//排序调用的排序算法
//使用容器vector存储不定长数组

#include<iostream>
#include<vector>
#include<algorithm>

using std::cin;
using std::cout;
using std::sort;
using std::vector;
using std::endl;

int main()
{
	int N;
	while (cin>>N)
	{
		int number;
		vector<int>ivec;
		for (int i = 0; i < N; i++)
		{
			cin >> number;
			ivec.push_back(number);
		}
		sort(ivec.begin(), ivec.end());	
        cout << ivec[0] << endl;
        for (int i = 1; i < N; i++)
        {
            if (ivec[i] != ivec[i - 1])		
                cout << ivec[i] << endl;
        }
	}
	return 0;
}
 ```

---

# 5. 进制转换

题目描述

> 写出一个程序，接受一个十六进制的数值字符串，输出该数值的十进制字符串。（多组同时输入 ）

输入描述:
> 输入一个十六进制的数值字符串。


输出描述:
> 输出该数值的十进制字符串。

输入例子:
> 0xA

输出例子:
> 10 

思路分析：
> 1. 将输入的十六进制字符串压入栈（包括十六进制标识符0x）
> 2. 根据十六进制和十进制的转换公式进行转换（）

> 十六进制数的第0位的权值为16的0次方，第1位的权值为16的1次方，第2位的权值为16的2次方……,计算完毕之后将所有的权值相加即可。
> - [参考来源](http://zhidao.baidu.com/link?url=jK4zj8_PPcQMDvCD92b1seh6u8mkI_z39PJN1KJbnNoEPtqH_1sMHV6FW5NE78iD7JlKSWLW9eOE1mevc-645_)

> 3. 从栈顶往下遍历时，一种方法时遇到`x`即停止，另一种方法是：遍历的长度肯定为字符串长度减去2.
> 4. 由于字符串中的字符为数字或者字母时，其数值均为ASCⅡ码，根据ASC码将其转换成对应的十进制的数字。
> 5. 由于需要多组输入，使用`while(cin>>str)`的形式。
> 6. 使用数学函数幂`pow(double,double)`，其原型为`double pow(double, double)` 其包含在头文件`#include<cmath>`或者`#include<math.h>`中。

程序代码：

```cpp
#include<iostream>
#include<string>
#include<stack>
#include<cmath>

using std::pow;
using std::stack;
using std::string;
using std::cin;
using std::cout;
using std::endl;

void ConvertToDecimal(string str)
{
	stack<char>istack;
	for (auto c : str)
	{
		istack.push(c);		//遍历string字符串，将十六进制的字符串压入栈
	}
	int number;
	int sum = 0;
	for (int i = 0; i < str.size() - 2; i++)	//遍历十六进制的有效部分
	{
		if (istack.top() >= 'A' && istack.top() <= 'F')
		{
			istack.top() = istack.top() - 55;
		}
		if (istack.top() >= 'a' && istack.top() <= 'f')
		{
			istack.top() = istack.top() - 87;
		}
		if (istack.top() >= '0' && istack.top() <= '9')
		{
			istack.top() = istack.top() - 48;
		}
		number = (istack.top()) * pow(16,i);
		sum = sum + number;		
		istack.pop();
	}
	cout << sum << endl;	
}

int main()
{
	string str;
	while(cin >> str)
    {    
		ConvertToDecimal(str);
	}
	return 0;
}
```

---

# 6. 指数因子

> 2016/8/12 16:17:22 

题目描述

> 功能:输入一个正整数，按照从小到大的顺序输出它的所有质数的因子（如180的质数因子为2 2 3 3 5 ）
 
 
输入描述:
> 输入一个long型整数


输出描述:
> 按照从小到大的顺序输出它的所有质数的因子，以空格隔开

输入例子:
> 180

输出例子:
> 2 2 3 3 5


```cpp
#include<iostream>

using std::cin;
using std::cout;
using std::endl;

void PrimeNumber(int ComNumber)
{
	for(int i = 2; i < ComNumber; i++)
	{
		while(ComNumber != i)
		{
			if(ComNumber % i == 0)
			{
				cout << i <<" ";
				ComNumber /= i;
			}
			else 
				break;
		}
	}
	cout << ComNumber << " " <<endl;		//这儿尤其要注意，TM最后一个质数后边也有空格，否则编译不能过。
}

int main()
{
	int ComNumber;
	cin >> ComNumber;	//输入一次，并未说明多次输入
	PrimeNumber(ComNumber);
	return 0;
}
```

# 7. 合并表记录

题目描述

> 数据表记录包含表索引和数值，请对表索引相同的记录进行合并，即将相同索引的数值进行求和运算，输出按照key值升序进行输出。

输入描述:
> 先输入键值对的个数
然后输入成对的index和value值，以空格隔开


输出描述:
> 输出合并后的键值对（多行）

输入例子:
> 4
0 1
0 2
1 2
3 4

输出例子:
> 0 3
1 2
3 4

思路分析
> 一种方法是可以考虑使用关联容器map来分别存储index和value，鉴于该题目中的index和value可以是类型相同的值，故采用vector。
> 采用vector构造的二维数组来存储index和value值，

```cpp
vector<vector<int>> ivec(lines)	//ivec中有line个元素，其中每个元素中又可以包含多个元素

for(int i = 0; i < line; i++)
{
	ivec[i].resize(rows)	//每个元素中包含rows个元素，使ivec成为lines×rows的二维数组
}
```
> 采用先排序，再对相同index的value相加，最后输出合并后的键值对的方法
> 排序时，调用Algorithm中的sort算法，根据二维数组中每行首元素进行排序
> 相同index的value相加后的值存放在相同index的最后
> 输出时，采用条件判断，当该行的首元素和该行后边的首元素不相同时才输出。

代码：
```cpp
#include<iostream>
#include<vector>
#include<algorithm>
#include<iterator>

using std::iterator;
using std::sort;
using std::vector;
using std::cin;
using std::cout;
using std::endl;

void MergeNumber(int number)
{
	vector<vector<int>>ivec(number);
	for (int i = 0; i < number; i++)
		ivec[i].resize(2);	//number行2列的数组
	int index, value;
	for (int i = 0; i < number; i++)
	{
		scanf("%d %d", &index, &value);
		ivec[i][0] = index;			//index储存成每行的首元素，value的值存储成每行的第二个元素
		ivec[i][1] = value;			//将键值以数列的形式存储		
	}

	vector<vector<int>>::iterator it=ivec.begin();
	vector<vector<int>>::iterator it1 = ivec.end();
	
	sort(it,it1);	//将数组按照首元素重新排序

	//将相同index后的value相加
	for (int i = 1; i < number; i++)
	{
		if (ivec[i][0] == ivec[i - 1][0])
		{
			ivec[i][1] = ivec[i][1] + ivec[i - 1][1];
		}
	}

	for (int i = 0; i < number-1; ++i)
	{
		if (ivec[i][0] != ivec[i+1][0])
			cout << ivec[i][0] << " " << ivec[i][1] << endl;
	}
	cout << ivec[number - 1][0] << " " << ivec[number - 1][1] << endl;	//单独输出最后一个元素
	
}

int main()
{
	int number;
	cin >> number;	//键值对的个数
	MergeNumber(number);
	return 0;
}

```

# 8. 提取不重复的整数

题目描述

> 输入一个int型整数，按照从右向左的阅读顺序，返回一个不含重复数字的新的整数。

输入描述:
> 输入一个int型整数


输出描述:
> 按照从右向左的阅读顺序，返回一个不含重复数字的新的整数

输入例子:
> 9876673

输出例子:
> 37689

思路分析：

    输入的整数要得到每一位的数字时，可用取余%10得到末尾的数字。
	这样每一步执行完毕之后将数字/10将前一位数字置于末位。
	将%出来的结果用vector数组来存放。存放顺序就是数字从后往前排列。
	
	至于对数字去重，本来想使用关联数组中的set来存储，
	但是set会对数字自动排序。
	
	参考牛客网友的方法，将每一个%得到的结果先存入vector中，
	再遍历该数字之前的所有数，如果和当前%得到的数字相同，再pop_back()将该重复数字弹出。

```cpp
#include<iostream>
#include<vector>

using std::vector;
using std::cin;
using std::cout;
using std::endl;

void ExtractInt(int Input)
{
	int temp;
	vector<int>ivec;
	while(Input != 0)
	{
		temp = Input % 10;
		ivec.push_back(temp);
		Input /= 10;	
		for(vector<int>::size_type i = 0; i < ivec.size()-1; i++)
		{
			if(temp == ivec[i])
			{
				ivec.pop_back();
				break;
			}
		}
	}

	for(vector<int>::size_type i = 0; i < ivec.size(); i++)
	{
		cout << ivec[i];
	}
}

int main()
{
	int Input;
	cin >> Input;
	ExtractInt(Input);
	return 0;
}

```

# 9. 字符个数统计

题目描述

> 编写一个函数，计算字符串中含有的不同字符的个数。字符在ACSII码范围内(0~127)。不在范围内的不作统计。

输入描述:
> 输入N个字符，字符在ACSII码范围内(0~127)。


输出描述:
> 输出字符的个数。

输入例子:
> abc

输出例子:
> 3

思路分析： 

	输出要求是不同字符的个数,想起关联容器set的自动去重功能，
	
	至于set的自动排列，who care？

	将输入的字符直接压入到set中，

	去重，统计都给你做好。

	STL，我服！

代码：
```cpp
#include<iostream>
#include<set>
#include<string>

using std::cin;
using std::cout;
using std::endl;
using std::set;
using std::string;

void CalcuteNumber(string str)
{
	set<char>iset(str.begin(),str.end());
	cout << iset.size() << endl;
}

int main()
{
	string str;
	cin >> str;
	CalcuteNumber(str);
	return 0;
}

```

# 10.句子逆序

题目描述

> 将一个英文语句以单词为单位逆序排放。例如“I am a boy”，逆序排放后为“boy a am I”
所有单词之间用一个空格隔开，语句中除了英文字母外，不再包含其他字符

输入描述:
> 将一个英文语句以单词为单位逆序排放。


输出描述:
> 得到逆序的句子

输入例子:
> I am a boy

输出例子:
> boy a am I

思路分析：

	从后往前遍历输入的字符串，设定一个指针p指向字符串的最后一个字符

	另外设定一个指针p往前移动，如果p所指向的字符为空格时
	从p+1遍历到q输出每个p所指向的字符
	然后将q移动到p-1的位置，作为下一个输出的终点。

	如此一来，因为第一个单词前没有空格
	所以第一个单词需要单独输出

	由前面指针的移动，第一个单词输出时
	只需要输出位置0到指针q的所有字符即可。

	本例假设所有输出都正确，没有多余的空格或者其他字符。

代码：
```cpp
#include<iostream>
#include<string>

using std::cin;
using std::cout;
using std::endl;
using std::string;

void RevertSentence(string str)
{
	int j = str.size() - 1;	//初始化j的位置
	for (int i = str.size() - 1; i >= 0; i--)
	{
		if (str[i] == ' ')
		{
			for (int i1 = i + 1; i1 <=j; i1++)	//从空格后到j遍历
				cout << str[i1];
			cout << " ";
			j = i - 1;						//对j重新赋值，使其指向空格前一位置
		}
	}
	
	for (int i = 0; i <=j; i++)
	{
		cout << str[i];			//对第一个单词单独输出，使用上述循环中的j
	}
	cout << endl;
}

int main()
{
	char ch[1000];
	gets(ch);
	RevertSentence(ch);
	return 0;
}
```
 
	