---
title: 青蛙跳台阶题目及其延伸
date: 2016-05-30 10:51:35
categories: C++面试
tags:
	- C++
	- 算法
	- 数据结构
	- 腾讯
---

# 青蛙跳台阶题目初级
腾讯面试题目：有一个50阶的楼梯，每次可以上一阶或者两阶，总共的方法有多少种。
算法应用：递归

```cpp
#include<stdio.h>
#include<stdlib.h>
//int tencent(int n)
double tencent(int n) //由于递归50次计算时一个很大的值，为了防止溢出，将int改成double型。
{
    if(n==1)
    {
        return 1.0;
    }


    else if(n==2)
    {
        return 2.0;
    }
    else
    {
        return tencent(n-1)+tencent(n-2);
    }
}
void main()
{
    printf("%f",tencent(50));


    getchar();


}
```
<!-- more -->

# 青蛙跳台阶进阶

题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

**********题目来自牛客网@xiaobaibai同学，感谢****************************

```cpp
class Solution 
{
public:
    int jumpFloorII(int number) {
        if(number==0)
            {
            return -1;
        	}
        else if(number==1)
            {
            return 1;
        }

        else

            {

            return jumpFloorII(number-1)*2;
        }
    }
};
```





