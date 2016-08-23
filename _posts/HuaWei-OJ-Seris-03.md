---
title: 华为OJ系列（三）
date: 2016-08-18 15:15:48
categories: C++
tags:
	- 华为OJ
---

# 1. 【中级】单词倒排

题目描述

> 对字符串中的所有单词进行倒排。
说明：
1、每个单词是以26个大写或小写英文字母构成；
2、非构成单词的字符均视为单词间隔符；
3、要求倒排后的单词间隔符以一个空格表示；如果原字符串中相邻单词间有多个间隔符时，倒排转换后也只允许出现一个空格间隔符；
4、每个单词最长20个字母；

输入描述:
> 输入一行以空格来分隔的句子


输出描述:
> 输出句子的逆序

输入例子:
> I am a student

输出例子:
> student a am I

思路分析：

	使用getline(cin,str)输入字符串
	
	判断字符串中的每个字符，将非字母转换成空格字符
	包括空格也进行转换

	设定一个游标k = 0，while(str[k] == ' ') { ++ k; }
	使得k指向开头不为空的第一个字符

	同理，设定一个游标 j = str.size() -1指向字符串的最后一个字符
	while(str[j] == ' ') { --j	;} 
	使得j指向结尾最后一个不为空字符的字符

	当在j 和k之间遍历时，考虑中间为多个空格的情况

	对开头的第一个单词，单独进行输出


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
	for (string::size_type i = 0; i < str.size(); i++)	//将非字母转换为空格
	{
		if ((str[i]<'a' && str[i]>'Z') || str[i] > 'z' || str[i] < 'A')
			str[i] = ' ';
	}
	int j = str.size() - 1;
	while (str[j] == ' ')
	{
		j--;		//使得遍历从后边不为空格的第一个字母开始
	}

	int k = 0;
	while (str[k] == ' ')
	{
		++k;	//使得遍历从开头不为空格的第一个字母开始
	}
	for (int i = j; i >= k; i--)
	{
		if (str[i] == ' ')	
		{
			for (int i1 = i + 1; i1 <= j; i1++)
				cout << str[i1];
			cout << " ";			
			j = i - 1;
			while (str[j] == ' ')	//去掉中间多余的空格
			{
				--i; --j;
			}
			
		}		
	}
	for (int i = k; i <= j; i++)
	{
		cout << str[i];
	}
	cout << endl;
}

int main()
{
	string str;
	getline(cin, str);	
	RevertSentence(str);
	return 0;
}

```

<!-- more -->

# 2. 字符串运用-密码截取

题目描述

> Catcher是MCA国的情报员，他工作时发现敌国会用一些对称的密码进行通信，比如像这些ABBA，ABA，A，123321，但是他们有时会在开始或结束时加入一些无关的字符以防止别国破解。比如进行下列变化 ABBA->12ABBA,ABA->ABAKK,123321->51233214　。因为截获的串太长了，而且存在多种可能的情况（abaaab可看作是aba,或baaab的加密形式），Cathcer的工作量实在是太大了，他只能向电脑高手求助，你能帮Catcher找出最长的有效密码串吗？


输入描述:
> 输入一个字符串


输出描述:
> 返回有效密码串的最大长度

输入例子:
> ABBA

输出例子:
> 4

解题思路：

	设定两个指针，一个指针i从字符串开始处向后遍历
	另一个指针j从字符串末尾开始向前遍历，j遍历的判断条件是j>i
	
	如果i和j对应的字符相同
	则判断其之间的字符是否相同
	
	其中有两种情况
	一种是奇数个对称（ABCBA），另一个是偶数个对称（ABBA）
	两种情况都要考虑

	这样会将所有的对称的字符的个数都会计算出来
	并且将其存储到vector中
	所有的字符遍历完毕之后
	对vector中的int排序
	输出vector中最后一个数，即为密码串的最大长度

代码：

```cpp
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>

using std::sort;

using std::vector;
using std::cin;
using std::cout;
using std::endl;
using std::string;


void CalculateLength(string str)
{
	int last = str.size() -1 ;
	vector<int>ivec;
	for (string::size_type i = 0; i < last -1; i++)
	{
		//string::size_type k =i+1;
		for (string::size_type j = last; j > i; j--)
		{
			if (str[i] == str[j])
			{
				int k = 1;
				if (i + 1 == j)	//内层相邻
				{
					ivec.push_back(2 * k);
					break;
				}
				if (i + 2 == j)
				{
					ivec.push_back(2 * k + 1);
					break;
				}
				while (str[i + k] == str[j - k])
				{
					if (i + k + 1 == j - k)	//相邻对称 总和=2（k+1）
					{
						ivec.push_back(2 * (k + 1));
						break;
					}
					if (i + k + 2 == j - k)	//不相邻对称 总和 = 2（k+1）+1
					{
						ivec.push_back(2 * (k + 1) + 1);
						break;
					}
					++k;
				}
				
			}
		}
	}
	vector<int>::iterator it = ivec.end();
	sort(ivec.begin(), it);
	cout << *(it-1) << endl;
}

int main()
{
	string str;
	while (getline(cin, str))
	{
		CalculateLength(str);
	}
	return 0;
}
```

# 3. 整数与IP地址间的转换

题目描述

> 原理：ip地址的每段可以看成是一个0-255的整数，把每段拆分成一个二进制形式组合起来，然后把这个二进制数转变成
一个长整数。

举例：
> 一个ip地址为10.0.3.193
每段数字             相对应的二进制数
10                   00001010
0                    00000000
3                    00000011
193                  11000001
组合起来即为：00001010 00000000 00000011 11000001,转换为10进制数就是：167773121，即该IP地址转换后的数字就是它了。
 
的每段可以看成是一个0-255的整数，需要对IP地址进行校验
 
 
 

输入描述:
输入 
> 1 输入IP地址
2 输入10进制型的IP地址


输出描述:
输出
> 1 输出转换成10进制的IP地址
2 输出转换后的IP地址

输入例子:
> 10.0.3.193
167969729

输出例子:
> 167773121
10.3.3.193


代码：
```cpp
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>
#include<stack>
#include<cmath>

using std::stack;
using std::sort;
using std::vector;
using std::cin;
using std::cout;
using std::endl;
using std::string;


int CharToInt(char ch)	//数字字符转换十进制
{
	return ch - '0';
}

vector<long>ivecsum;	//设为全局变量
void IntToBi(long sum)	//将单个的十进制的IP转换为二进制
{
	//vector<long>ivecsum;
	stack<long>istack;
	while (sum)
	{
		long temp = sum % 2;
		sum /= 2;
		istack.push(temp);
	}
	if (istack.size() < 8)
	{
		for (int i = istack.size(); i < 8; i++)
			istack.push(0);
	}
	while (!istack.empty())
	{
		ivecsum.push_back(istack.top());
		istack.pop();
	}
}

void TransToInt(string str)	//将十进制IP转换成一个长整型
{
	vector<long>ivec;
	int j = 0;
	ivecsum.clear();		//第二次调用时，清空全局的容器
	for (string::size_type i = 0; i < str.size(); i++)
	{
		long sum = 0;
		if (str[i] == '.')
		{
			for (int k = i-1; k >= j; k--)
				sum = sum + CharToInt(str[k])*pow(10, i-k-1);
			j = i + 1;
			ivec.push_back(sum);
		}
	}

	//单独处理最后一个
	long sum1 = 0;
	int length = str.size();
	for (int ii = length - 1; ii >= j; ii--)
		sum1 = sum1 + CharToInt(str[ii]) *pow(10, length - 1 - ii);
	ivec.push_back(sum1);
	for (int i = 0; i < 4; i++)
	{
		if (ivec[i] <= 255 && ivec[i] >= 0)
			IntToBi(ivec[i]);
		else
			return;
	}

	long sumsum = 0;
	for (int i = 31; i >= 0; i--)
	{
		sumsum = sumsum + ivecsum[i] * pow(2, 32 - i - 1);
	}
	cout << sumsum << endl;
}

void TransToBi(long number)	//将长整型转换成带分隔符的类型
{
	vector<int>ivec1;
	stack<int>istack1;
	while (number)
	{
		int tempp = number % 2;
		istack1.push(tempp);
		number /= 2;
	}
	if (istack1.size() < 32)
	{
		for (int i = istack1.size(); i < 32; i++)
		{
			istack1.push(0);
		}
	}
	while (!istack1.empty())
	{
		ivec1.push_back(istack1.top());
		istack1.pop();
	}
	int num = 0;
	int j = 0;	//结尾位置
	vector<int>IvecResult;
	for (int i = 0; i < 32; i++)
	{		
		++num;	
		int sum = 0;
		if (num == 8)	
		{
			j = i+1;			
			for (int kk = j - 8,k1 = 0; kk < j, k1 < 8; kk++, k1++)
			{
				sum = sum + ivec1[kk] * pow(2, 7 - k1);
			}
			num = 0;
			IvecResult.push_back(sum);
		}
	}

	for (int i = 0; i < 3; i++)
	{
		cout << IvecResult[i] << ".";
	}
	cout << IvecResult[3] << endl;
}

int main()
{
	string str1;
	long number;
	while (cin >> str1 >> number)
	{
		TransToInt(str1);
		TransToBi(number);
	}
	return 0;
}
```

# 4. 及格线

描述：
> 10个学生考完期末考试评卷完成后，A老师需要划出及格线，要求如下：
(1) 及格线是10的倍数；
(2) 保证至少有60%的学生及格；
(3) 如果所有的学生都高于60分，则及格线为60分

输入：
> 输入10个整数，取值0~100

输出：
> 输出及格线，10的倍数

输入样例：
> 61 51 49 3020 10 70 80 90 99

输出样例：
> 50

思路分析：

	输入十个数之后，对数字存储到vector中进行排序
	要保证60%的学生及格，即6个学生
	所以判断排序之后第6个学生的成绩

代码：
```cpp
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

void PassLine(int a)
{
	int passline=0;
	vector<int>ivec;
	for (int i = 0; i < a; i++)
	{
		int temp;
		cin >> temp;
		ivec.push_back(temp);	//将输入的十个数存入ivec中
	}
	sort(ivec.begin(), ivec.end());
	int temp1 = ivec[4];
	if (temp1 >= 60)
		passline = 60;
	else
		passline = temp1 - temp1 % 10;
	cout << passline;
}
int main()
{
	int a = 10;
	PassLine(a);
	return 0;
}
```

# 5. 蛇形矩阵

题目描述

题目说明
> 蛇形矩阵是由1开始的自然数依次排列成的一个矩阵上三角形。
 
 
 
样例输入
5
样例输出
1 3 6 10 15
2 5 9 14
4 8 13
7 12
11

输入描述:
> 输入正整数N（N不大于100）


输出描述:
> 输出一个N行的蛇形矩阵。

输入例子:
> 4

输出例子:
> 1 3 6 10
2 5 9
4 8
7

思路分析：

	新建一N行N列的二维数组，用来存放蛇形矩阵
	
	根据蛇形矩阵的规律，先遍历将第1列的值计算出来

	设外层遍历索引为i，内层遍历索引为j
	则i为[0,N)，j的范围为[1，N)
	根据蛇形矩阵的规律，有如下关系式：
	ivec[i][j] = ivec[i][j - 1] + i + 1 + j;
	计算出矩阵中的各个数
	此时计算出来的数并不是蛇形矩阵，而是一个N行N列的矩阵

	蛇形矩阵在输出时体现，
	将N的值赋值给M	
	外层循环i的值为0到M
	内存循环每执行完一个j时，N执行--操作

	
	另外注意输出的空格，每一行的数字之间是用空格来间隔的
	但是每一行的第一个数字之前和最后一个数字之后是没有空格的。

代码：
```cpp
#include<iostream>
#include<vector>

using std::vector;
using std::cin;
using std::cout;
using std::endl;


void Matrix(int N)
{
	vector<vector<int>>ivec(N);
	for (int i = 0; i < N; i++)
	{
		ivec[i].resize(N);	//建立一个N*N的二维数组；实际的数组维度
	}
	
    ivec[0][0] = 1;	
    for (int j = 0; j < 1; j++)
	{
		for (int i = 1; i < N; i++)
		{			
			ivec[i][0] = ivec[i-1][0] + i;
		}		
	}
    
	for (int i = 0; i < N; i++)
	{
		for (int j = 1; j < N; j++)
		{
			ivec[i][j] = ivec[i][j - 1] + i + 1 + j;
		}

	}

	int M = N;
	for (int i = 0; i < M; i++)
	{
		cout << ivec[i][0];
		for (int j = 1; j < N; j++)
		{
			cout << " "<<ivec[i][j];	//空格是在数字之间，最后一个数字之后不能有空格
		}
		cout << endl;
		--N;
	}
	
}

int main()
{
	int Number;
	while (cin>>Number )
	{
		if (Number>=1 && Number	<=100)
		{
			Matrix(Number);
		}
	}
	return 0;
}


```

# 6. 字符串加密

题目描述

> 有一种技巧可以对数据进行加密，它使用一个单词作为它的密匙。下面是它的工作原理：首先，选择一个单词作为密匙，如TRAILBLAZERS。如果单词中包含有重复的字母，只保留第1个，其余几个丢弃。现在，修改过的那个单词死于字母表的下面，如下所示：
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
T R A I L B Z E S C D F G H J K M N O P Q U V W X Y
上面其他用字母表中剩余的字母填充完整。在对信息进行加密时，信息中的每个字母被固定于顶上那行，并用下面那行的对应字母一一取代原文的字母(字母字符的大小写状态应该保留)。因此，使用这个密匙，Attack AT DAWN(黎明时攻击)就会被加密为Tpptad TP ITVH。


 
 

输入描述:
> 先输入key和要加密的字符串


输出描述:
> 返回加密后的字符串

输入例子:
> nihao
ni

输出例子:
> le

思路分析：
	将输入转换成大写运行计算
	输出的时候再将对应的小写转换成大写

代码
```cpp
#include<iostream>
#include<vector>
#include<string>
#include<map>

using std::map;
using std::string;
using std::vector;
using std::cin;
using std::cout;
using std::endl;
using std::pair;

void AddKey(string Key,string WaitToKey)
{
	//将Key全部转换成大写
	for (string::size_type i = 0; i < Key.size(); i++)
	{
		if (Key[i] <= 'z' && Key[i] >= 'a')
			Key[i] = Key[i] - 32;
	}

	//对Key去重
	int length = Key.size();
	for (string::size_type i = 0; i < length-1; i++)
	{
		for (string::size_type j = length - 1; j > i; j--)
		{
			if (Key[i] == Key[j])
				Key[j] = '0';
		}
	}
	vector<char>ivec;
	for (string::size_type i = 0; i < Key.size(); i++)
	{
		if (Key[i] != '0')
			ivec.push_back(Key[i]);
	}

	//加上余下的字母
	for (string::size_type i = 0; i < Key.size(); i++)
	{
		if (Key[i] <= 'z' && Key[i] >= 'a')
		{
			toupper(Key[i]);
		}
	}
	for (int i = 0; i < 26; i++)
	{
		ivec.push_back('A' + i);
	}
	for (int i = 0; i < ivec.size() - 1; i++)
	{
		for (int j = ivec.size() - 1; j > i; j--)
		{
			if (ivec[i] == ivec[j])
				ivec[j] = '0';
		}
	}
	vector<char>ivec1;
	for (int i = 0; i < ivec.size(); i++)
	{
		if (ivec[i] != '0')
			ivec1.push_back(ivec[i]);
	}
	//加上余下的字母——完毕

	//构造关于key和明文的map
	map<char,char>imap;
	for (int i = 0; i < 26; i++)
	{
		imap.insert(pair<char, char>(ivec1[i], 'A' + i));
	}
	string temp;
	for (string::size_type i = 0; i < WaitToKey.size(); i++)
	{
		//如果明文是小写字符，将其转换成大写，输出时再转换成对应的小写
		if (WaitToKey[i] <= 'z' && WaitToKey[i] >= 'a')
		{
			WaitToKey[i] = WaitToKey[i] - 32;	//将输入转换成大写
			
			for (map<char, char>::iterator it = imap.begin(); it != imap.end();it++)
			{
				if (WaitToKey[i] == it->second)
					printf("%c",(it->first) +32);
			}
			continue;
		}
		if (WaitToKey[i] <= 'Z' && WaitToKey[i] >= 'A')
		{
			for (map<char, char>::iterator it = imap.begin(); it != imap.end(); it++)
			{
				if (WaitToKey[i] == it->second)
					printf("%c", it->first);
			}
		}
		
	}
	cout << endl; 

}

int main()
{
	string str1, str2;
	while (cin >> str1 >> str2)
	{
		AddKey(str1, str2);
	}
	return 0;
}



```
	


	


	
