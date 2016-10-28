---
title: Python-列表和元组
date: 2016-10-25 15:46:16
categories: python
tags:
	- python
---



两者最主要的区别：

- 列表可以修改
- 元组不可以修改

序列中的元素可以是数字，可以是字符串，甚至可以是另一个序列。

例如：

```python
edwad = ['Edward', 42]
john = ['John Smith', 50]
database = [edwad, john]
print database[0]
print database[1]
print database

//运行以上代码，打印如下内容

['Edward', 42]
['John Smith', 50]
[['Edward', 42], ['John Smith', 50]]
```

## 通用序列操作

索引，分片，加、乘、检查某个元素是否属于序列成员。

<!-- more -->

除此之外，还可以计算序列长度、找出最大最小的元素。

- 索引：索引序号0指向第一个序列中的元素，而使用负数索引时，Python会从最后一个元素开始计数。最后一个元素的索引值是-1.

  ```python
  print '中文测试'
  fourth = raw_input('Year: ')[3]
  print fourth

  //执行上述代码输出如下内容

  Input：
  	Year: 2005
  Output：
      5
  ```


- 分片：分片操作访问一定范围内的元素。分片通过冒号分隔的两个索引来实现。例如：

  ```python
  tag = '<a href="http://www.python.org">Python Web Site </a>'
  print tag[9:30]

  //执行代码输出以下内容
  http://www.python.org
  ```

  ​	需要注意的是索引被冒号分隔的两个索引，符合左闭右开原则，即分号左边对应的元素被包含，而分号右边对应的元素不被包含。

```python
numbers = [1,2,3,4,5,6,7,8,9,10]
#如果要访问序列中最后三个元素时，可以使用以下方法
print numbers[-3:]
#程序输出为：
[8,9,10]

#同样适用于序列开始的元素
print numbers[:3]

//Output:
[1,2,3]

#打印整个序列
print numbers[:]
[1,2,3,4,5,6,7,8,9,10]
```

​	步长增大的情况：

​	普通的分片情况中，步长被设置为1.同理，步长也可以设置为其他比1大的数。

```python
numbers = [1,2,3,4,5,6,7,8,9,10]

print numbers[0:10:1]	# 步长为1
#[1,2,3,4,5,6,7,8,9,10]
print numbers[0:10:2]	# 步长为2
#[1, 3, 5, 7, 9]
```

- 序列相加：<font color=yellow>两种相同类型的序列才能进行连接操作。_引自Python基础教程（第二版）</font>

  ```python
  print [1,2,3]+[4,5,6]
  print [1,2,3]+4

  #[1, 2, 3, 4, 5, 6]
  #Traceback (most recent call last):
  # File "E:\Eclipse+PyDev\eclipse-jee-#mars\spider\demo\__init__.py", line 19, in    #<module>
  #    print [1,2,3]+4
  #TypeError: can only concatenate list (not #"int") to list
  #提示只有序列才可以和序列相加

  #——————————————博主注————————————————————
  print [1,2,3]+['world']
  输出：[1, 2, 3, 'world']

  ```

上述代码中最后一个例子是int数列和一个字符串序列相加，结果可以顺利输出connect之后的结果，因此，《Python基础教程（第二版）》中的“两种相同类型的序列才能进行连接操作”该句存疑。



- 乘法：一个数字乘以一个序列会生成一个新的序列。

  ```python
  sequence = [None]*10
  print sequence
  #输出：[None, None, None, None, None, None, None, None, None, None]
  ```


- 成员资格检查：使用in运算符，如果被检查的元素存在序列中，返回一个bool运算符True，否则返回False

  ```python
  permissions = 'rwct'
  print 'w' in permissions

  colors = ['blue','yellow','brown','red','black']
  print 'blue' in colors
  print 'white' in colors

  #输出结果为：
  #True
  #True
  #False
  ```


- 长度、最小最大值：对应len,min,max函数。

  ```python
  numbers = [100,2,56]
  print '长度为：' ,len(numbers)
  print '最小值为：',min(numbers)
  print '最大值为：',max(numbers)

  # -------------输出为：-----------------------
  长度为： 3
  最小值为： 2
  最大值为： 100
  ```

## 列表

### list函数

将字符串转换为列表

```python
print list('python')

#-------------输出：-------------
['p', 'y', 't', 'h', 'o', 'n']
```

list函数适用于所有类型的序列，而不只是字符串。

### 列表方法

> 调用方法一般为：对象.方法(参数)

- append():用于在列表末尾追加新的对象

- count()方法：统计某个元素在列表中出现的次数。

- extend():在列表的末尾一次性追加一个序列中的多个值。即用一个列表扩展原有列表。

- index():

  ```python
  seq = ['I','am','a','teacher','a']
  seq1 = ['You','are','a','student'] 
  print seq
  seq.append('student')
  print seq
  print seq.count('a')
  seq.extend(seq1)
  print seq

  #-------输出：-----------
  ['I', 'am', 'a', 'teacher', 'a']
  ['I', 'am', 'a', 'teacher', 'a', 'student']
  2
  ['I', 'am', 'a', 'teacher', 'a', 'student', 'You', 'are', 'a', 'student']
  ```


- insert()方法：序列.insert(index, object)

  ```python
  numbers = [1,2,3,5,5]
  numbers.insert(3,'four')
  print numbers
  #--------------output:-----------
  [1, 2, 3, 'four', 5, 5]
  ```


- pop()方法：移除列表中的一个元素（默认是最后一个），且返回该元素的值。

- remove()方法：用于移除列表中某个值的第一个匹配项。

- reverse()方法：反转序列

  ```python
  x=['to','be','or','not','to','be']
  print x
  x.remove('be')
  print x
  x.reverse()
  print x

  #-----output:---------------
  ['to', 'be', 'or', 'not', 'to', 'be']
  ['to', 'or', 'not', 'to', 'be']
  ['be', 'to', 'not', 'or', 'to']
  ```


- sort()方法：在原位置对列表进行排序

  ```python
  num=[5,3,5,72,1,4]
  num.sort()
  print num

  #-----------output:---------
  [1, 3, 4, 5, 5, 72]
  ```



- 高级排序：

  - compare(x,y)函数:x>y,返回正数，x<y返回负数，相等返回0.

  - sort()函数的参数可选参数：cmp，key和reverse。

    ```python
     x=['aardvar','abalone','came','add','aerate']
    x.sort(key=len) #以长度len为关键词进行排序
    print x
    #---------output：-----------
    ['add', 'came', 'aerate', 'aardvar', 'abalone']
    ```
    ```python
    num=[5,3,5,72,1,4]
    num.sort(reverse=True)	#排序且反转
    print num

    #-------output:---------
    [72, 5, 5, 4, 3, 1]
    ```