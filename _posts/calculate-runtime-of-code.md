---
title: 计算代码运行时间
date: 2016-05-31 17:19:40
categories: C++
tags:
	- C++
	- C
---

# 计算代码的运行时间

## 包含头文件

```cpp
#include<time.h>
```

## 在程序执行的起始点，插入代码

```cpp
clock_t start,finish;
start=clock();
```

## 在程序执行的终点，插入代码：

```cpp
finish=clock();
printf("共耗时%.3lf秒"，((double)finish - start)/1000);
```

<!-- more -->

### 例如，有如下代码：

```cpp
void swap(int a[], int i, int j)
{
	int temp = a[i];
	a[i] = a[j];
	a[j] = temp;
}
```

```cpp
//Sort1
void BubbleSort1(int a[], int length)
{
	int i, j;
	for (i = 0; i < length - 1; i++)
	{
		for (j = length - 2; j >= i; j--)
		{
			if (a[j]>a[j + 1])
			{
				swap(a, j, j + 1);
			}
		}
	}
}
```
```cpp
#include<stdlib.h>
#include<time.h>
#include<stdio.h>
#define LENGTH 10
int main()
{
	
	clock_t start, finish;
	start = clock();

	srand(unsigned(time(0)));

	int a[LENGTH];

	printf("排序前：");
	for (int k = 9; k >=0; k--)
	{
//		a[k] = rand()%10000;
		a[k] = k + 1;
		printf("%d ", a[k]);
	}
	
	BubbleSort1(a,LENGTH);
	
	printf("\n排序后：");
	for (int i1 = 0; i1 < 10; i1++)
	{
		printf("%d ", a[i1]);
	}

	finish = clock();
	printf("共耗时%.3lf秒", ((double)finish - start) / 1000);
	return 0;
}
```