---
title: 华为OJ系列（四）
date: 2016-09-06 10:30:34
categories: C++
tags:
	- 华为OJ
---

# 1. 名字的漂亮度

题目描述

> 给出一个名字，该名字有26个字符串组成，定义这个字符串的“漂亮度”是其所有字母“漂亮度”的总和。
每个字母都有一个“漂亮度”，范围在1到26之间。没有任何两个字母拥有相同的“漂亮度”。字母忽略大小写。
给出多个名字，计算每个名字最大可能的“漂亮度”。

输入描述:
> 整数N，后续N个名字


输出描述:
> 每个名称可能的最大漂亮程度

输入例子:
> 2
zhangsan
> lisi

输出例子:
> 192
101

<!-- more -->

思路分析：
	
	将漂亮度计算的函数单独抽象出来计算，输入每个名字时调用即可。
	
	具体计算时，考虑使用map先统计每个字母出现的次数，imap[char]++.
	由于使用这种方法存储时，key是出现的字母，而value是每个字母出现的次数。
	然后再使用multimap将出现的字母和对应的次数交换位置
	即key是次数，value是对应的字母，由于自动会对key排序
	从后往前遍历，用对应的次数乘以相应的权值，将结果求和

```cpp
#include<iostream>
#include<string>
#include<map>
#include<vector>

using namespace std;

int func(string str)
{
	map<char,int>imap;
	for (string::size_type i = 0; i < str.size(); i++)
	{
		if(str[i]<='Z' && str[i]>='A')	//大写统一转换为小写
            	str[i]=str[i]+32;
		imap[str[i]]++;	//统计字符出现的次数存储为value
	}
	multimap<int,char>map_sort1;	//该map存储key为次数，value为字母
	map<char, int>::iterator it = imap.begin();
	for (int i = 0; i < imap.size(); i++)
	{
		map_sort1.insert(pair<int, char>(it->second, it->first));
		++it;
	}	//数字置于first	

	int sum = 0;
	int index = 0;
	multimap<int, char>::reverse_iterator it1 = map_sort1.rbegin();
	for (int i = map_sort1.size()-1; i >=0; i--)	//由于排序的性质是从小往大的顺序排列
	{			//遍历从map的后边开始
		sum = sum + (it1->first)*(26 - index);	//初始时，索引值是26，
		++index;	//每循环一次索引值减1
		++it1;
	}
	return sum;
}

void BeautyOfName(int N)
{
	vector<string>ivec;		
	for (int i = 0; i<N; i++)
	{
		string str;
		cin >> str;
		ivec.push_back(str);
	}
	for (int i = 0; i < N; i++)
	{
		cout << func(ivec[i]) << endl;
	}
}

int main()
{
	int a;
	while (cin >> a)
	{
		BeautyOfName(a);
	}
	return 0;
}
```

# 2. 按字节截取字符串

题目描述

> 编写一个截取字符串的函数，输入为一个字符串和字节数，输出为按字节截取的字符串。但是要保证汉字不被截半个，如"我ABC"4，应该截为"我AB"，输入"我ABC汉DEF"6，应该输出为"我ABC"而不是"我ABC+汉的半个"。 
 

输入描述:
> 输入待截取的字符串及长度


输出描述:
> 截取后的字符串

输入例子:
> 我ABC汉DEF
6

输出例子:
> 我ABC


```cpp
#include<string>
#include<iostream>
#include<vector>

using namespace std;

void func(string str,int number)
{
	if(str.empty() || number == 0)
        return;
    int N = 0;		//存储判断得到的字节数
	int M = 0;	//存储索引值
	for (int i = 0; i < number; i++)
	{
		if ((str[i] <= 'Z'&&str[i] >= 'A') || (str[i] <= 'z'&&str[i] >= 'a'))
		{
			++N;
			if (N == number)
				M = i;	//记录编号
		}
		else
		{
			N ++;
			if (N == number || N == number + 1)
				M = i;
		}

	}
	for (int i = 0; i <= M; i++)
	{
		cout << str[i];
	}
	cout << endl;
}

int main()
{
	string str;
	int number;
	while (cin>>str>>number)
	{
		func(str, number);
	}
	return 0;
}
```

# 3. 与7相关的数字

题目描述

> 输出7有关数字的个数，包括7的倍数，还有包含7的数字（如17，27，37...70，71，72，73...）的个数

输入描述:
> 一个正整数N。(N不大于30000)


输出描述:
> 不大于N的与7有关的数字个数，例如输入20，与7有关的数字包括7,14,17.

输入例子:
> 20

输出例子:
> 3

思路分析
	
	遍历从1到给定的数，如果是7的倍数，则个数+1
	如果不是7的倍数，将该数字转换为对应的字符串查找是否包含7


```cpp
#include<algorithm>
#include<iostream>
#include<vector>

using namespace std;

void func(int N)
{
	if (N <= 0)
		return;
	int number = 0;
	
	for (int i = 1; i <= N; i++)
	{
		if (i % 7 == 0)	//统计7的倍数
		{
			++number;
		}
		else	//不含7的倍数
		{
			vector<int>ivec;
			ivec.clear();
			int temp_1 = i;
			while (temp_1)
			{
				int temp;
				temp = temp_1 % 10;
				ivec.push_back(temp);
				temp_1 /= 10;
			}
			if (find(ivec.begin(), ivec.end(), 7) != ivec.end())
				++number;
		}
	}
	cout << number << endl;
}

int main()
{
	int N;
	while (cin >> N)
	{
		func(N);
	}
	return 0;
}
```
# 4. <我的华为机试题目>字符串输出

题目描述

> 输入一串整数，对整数进行排序，然后将排序后的数输出，输出时有如下规则：如果排序后的数是相邻数，只输出相邻数中最小和最大的数，其余数字正常输出。

输入描述:
> 以逗号分开的一串数字，比如
> 7,1,3,2,5,8,6,9


输出描述:
> 输出排序后的数，相邻数只输出最小和最大数.输出数之间以空格分隔。

输入例子:
> 7,1,3,2,5,8,6,9

输出例子：

> 1 3 5 9

思路分析：

	由于输入的数字不确定个数，而且之间是以逗号分隔开，因此，考虑以字符串的形式读入之后再分割化为整数进行排序输出。
	其中使用atoi函数将字符串转换为对应的整数，而其中存储字符串的char ch[]在每次使用完毕要进行初始化操作，这儿将其每个字符初始化为字符串结尾符'\0'。
	进行相邻数字的判定时，先输出vector中的第一个数字，然后从第二个数字遍历到倒数第二个数字，如果当前数减1等于前一个数 && 当前数加1等于后一个数时，进行一个空操作，其else（非条件）时，输出当前对应的数字。
	最后输出最后一个数字。
	
	其中，为了防止输入的数字中有重复的数字，将所有数字压入set进行了一次去重操作。

示例代码：

```cpp
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>
#include<set>

using namespace std;

void func(string str)
{
	char ch[100];
	memset(ch, '\0', 100);
	int k = 0;
	vector<int>ivec;
	for (int i = 0; i < str.size(); i++)
	{
		if (str[i] != ',')
			ch[k++] = str[i];
		if (str[i] == ',')
		{
			int temp = atoi(ch);
			ivec.push_back(temp);
			memset(ch, '\0', 100);
			k = 0;
		}
	}
	char ch1[100];
	memset(ch1, '\0', 100);
	int kk = 0;
	for (int i = str.size() - 1; i >= 0; i--)
	{
		if (str[i] == ',')
		{
			for (int jj = i + 1; jj < str.size(); jj++)
			{
				ch1[kk++] = str[jj];
				int temp1 = atoi(ch1);
				ivec.push_back(temp1);
			}
			break;
		}
	}
	set<int>iset(ivec.begin(),ivec.end());	//踢除重复数
	vector<int>vec(iset.begin(),iset.end());
	cout << vec[0] << " ";
	for (int i = 1; i < vec.size()-1; i++)
	{
		if ((vec[i] - 1 == vec[i - 1]) && (vec[i] + 1 == vec[i + 1]))
			;
		else
		{
			cout << vec[i] << " ";
		}
	}
	cout << vec[vec.size() - 1] << endl;
}

int main()
{
	string str;
	while (cin >> str)
	{
		func(str);
	}
	return 0;
}
```
# 5. 连续子数组的最大和

题目描述

> HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。你会不会被他忽悠住？


思路分析：
	
	1. 考虑输入的合法性
	2. 判断是否全是负数的情况

代码：
```cpp
class Solution 
{
public:
    int FindGreatestSumOfSubArray(vector<int> array) 
    {
    	if(array.empty())
            return 0;
        vector<int>::iterator it = max_element(array.begin(),array.end());      
        int temp = (*it);
        if(temp < 0)
        {
            return temp;
        }
        int sum = 0;
        int max = 0;
        for(vector<int>::size_type i =0;i<array.size();i++)
        {
        	sum = sum + array[i];
            if(sum<0)
            {
                sum = 0;
            }
            else
            {
                if(sum > max)
                    max = sum;
            }
                
        }
        return max;
    }
};
```

