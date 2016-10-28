---
title: python入门学习
date: 2016-10-12 21:01:38
categories: python
tags:
	- python
---



##  print命令和逗号

print "Enter your name:"

然后使用raw_input()函数得到用户的相应，someName = raw_input() 。

运行代码，键入名字，会得到“Enter your name:”和Name并未在同一行。如果希望在同一行，只需要在printf末尾放一个逗号即可。



## 运算符

### 成员运算符

- in 如果在指定的序列中找到值返回true，否则返回false
- not in 如果在指定的序列中没有找到值则返回true，否则返回false

### 身份运算符

- is 是判断两个标识符是否是引用自一个对象
- is not 判断两个标识符是不是引用自不同对象

### 条件判断

python不支持switch语句，多个条件判断，只能用elif实现。



<!-- more -->

## for循环语句

for循环的语法格式如下：

`for iterating_var in sequence:`

​	`statements(s)`

实例：
```python
for letter in 'python':
	print '当前字母：', letter

fruits = ['banana'，'apple','mango']
for fruit in fruits:
	print '当前字母：', fruit

print "Good bye!"
```
## python 函数

### 定义一个函数

简单的规则：

- 函数代码块以def为关键词开头,后接函数标识符名称和圆括号()
- 任何传入参数和自变量必须放在圆括号中间。
- 函数的第一行语句可以选择性地使用文档字符串--用于存放函数说明
- 函数内容以冒号起始，并且缩进
- return 结束函数，选择性返回一个值给调用方。不带表达式的return相当于返回None。

语法：

```python
def functionname( parameters ):
	"函数_文档字符串"
    function_suite
    return [expression]
```

实例：

```python
def printme( str ):
    "打印传入的字符串到标准显示设备上"
	print str
    return
```

### 函数调用

```python
def printme(str):
    "打印传入的字符串"
    print str;
    return;

# 调用函数
printme("我要调用用户自定义函数！")；
printme（"再调用一次！"）；
```

### 按值传递和按引用传递参数

python中所有参数都是按引用传递。

如果在函数中修改了参数，那么在调用这个函数的函数里，原始的参数也被改变了。



例如：

​		

```python
def changeme(mylist):
    "修改传入的列表"
    mylist.append([1,2,3,4]);
    print "函数内取值", mylist
    return 
#调用changeme函数
mylist = [10,20,30];
changeme(mylist)
print "函数外取值：",mylist

#输出为

#函数内取值 [10, 20, 30, [1, 2, 3, 4]]
#函数外取值： [10, 20, 30, [1, 2, 3, 4]]
'''
```