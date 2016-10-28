---
title: Beautiful Soup常用语法
date: 2016-10-14 15:37:14
categories: python
tags:
	- Beautiful Soup
	- 爬虫
---

# Beautiful Soup简介

> [Beautiful Soup](http://beautifulsoup.readthedocs.io/zh_CN/latest/)是一个可以从HTML或XML文件中提取数据的Python库。它能够通过你喜欢的转换器实现惯用的文档导航，查找，修改文档的方式。Beautiful Soup会帮你节省数小时甚至数天的工作时间。

# 快速开始



下面的一段HTML代码将作为例子被多次用到.这是*爱丽丝梦游仙境的*的一段内容(以后内容中简称为 *爱丽丝* 的文档):

```html
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
```

## 几个简单的浏览结构化数据的方法

```python
soup.title
# <title> The Dormouse's story </title>

soup.title.name
# u'title'

soup.title.string
# u'The Dormouse's story'

soup.title.parent.name
# u'head'

soup.p
# <p class="tltle"><b>The Dormouse's story</b></p>

soup.p['class']
# u'title'

soup.a
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

soup.find_all('a')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.find(id="link3")
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

```



<!-- more -->

从文档中找到所有<a>标签的连接：

```python
for link in soup.find_all('a'):
    print link.get('href') 
    # http://example.com/elsie
    # http://example.com/lacie
    # http://example.com/tillie
```

从文档中获取所有文字内容

```python
print soup.get_text()
# The Dormouse's story
#
# The Dormouse's story
#
# Once upon a time there were three little sisters; and their names were
# Elsie,
# Lacie and
# Tillie;
# and they lived at the bottom of a well.
#
# ...
```

## 网页解析器比较

| 解析器          | 使用方法                                 | 优势                                    | 劣势                                   |
| :----------- | :----------------------------------- | :------------------------------------ | :----------------------------------- |
| Python标准库    | BeautifulSoup(markup, "html.parser") | - Python的内置标准库   - 执行速度适中  - 文档容错能力强  | - Python 2.7.3 or 3.2.2前的版本中文文档容错能力差 |
| lxml HTML解析器 | BeautifulSoup(markup, "lxml")        | 速度快  文档容错能力强                          | - 需要安装C语言库                           |
| lxml HTML解析器 | BeautifulSoup(markup,["lxml-xml"])   | 速度快   唯一支持XML的解析器                     | - 需要安装C语言库                           |
| html5lib     | BeautifulSoup(markup,"html5lib")     | - 最好的容错性 - 以浏览器的方式解析文档 - 生成HTML5格式的文档 | - 速度慢 -不依赖外部扩展                       |

# 使用入门

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(open("index.html"))

soup = BeautifulSoup("<html>data</html>")
```

# 对象的种类

BeautifulSoup将复杂的HTML文档转换成一个复杂的树形结构，每个结点都是Python对象，所有对象都可以归为4种：

- Tag
- NavigableString
- BeautifulSoup
- Comment

## Tag

`Tag`对象与XML或HTML原生文档中的tag相同：

```python
soup = BeautifulSoup('<b class="boldest">Extremely bold</b>')
tag = soup.b
type(tag)
# <class 'bs4.element.Tag'>
```

Tag中最重要的两个属性：name和attributes

###  name

每个tag都有自己的名字，通过`.name`来获取：

```python
tag.name
# u'b'
```

如果改变了tag的name,那将影响所有通过当前Beautiful Soup对象生成的HTML文档:

```python
tag.name = "blockquote"
tag
# <blockquote class="boldest">Extremely bold</blockquote>
```

### Attributes

一个tag可能有很多个属性. tag `` 有一个 “class” 的属性,值为 “boldest” . tag的属性的操作方法与字典相同:

```python
tag['class']
# u'boldest'
```

也可以直接”点”取属性, 比如: `.attrs` :

```python
tag.attrs
# {u'class': u'boldest'}
```

```python
tag.attrs
# {u'class': u'boldest'}


tag['class'] = 'verybold'
tag['id'] = 1
tag
# <blockquote class="verybold" id="1">Extremely bold</blockquote>

del tag['class']
del tag['id']
tag
# <blockquote>Extremely bold</blockquote>

tag['class']
# KeyError: 'class'
print(tag.get('class'))
# None
```

## NavigableString



Beautiful Soup用 `NavigableString` 类来包装tag中的字符串:

```python
tag.string
# u'Extremely bold'

# 检查一下其类型
type(tag.string)
# <class 'bs4.element.NavigableString'>
```

## BeautifulSoup

`BeautifulSoup` 对象并不是真正的HTML或XML的tag,所以它没有name和attribute属性.但有时查看它的 `.name` 属性是很方便的,所以 `BeautifulSoup` 对象包含了一个值为 “[document]” 的特殊属性 `.name`

```python
soup.name
# u'[document]'
```

## Comment 注释

```python
markup = "<b><!--Hey, buddy. Want to buy a used parser?--></b>"
soup = BeautifulSoup(markup)
comment = soup.b.string
type(comment)
# <class 'bs4.element.Comment'>
```

# 遍历文档树

```python
html_doc = """
<html><head><title>The Dormouse's story</title></head>
    <body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""

# 导入bs4模块
from bs4 import BeautifulSoup
# 以html.parser方式遍历文档html_doc
soup = BeautifulSoup(html_doc, 'html.parser')
```



## .contents 和 .children



tag的 `.contents` 属性可以将tag的子节点以列表的方式输出:

```python
head_tag = soup.head
head_tag
# <head><title>The Dormouse's story</title></head>

head_tag.contents
[<title>The Dormouse's story</title>]

 # 索引形式获取
title_tag = head_tag.contents[0]
title_tag
# <title>The Dormouse's story</title>

 title_tag.contents
# [u'The Dormouse's story']
```

通过tag的 `.children` 生成器,可以对tag的子节点进行循环:

```python
for child in title_tag.children:
    print child 
    # The Dormouse's story
```

## 所有子孙节点：.descendants



.`contents` 和 `.children` 属性仅包含tag的直接子节点，`.descendants` 属性可以对所有tag的子孙节点进行递归循环，和 children类似，我们也需要遍历获取其中的内容。

```python
for child in head_tag.descendants:
    print child 
    # <title>The Dormouse's story</title>
    # The Dormouse's story
```



上面的例子中, <head>标签只有一个子节点,但是有2个子孙节点:<head>节点和<head>的子节点, `BeautifulSoup` 有一个直接子节点(<html>节点),却有很多子孙节点:

```python
len(list(soup.children))
# 1
len(list(soup.descendants))
# 25
```

## 父节点.parent



通过 `.parent` 属性来获取某个元素的父节点.在例子“爱丽丝”的文档中,<head>标签是<title>标签的父节点:

```python
title_tag = soup.title
title_tag
# <title>The Dormouse's story</title>
title_tag.parent
# <head><title>The Dormouse's story</title></head>
```

### .parents



通过元素的 `.parents` 属性可以递归得到元素的<font color="red">所有父辈节点</font>,下面的例子使用了`.parents` 方法遍历了<a>标签到根节点的所有节点.

```python
link = soup.a
link
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
for parent in link.parents:
    if parent is None:
        print parent 
    else:
        print parent.name 
# p
# body
# html
# [document]
# None
```

## 兄弟节点

### .next_sibling 和 .previous_sibling

在文档树中,使用 `.next_sibling` 和 `.previous_sibling` 属性来查询兄弟节点:

```python
sibling_soup.b.next_sibling
# <c>text2</c>

sibling_soup.c.previous_sibling
# <b>text1</b>
```

# 搜索文档树



- find()
- find_all()

## 过滤器

### 字符串

最简单的过滤器是字符串.在搜索方法中传入一个字符串参数,Beautiful Soup会查找与字符串完整匹配的内容,下面的例子用于查找文档中所有的<b>标签:

```python
soup.find_all('b')
# [<b>The Dormouse's story</b>]
```

### 正则表达式

如果传入正则表达式作为参数,Beautiful Soup会通过正则表达式的 `match()` 来匹配内容.下面例子中找出所有<font color="red">以b开头</font>的标签,这表示<body>和<b>标签都应该被找到:

```python
import re
for tag in soup.find_all(re.compile("^b")):
    print tag.name 
# body
# b
```

下面代码找出所有名字中<font color="red">包含”t”</font>的标签:

```python
for tag in soup.find_all(re.compile("t")):
    print(tag.name)
# html
# title
```

### 列表



如果传入列表参数,Beautiful Soup会将与列表中任一元素匹配的内容返回.下面代码找到文档中所有<a>标签和<b>标签:

```python
soup.find_all(["a", "b"])	# 列表的形式搜索
# [<b>The Dormouse's story</b>,
#  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

### True

`True` 可以匹配任何值,下面代码查找到<font color="red">所有的tag</font>,但是不会返回字符串节点

```python
for tag in soup.find_all(True):
    print tag.name 
# html
# head
# title
# body
# p
# b
# p
# a
# a
# a
# p
```

### 方法

该文档暂不介绍。

## find_all()



find_all( [name](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id35) , [attrs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#css) , [recursive](http://beautifulsoup.readthedocs.io/zh_CN/latest/#recursive) , [string](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id36) , [**kwargs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#keyword) )



`find_all()` 方法搜索当前tag的所有tag子节点,并判断是否符合过滤器的条件.这里有几个例子:

```python
# 搜索name
soup.find_all("title")
# [<title>The Dormouse's story</title>]

# 搜索name 和属性
soup.find_all("p", "title")
# [<p class="title"><b>The Dormouse's story</b></p>]

# 搜索name
soup.find_all("a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

# 如果一个指定名字的参数不是搜索内置的参数名,搜索时会把该参数当作指定名字 tag 的属性来搜索
# 如果包含一个名字为 id 的参数,Beautiful Soup会搜索每个tag的”id”属性
soup.find_all(id="link2")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

# 正则匹配
import re
soup.find(string=re.compile("sisters"))
# u'Once upon a time there were three little sisters; and their names were\n'
```



### name 参数



`name` 参数可以查找所有名字为 `name` 的tag,字符串对象会被自动忽略掉.

```python
soup.find_all("title")
# [<title>The Dormouse's story</title>]
```

### keyword 参数



如果一个指定名字的参数不是搜索内置的参数名,搜索时会把该参数当作指定名字tag的属性来搜索,如果包含一个名字为 `id` 的参数,Beautiful Soup会搜索每个tag的”id”属性.

```python
soup.find_all(id='link2')
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```

如果传入参数`href`参数，Beautiful Soup会搜索每个tag的”href“属性。

```python
soup.find_all(href=re.compile("elsie"))
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
```

下面的例子在文档树中查找所有包含`id`属性的tag，无论`id`的值是什么：

```python
soup.find_all(id=True)
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

### string参数

通过`string`参数可以搜索文档中的字符串内容，与`name` 参数的可选值一样，`string` 参数接收 字符串，正则表达式，列表，True。

```python
soup.find_all(string="Elsie")
# [u'Elsie']

soup.find_all(string=["Tillie", "Elsie", "Lacie"])
# [u'Elsie', u'Lacie', u'Tillie']

soup.find_all(string=re.compile("Dormouse"))
[u"The Dormouse's story", u"The Dormouse's story"]

def is_the_only_string_within_a_tag(s):
    ""Return True if this string is the only child of its parent tag.""
    return (s == s.parent.string)

soup.find_all(string=is_the_only_string_within_a_tag)
# [u"The Dormouse's story", u"The Dormouse's story", u'Elsie', u'Lacie', u'Tillie', u'...']
```



虽然 `string` 参数用于搜索字符串,还可以与其它参数混合使用来过滤tag，Beautiful Soup会找到 `.string` 方法与 `string` 参数值相符的tag.下面代码用来搜索内容里面包含“Elsie”的<a>标签:

```python
soup.find_all("a", string="Elsie")
# [<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>]
```

### limit参数

`find_all()` 方法返回全部的搜索结果，如果文档树很大，搜索结果则非常慢。

如果不需要全部结果，可以使用`limit` 参数限制返回结果数量。

当搜索到的结果数量达到`limit` 的限制时，就停止搜索返回的结果。

文档中总共有3个tag符合搜索条件，但结果只返回了2个，因为对其搜索数量做了限制。

```python
sou.find_all('a', limit=2)
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```

### recursive 参数

调用tag的`find_all()` 方法时，Beautiful Soup会检索当前tag的所有子孙节点，如果只想搜索tag的直接子节点，可以使用参数`recursive=False`.

一段简单的文档：

```html
<html>
 <head>
  <title>
   The Dormouse's story
  </title>
 </head>
...
```

是否使用`recursive`参数的搜索结果：

```python
soup.html.find_all("title")
# [<title>The Dormouse's story</title>]


soup.html.find_all("title", recursive=False)
# []
```



这是一段文档片段：

```html
<html>
        <head>
        <title>
        The Dormouse's story
    	</title>
        </head>
        ...
```

<title>标签在<html>标签下，但并不是直接子节点，<head>标签才是直接子节点。

在允许查询所有后代节点时，Beautiful Soup 能够查到<title>标签，但是使用了`recursive=False` 参数之后，只能查找直接子节点，这样就查询不到<title>标签了。



### 像调用find_all()一样调用tag

`find_all()` 几乎是BeautifulSoup中最常用的搜索方法，所以定义了其简写方法。

`Beautiful Soup`对象和`tag` 对象可以被当作一个方法来使用，这个方法的执行结果与调用这个对象的`find_all()` 方法相同，如下的两行代码是等价的：

```python
soup.find_all("a")
soup("a")
```

这两行代码也是等价的：

```python
soup.title.find_all(string=True)
soup.title(string=True)
```





## find()



find( [name](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id35) , [attrs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#css) , [recursive](http://beautifulsoup.readthedocs.io/zh_CN/latest/#recursive) , [string](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id36) , [**kwargs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#keyword) )

`find_all()` 方法将返回文档中符合条件的所有tag，尽管有时候只想得到一个结果，比如文档中只有一个<body>标签，Name使用`find_all()` 方法来查找<body> 标签就不太合适， 使用`find_all()` 方法并设置`limit=1` 参数不如直接使用`find()` 方法。

下面两行代码是等价的：

```python
soup.find_all('title', limit=1)
## [<title>The Dormouse's story</title>]

soup.find('title')
# [<title>The Dormouse's story</title>]
```

> 唯一的区别是find_all() 方法的返回结果是值包含一个元素的列表，而find()方法直接返回结果。
>
> find_all() 方法没有找到目标是返回空列表，find()方法找不到目标时，返回`None` 

## find_parent() 和 find_parent()

find_parents( [name](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id35) , [attrs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#css) , [recursive](http://beautifulsoup.readthedocs.io/zh_CN/latest/#recursive) , [string](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id36) , [**kwargs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#keyword) )

find_parent( [name](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id35) , [attrs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#css) , [recursive](http://beautifulsoup.readthedocs.io/zh_CN/latest/#recursive) , [string](http://beautifulsoup.readthedocs.io/zh_CN/latest/#id36) , [**kwargs](http://beautifulsoup.readthedocs.io/zh_CN/latest/#keyword) )

> find_all() 和 find() 只搜索当前节点的所有子节点，孙子节点等。
>
> find_parents() 和  find_parent() 用来搜索当前节点的父辈结点，搜索方法与普通tag的搜索方法相同，搜索文档搜索文档包含的内容。

我们从一个文档中的一个叶子节点开始：

```python
a_string = soup.find(string="Lacie")
a_string
# u'Lacie'

a_string.find_parents("a")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

a_string.find_parent("p")
# <p class="story">Once upon a time there were three little sisters; and their names were
#  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
#  and they lived at the bottom of a well.</p>

a_string.find_parents("p", class="title")
# []
```

## 修改文档树

## 输出

## 指定文档解析器

## 编码

