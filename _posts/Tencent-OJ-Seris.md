---
title: 腾讯研发编程题
date: 2016-08-20 16:01:07
categories: C++
tags:
	- 查找
---

# 1. 腾讯2016研发工程师编程题——微信红包

> 春节期间小明使用微信收到很多个红包，非常开心。在查看领取红包记录时发现，某个红包金额出现的次数超过了红包总数的一半。请帮小明找到该红包金额。写出具体算法思路和代码实现，要求算法尽可能高效。
给定一个红包的金额数组gifts及它的大小n，请返回所求红包的金额。
若没有金额超过总数的一半，返回0。

测试样例：
> [1,2,3,2,2],5


返回：
> 2

代码：

```cpp
int getValue(vector<int> gifts, int n)
    {
        for (vector<int>::iterator it = gifts.begin(); it != gifts.end(); it++)
        {
            int temp = count(gifts.begin(), gifts.end(), *it);
            if (temp > gifts.size() / 2)
                return *it;
        }
        return 0;
    }
```