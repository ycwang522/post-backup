---
title: 华为OJ系列（二）
date: 2016-08-15 14:48:13
categories: C++
tags:
	- 华为OJ
---

# 1. 字符逆序

题目描述

> 将一个字符串str的内容颠倒过来，并输出。str的长度不超过100个字符。 如：输入“I am a student”，输出“tneduts a ma I”。
 
 
 
输入参数：
> inputString：输入的字符串
 

返回值：
> 输出转换好的逆序字符串
 
 

输入描述:
> 输入一个字符串，可以有空格


输出描述:
> 输出逆序的字符串

输入例子:
> I am a student

输出例子:
> tneduts a ma I

思路分析：

	对string字符串从后往前遍历输出即可

代码：
```cpp
#include<iostream>
#include<string>

using std::string;
using std::cin;
using std::cout;
using std::endl;

void RevertChar(string str)
{
	for (int i = str.size()-1; i >=0; i--)
	{
		cout << str[i];
	}
	cout << endl;
}

int main()
{
	char ch[100];
	gets_s(ch);
	RevertChar(ch);
	return 0;
}
```

<!-- more -->

# 2. 求最小公倍数

题目描述

> 正整数A和正整数B 的最小公倍数是指 能被A和B整除的最小的正整数值，设计一个算法，求输入A和B的最小公倍数。

输入描述:
> 输入两个正整数A和B。


输出描述:
> 输出A和B的最小公倍数。

输入例子:
> 5 
7

输出例子:
> 35

思路分析：
	从输入的两个数中较大的一个数开始遍历，直到两个数的乘积为止
	如果当前数%输入的两个数的结果都为0，那么这个数即为所求
	输出当前数之后，break结束遍历

代码：
```cpp
#include<iostream>

void SomeNumber(int a, int b)
{
	int j = (a>b ? a : b);
	for (int i = (a>b ? a : b); i <= (b*a); i++)
	{
		if (i % a == 0 && i % b == 0)
		{
			std::cout << i;
			break;
		}
	} std::cout << std::endl;
}

int main()
{
	int A, B;
	std::cin >> A >> B;
	SomeNumber(A, B);
	return 0;
}
```

# 3. 字串的连接最长路径查找

题目描述

> 给定n个字符串，请对n个字符串按照字典序排列。 

输入描述:
> 输入第一行为一个正整数n(1≤n≤1000),下面n行为n个字符串(字符串长度≤100),字符串中只含有大小写字母。


输出描述:
> 数据输出n行，输出结果为按照字典序排列的字符串。

输入例子:
> 9
cap
to
cat
card
two
too
up
boat
boot

输出例子:
> boat
boot
cap
card
cat
to
too
two
up


思路分析：

	将输入的字符串存入vector容器中
	利用algorithm中的sort进行自动排序
	再输出

代码：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
#include<string>

using std::string;
using std::cin;
using std::cout;
using std::endl;
using std::sort;
using std::vector;

void CharLength(int length)
{
	vector<string>ivec;
	string str;
	for (int i = 0; i < length; i++)
	{
		cin >> str;
		ivec.push_back(str); 
	}
	sort(ivec.begin(), ivec.end());
	for (int i = 0; i < length; i++)
	{
		cout << ivec[i] << endl;
	}	
}

int main()
{
	int length;
	cin >> length;
	CharLength(length);
	return 0;
}


```

# 4. 求int型数据在内存中存储时1的个数

题目描述

> 输入一个int型数据，计算出该int型数据在内存中存储时1的个数。

输入描述:
> 输入一个整数（int类型）


输出描述:
> 这个数转换成2进制后，输出1的个数

输入例子:
> 5

输出例子:
> 2

思路分析：

	记得以前做选择题的时候，对一个整数x做如下运算
	x & (x-1) 可以一位一位地消灭x的二进制的1
	而 x | x+1 可以一位一位地消灭x的二进制位的0
	而本代码正是利用了这种算法来计算1的个数。

代码：
```cpp
#include<iostream>

int main()
{
	int x;
    std::cin >> x;
    int number = 0;
    while (x)
    {
    	x = x & (x-1)    ;
        number ++;
    }
    std::cout << number << std::endl;
}
```

# 5. 简单密码

题目描述

> 密码是我们生活中非常重要的东东，我们的那么一点不能说的秘密就全靠它了。哇哈哈. 接下来渊子要在密码之上再加一套密码，虽然简单但也安全。
 
> 假设渊子原来一个BBS上的密码为zvbo9441987,为了方便记忆，他通过一种算法把这个密码变换成YUANzhi1987，这个密码是他的名字和出生年份，怎么忘都忘不了，而且可以明目张胆地放在显眼的地方而不被别人知道真正的密码。
 
> 他是这么变换的，大家都知道手机上的字母： 1--1， abc--2, def--3, ghi--4, jkl--5, mno--6, pqrs--7, tuv--8 wxyz--9, 0--0,就这么简单，渊子把密码中出现的小写字母都变成对应的数字，数字和其他的符号都不做变换，
 
> 声明：密码中没有空格，而密码中出现的大写字母则变成小写之后往后移一位，如：X，先变成小写，再往后移一位，不就是y了嘛，简单吧。记住，z往后移是a哦。


输入描述:
> 输入包括多个测试数据。输入是一个明文，密码长度不超过100个字符，输入直到文件结尾


输出描述:
> 输出渊子真正的密文

输入例子:
> YUANzhi1987

输出例子:
> zvbo9441987

代码：

```cpp
#include<iostream>
#include<string>

using std::cin;
using std::cout;
using std::endl;
using std::string;

void SimpleKey(string str)
{
	for (int i = 0; i < str.size(); i++)
	{
		if (str[i] >= '0' && str[i] <= '9')
			cout << str[i];
		if (str[i] >= 'a' && str[i] <= 'c')
			cout << 2;
		if (str[i] >= 'd' && str[i] <= 'f')
			cout << 3;
		if (str[i] >= 'g' && str[i] <= 'i')
			cout << 4;
		if (str[i] >= 'j' && str[i] <= 'l')
			cout << 5;
		if (str[i] >= 'm' && str[i] <= 'o')
			cout << 6;
		if (str[i] >= 'p' && str[i] <= 's')
			cout << 7;
		if (str[i] >= 't' && str[i] <= 'v')
			cout << 8; 
		if (str[i] >= 'w' && str[i] <= 'z')
			cout << 9;

		if (str[i] >= 'A' && str[i] <= 'Z')
		{
			str[i] = str[i] + 32 ;	//将大写转换成小写
			if (str[i] == 'z')	//如果转换后为z，
			{
				str[i] = 'a'-1;
			}
			str[i] = str[i] + 1;	//转换成小写之后往后移动一位
			cout << str[i];
		}
	}
	cout << endl;
}

int main()
{
	char ch[100];
	while (cin >> ch)
	{
		SimpleKey(ch);
	}	
	return 0;
}
```

# 6. 汽水瓶

题目描述

> 有这样一道智力题：“某商店规定：三个空汽水瓶可以换一瓶汽水。小张手上有十个空汽水瓶，她最多可以换多少瓶汽水喝？”答案是5瓶，方法如下：先用9个空瓶子换3瓶汽水，喝掉3瓶满的，喝完以后4个空瓶子，用3个再换一瓶，喝掉这瓶满的，这时候剩2个空瓶子。然后你让老板先借给你一瓶汽水，喝掉这瓶满的，喝完以后用3个空瓶子换一瓶满的还给老板。如果小张手上有n个空汽水瓶，最多可以换多少瓶汽水喝？

输入描述:
> 输入文件最多包含10组测试数据，每个数据占一行，仅包含一个正整数n（1<=n<=100），表示小张手上的空汽水瓶数。n=0表示输入结束，你的程序不应当处理这一行。


输出描述:
> 对于每组测试数据，输出一行，表示最多可以喝的汽水瓶数。如果一瓶也喝不到，输出0。

输入例子:
> 3
10
81
0

输出例子:
> 1
5
40

代码：

```cpp
#include<iostream>
#include<vector>
#include<iterator>

using std::iterator;
using std::vector;
using std::cin;
using std::cout;
using std::endl;

void EmptyCan(vector<int>ivec)
{
	vector<int>::iterator it;
	int temp, temp1, temp2;
	for (vector<int>::iterator it = ivec.begin(); it != ivec.end(); it++)
	{
		temp = *it;
		int sum = 0;
		while (temp > 1)
		{
			if (temp > 2)
			{
				temp1 = temp / 3;//第一次兑换得到temp1瓶汽水
				temp2 = temp % 3;//第一次兑换剩下的空瓶子的数量，
				//此时，已经兑换了temp1瓶汽水，手里还剩temp1 + temp2个瓶子
				sum = sum + temp1;	//3 + 1
				//cout << sum << endl;
				temp = temp1 + temp2;	//更新瓶子的数量
			}
			if (temp == 2)
			{
				sum += 1;
				//cout << sum << endl;
				break;
			}
		}
		cout << sum << endl;
	}
}

int main()
{
	int Input;
	vector<int>ivec;
	while (cin>>Input)
	{
		if (Input == 0)
			break;		//n==0时表示输入结束
		ivec.push_back(Input);
	}
	EmptyCan(ivec);
	return 0;
}
```

# 7. 删除字符串中出现次数最少的字符

题目描述

> 实现删除字符串中出现次数最少的字符，若多个字符出现次数一样，则都删除。输出删除这些单词后的字符串，字符串中其它字符保持原来的顺序。

输入描述:
> 字符串只包含小写英文字母, 不考虑非法输入，输入的字符串长度小于等于20个字节。


输出描述:
> 删除字符串中出现次数最少的字符后的字符串。

输入例子:
> abcdd

输出例子:
> dd

解题思路：

	从前往后两两比较两个字符是否相等，
	内外层遍历序号分别为i，j
	
	如果两个字符相等，则将外层序号对应的字符存入vector中
	外层i的遍历是从第1个字符到倒数第一个字符
	内存j的遍历是从第1个字符到最后一个字符
	
	将ivec[i]存入vector的条件是 i != j && ivec[i] == ivec[j]
	存入之后，break退出当前j
	
	最后对最后一个字符补充判断
	如果前边有一个和最后一个字符相等
	将最后一个字符也存入ivec中，并break

代码：

```cpp
#include<iostream>
#include<string>
#include<vector>

using std::vector;
using std::string;
using std::cin;
using std::cout;
using std::endl;

void DeleteChar(string str)
{
	vector<char>ivec;
	char a;
	for (int i = 0; i < str.size() - 1; i++)
	{
		for (int j = 0 ; j < str.size(); j++)
		{
			a = str[i];
			if (i != j  && a == str[j])
			{
				ivec.push_back(str[i]);
				break;
			}
		}
	}

	for (int i = 0; i < str.size() - 1; i++)	//对最后一个字符补充判断
	{
		if (str[i] == str[str.size() - 1])
		{
			ivec.push_back(str[str.size() - 1]);
			break;
		}
	}

	for (int i = 0; i < ivec.size(); i++)
	{
		cout << ivec[i];
	}	
	cout << endl;
	
}

int main()
{
	char ch[20];
	while (cin >> ch)
	{
		DeleteChar(ch);
	}
	
	return 0;
}

```

# 8. 加强版字符串排序

题目描述

> 编写一个程序，将输入字符串中的字符按如下规则排序。
规则1：英文字母从A到Z排列，不区分大小写。

      如，输入：Type 输出：epTy

> 规则2：同一个英文字母的大小写同时存在时，按照输入顺序排列。

    如，输入：BabA 输出：aABb

> 规则3：非英文字母的其它字符保持原来的位置。

    如，输入：By?e 输出：Be?y

> 样例：

    输入：

	 A Famous Saying: Much Ado About Nothing(2012/8).

    输出：

	 A aaAAbc dFgghh: iimM nNn oooos Sttuuuy (2012/8).


输入例子:
> A Famous Saying: Much Ado About Nothing (2012/8).

输出例子:
> A aaAAbc dFgghh: iimM nNn oooos Sttuuuy (2012/8).

解题思路：

	外层i循环为[0,26),内层j循环为[0，str.size())
	判断条件为如果j对应的字符减去字符a或者A等于外层循环i的值
	则将当前字符存入容器中

	字母提取总结：根据内外层遍历，按照字母表顺序提取出字符
	存入vector中

	再遍历原来输入的字符串，如果当前字符为字母，则在vector中按顺序取出字母替换掉原来字符串中的字母

代码：

```cpp
#include<iostream>
#include<vector>
#include<string>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	string str;
	vector<char>ivec;
	while (getline(cin, str))
	{
		int length = str.size();
		ivec.clear();
		for (int i = 0; i<26; i++)
		{
			for (int j = 0; j<length; j++)
			{
				if (str[j] - 'a' == i || str[j] - 'A' == i)
				{
					ivec.push_back(str[j]);
				}
			}
		}

		for (int i = 0, k = 0; (i<length) && (k < ivec.size()); i++)
		{
			if ((str[i] >= 'a' && str[i] <= 'z') || (str[i] >= 'A' && str[i] <= 'Z'))
			{
				str[i] = ivec[k++];
				//k++;
			}
		}
		cout << str << endl;;
	}
	return 0;
}
```
# 9. 字符串加密

题目描述

> 1、对输入的字符串进行加解密，并输出。

> 2加密方法为：
当内容是英文字母时则用该英文字母的后一个字母替换，同时字母变换大小写,如字母a时则替换为B；字母Z时则替换为a；
当内容是数字时则把该数字加1，如0替换1，1替换2，9替换0；
其他字符不做变化。

> 3、解密方法为加密的逆过程。
 
	接口描述：
	    实现接口，每个接口实现1个基本操作：
	void Encrypt (char aucPassword[], char aucResult[])：在该函数中实现字符串加密并输出
	说明：
	1、字符串以\0结尾。
	2、字符串最长100个字符。
	 
	int unEncrypt (char result[], char password[])：在该函数中实现字符串解密并输出
说明：
> 1、字符串以\0结尾。

> 2、字符串最长100个字符。
 
 
 

输入描述:
> 
输入一串要加密的密码
输入一串加过密的密码


输出描述:
> 
输出加密后的字符
输出解密后的字符

输入例子:
> abcdefg
BCDEFGH

输出例子:
> BCDEFGH
abcdefg

思路分析：
	按部就班，一步一步来

代码：

```cpp
#include<iostream>
#include<string>

using std::cin;
using std::cout;
using std::endl;
using std::string;
using std::getline;

void AddKey(string str)
{
	for (string::size_type i = 0; i < str.size(); i++)
	{
		if (str[i] >= 'a' && str[i] <= 'z')
		{
			if (str[i] == 'z')
			{
				str[i] = 'A';
				continue;
			}
			else
			{
				str[i] = str[i] - 32 + 1;
				continue;
			}
		}
		if (str[i] >= 'A' && str[i] <= 'Z')
		{
			if (str[i] == 'Z')
			{
				str[i] = 'a'; continue;
			}
			else
			{
				str[i] = str[i] + 32 + 1; continue;
			}
		}
		if (str[i] >= '0' && str[i] <= '9')
		{
			if (str[i] == '9')
			{
				str[i] = '0'; continue;
			}
			else
			{
				str[i] = str[i] + 1; continue;
			}
		}
		
	}
	cout << str << endl;
}	

void SubKey(string str)
{
	for (string::size_type i = 0; i < str.size(); i++)
	{
		if (str[i] >= 'a' && str[i] <= 'z')
		{
			if (str[i] == 'a')
			{
				str[i] = 'Z';
				continue;
			}
			else
			{
				str[i] = str[i] - 32 - 1;
				continue;
			}
		}
		if (str[i] >= 'A' && str[i] <= 'Z')
		{
			if (str[i] == 'A')
			{
				str[i] = 'z'; continue;
			}
			else
			{
				str[i] = str[i] + 32 - 1; continue;
			}
		}
		if (str[i] >= '0' && str[i] <= '9')
		{
			if (str[i] == '0')
				str[i] = '9';
			else
				str[i] = str[i] - 1;
		}
	}
	cout << str << endl;
}

int main()
{
	string str;
	string str1;
	while (cin >> str, cin >> str1)
	{
		AddKey(str);
		SubKey(str1);
	}
	return 0;
}
```

# 10. 字符串合并处理（待调试）

题目描述

> 按照指定规则对输入的字符串进行处理。
> 
> 详细描述：
将输入的两个字符串合并。
对合并后的字符串进行排序，要求为：下标为奇数的字符和下标为偶数的字符分别从小到大排序。这里的下标意思是字符在字符串中的位置。
对排训后的字符串进行操作，如果字符为‘0’——‘9’或者‘A’——‘F’或者‘a’——‘f’，则对他们所代表的16进制的数进行BIT倒序的操作，并转换为相应的大写字符。如字符为‘4’，为0100b，则翻转后为0010b，也就是2。转换后的字符为‘2’； 如字符为‘7’，为0111b，则翻转后为1110b，也就是e。转换后的字符为大写‘E’。
 
举例：

> 输入str1为"dec"，str2为"fab"，合并为“decfab”，分别对“dca”和“efb”进行排序，排序后为“abcedf”，转换后为“5D37BF”

输入描述:
> 输入两个字符串


输出描述:
> 输出转化后的结果

输入例子:
> dec fab

输出例子:
> 5D37BF

代码示例：

```cpp
#include<string>
#include<iostream>
#include<vector>
#include<algorithm>

using std::sort;
using std::vector;
using std::cin;
using std::cout;
using std::endl;
using std::string;

void Output(int sum)	//根据反转后二进制的加权和输出对应的字符
{

	if (sum >= 10)
	{
		for (int i = 0; i < 6; i++)
		{
			char ch[7] = "ABCDEF";
			if (sum - 10 == i)
				cout << ch[i];
		}
	}
	if (sum < 10 && sum >= 0)
		cout << sum;
}

int TransToBi(int number)	//将字母对应的整数转换成二进制数以后反转再求值
{
	int temp;
	vector<int>ivec4;
	while (number)
	{
		temp = number % 2;
		ivec4.push_back(temp);	//将数逆序压入容器
		number /= 2;		//111
	}
	int len = ivec4.size();
	if (ivec4.size() < 4)	//如果ivec0中的数字不够4个
	{
		for (int i = 0; i < 4 - len; i++)
			ivec4.push_back(0);	//1110
	}
	int sum = 0;
	for (int i = 0; i < 4; i++)
	{
		sum = sum + ivec4[i] * pow(2, 4 - i - 1);
	}
	return sum;
}


int CharToInt(char ch)	//传入的字符转换成数字
{
	/*
	if (ch == 'a' || ch == 'A')
		return 10;
	else if (ch == 'b' || ch == 'B')
		return 11;
	else if (ch == 'c' || ch == 'C')
		return 12; 
	else if (ch == 'd' || ch == 'D')
		return 13; 
	else if (ch == 'e' || ch == 'E')
		return 14; 
	else //if (ch == 'f' || ch == 'F')
		return 15;
	*/
	for (int i = 0; i < 6; i++)
	{
		if (ch - 'a' == i || ch - 'A' == i)
			return (i+10);
	}
}


void Tranfer(char ch)	//转换
{
	
	if ((ch >= '0' && ch <= '9'))
	{
		int i = (ch - '0');	//将字符转换为数字求其二进制数
		int sum = TransToBi(i);
		Output(sum);
		
	}
	if (ch >= 'a' && ch <= 'f')
	{
		int number = CharToInt(ch);	//得到对应的整数
		int sum =TransToBi(number);		//这儿得到的是反转以后的sum
		Output(sum);
	}
	if (ch >= 'A' && ch <= 'F')
	{
		int number = CharToInt(ch);	//得到对应的整数
		int sum = TransToBi(number);		//这儿得到的是反转以后的sum
		Output(sum);
	}
}

void MergeChar(string str)
{
	vector<char>ivec1;
	
	for (string::size_type i = 0; i < str.size(); i += 2)
		ivec1.push_back(str[i]);	//将第奇数个字符压入ivec中
	sort(ivec1.begin(), ivec1.end());

	vector<char>ivec2;
	for (string::size_type i = 1; i < str.size(); i += 2)
		ivec2.push_back(str[i]);	//将第偶数个字符压入ivec1中
	sort(ivec2.begin(), ivec2.end());
	
	vector<char>ivec3;
	for (string::size_type i = 0; i < str.size(); i++)
	{
		if (i % 2 == 0)
			ivec3.push_back(ivec1[i / 2]);
		if (i % 2 == 1)
			ivec3.push_back(ivec2[i / 2]);
	}

	//以上完成字符串的局部排序
	for (vector<char>::size_type i = 0; i < ivec3.size(); i++)
	{
		Tranfer(ivec3[i]);
	}

}

int main()
{
	string str1;
	string str2;
	cin >> str1;
	cin >> str2;
	string str = str1 + str2;
	MergeChar(str);
	return 0;
}
```

	

