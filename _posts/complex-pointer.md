---
title: 复杂指针解析
date: 2016-05-31 23:01:57
categories: C++
tags:
	- C++
	- 指针
---

# 右左法则----复杂指针解析<转>


http://blog.csdn.net/code_crash/article/details/4854965

首先看看如下一个声明：
 

`int* ( *( *fun )( int* ) )[10];`
 
这是一个会让初学者感到头晕目眩、感到恐惧的函数指针声明。在熟练掌握C/C++的声明语法之前，不学习一定的规则，想理解好这类复杂声明是比较困难的。
 
C/C++所有复杂的声明结构，都是由各种声明嵌套构成的。

如何解读复杂指针声明？右左法则是一个很著名、很有效的方法。

不过，右左法则其实并不是C/C++标准里面的内容，它是从C/C++标准的声明规定中归纳出来的方法。

C/C++标准的声明规则，是用来解决如何创建声明的，而右左法则是用来解决如何辩识一个声明的，从嵌套的角度看，两者可以说是一个相反的过程。右左法则的英文原文是这样说的：
 
> The right-left rule: Start reading the declaration from the innermost parentheses, go right, and then go left. When you encounter parentheses, the direction should be reversed. Once everything in the parentheses has been parsed, jump out of it. Continue till the whole declaration has been parsed.
 
<!-- more --> 


这段英文的翻译如下：
 
右左法则：首先从最里面的圆括号看起，然后往右看，再往左看。每当遇到圆括号时，就应该掉转阅读方向。一旦解析完圆括号里面所有的东西，就跳出圆括号。重复这个过程直到整个声明解析完毕。
 
    笔者要对这个法则进行一个小小的修正，应该是从未定义的标识符开始阅读，而不是从括号读起，之所以是未定义的标识符，是因为一个声明里面可能有多个标识符，但未定义的标识符只会有一个。
 
    现在通过一些例子来讨论右左法则的应用，先从最简单的开始，逐步加深：


`int (*func)(int *p);`
 
首先找到那个未定义的标识符，就是func，它的外面有一对圆括号，而且左边是一个*号，这说明func是一个指针，然后跳出这个圆括号，先看右边，也是一个圆括号，这说明(*func)是一个函数，而func是一个指向这类函数的指针，就是一个函数指针，这类函数具有int*类型的形参，返回值类型是int。
 
`int (*func)(int *p, int (*f)(int*));`
 
func被一对括号包含，且左边有一个`*`号，说明`func`是一个指针，跳出括号，右边也有个括号，那么func是一个指向函数的指针，这类函数具有`int *`和`int (*)(int*)`这样的形参，返回值为int类型。再来看一看func的形参`int (*f)(int*)`，类似前面的解释，f也是一个函数指针，指向的函数具有int*类型的形参，返回值为int。
 
`int (*func[5])(int *p);`
 
func右边是一个[]运算符，说明func是一个具有5个元素的数组，func的左边有一个`*`，说明func的元素是指针，要注意这里的`*`不是修饰func的，而是修饰func[5]的，原因是[]运算符优先级比`*`高，func先跟[]结合，因此`*`修饰的是func[5]。跳出这个括号，看右边，也是一对圆括号，说明func数组的元素是函数类型的指针，它所指向的函数具有`int*`类型的形参，返回值类型为int。
 
 
`int (*(*func)[5])(int *p);`
 
func被一个圆括号包含，左边又有一个`*`，那么func是一个指针，跳出括号，右边是一个[]运算符号，说明func是一个指向数组的指针，现在往左看，左边有一个`*`号，说明这个数组的元素是指针，再跳出括号，右边又有一个括号，说明这个数组的元素是指向函数的指针。总结一下，就是：func是一个指向数组的指针，这个数组的元素是函数指针，这些指针指向具有`int*`形参，返回值为int类型的函数。
 
`int (*(*func)(int *p))[5];`
 
func是一个函数指针，这类函数具有int*类型的形参，返回值是指向数组的指针，所指向的数组的元素是具有5个int元素的数组。
 
要注意有些复杂指针声明是非法的，例如：
 
`int func(void) [5];`
 
func是一个返回值为具有5个int元素的数组的函数。但C语言的函数返回值不能为数组，这是因为如果允许函数返回值为数组，那么接收这个数组的内容的东西，也必须是一个数组，但C/C++语言的数组名是一个不可修改的左值，它不能直接被另一个数组的内容修改，因此函数返回值不能为数组。
 
`int func[5](void);`
 
func是一个具有5个元素的数组，这个数组的元素都是函数。这也是非法的，因为数组的元素必须是对象，但函数不是对象，不能作为数组的元素。
 
实际编程当中，需要声明一个复杂指针时，如果把整个声明写成上面所示这些形式，将对可读性带来一定的损害，应该用typedef来对声明逐层分解，增强可读性。
 
typedef是一种声明，但它声明的不是变量，也没有创建新类型，而是某种类型的别名。typedef有很大的用途，对一个复杂声明进行分解以增强可读性是其作用之一。例如对于声明：
 
`int (*(*func)(int *p))[5];`
 
可以这样分解：
 
    typedef  int (*PARA)[5];
    typedef PARA (*func)(int *);
 
这样就容易看得多了。
 
typedef的另一个作用，是作为基于对象编程的高层抽象手段。在ADT中，它可以用来在C/C++和现实世界的物件间建立关联，将这些物件抽象成C/C++的类型系统。在设计ADT的时候，我们常常声明某个指针的别名，例如：
 
    typedef struct node * list;
 
从ADT的角度看，这个声明是再自然不过的事情，可以用list来定义一个列表。但从C/C++语法的角度来看，它其实是不符合C/C++声明语法的逻辑的，它暴力地将指针声明符从指针声明器中分离出来，这会造成一些异于人们阅读习惯的现象，考虑下面代码：
 
    const struct node *p1;
    typedef struct node *list;
    const list p2;
 
p1类型是`const struct node*`，那么p2呢？如果你以为就是把list简单“代入”p2，然后得出p2类型也是`const struct node*`的结果，就大错特错了。p2的类型其实是`struct node * const p2`，那个const限定的是p2，不是node。造成这一奇异现象的原因是指针声明器被分割，标准中规定：
 
6.7.5.1 Pointer declarators
 
Semantics
 
 If in the declaration ‘‘T D1’’, D1 has the form
 
* type-qualifier-listopt D
 
and the type specified for ident in the declaration ‘‘T D’’ is
 
‘‘derived-declarator-type-list T’’
 
then the type specified for ident is
 
‘‘derived-declarator-type-list type-qualifier-list pointer to T’’
 
For each type qualifier in the list, ident is a so-qualified pointer.
 
指针的声明器由指针声明符`*`、可选的类型限定词type-qualifier-listopt和标识符D组成，这三者在逻辑上是一个整体，构成一个完整的指针声明器。这也是多个变量同列定义时指针声明符必须紧跟标识符的原因，例如：
 
    int *p, q, *k;
 
p和k都是指针，但q不是，这是因为`*p、*k`是一个整体指针声明器，以表示声明的是一个指针。编译器会把指针声明符左边的类型包括其限定词作为指针指向的实体的类型，右边的限定词限定被声明的标识符。但现在`typedef struct node *list`硬生生把`*`从整个指针声明器中分离出来，编译器找不到`*`，会认为const list p2中的const是限定p2的，正因如此，p2的类型是`node * const`而不是`const node*。`
 
虽然`typedef struct node* list`不符合声明语法的逻辑，但基于typedef在ADT中的重要作用以及信息隐藏的要求，我们应该让用户这样使用list A，而不是`list *A`，因此在ADT的设计中仍应使用上述typedef语法，但需要注意其带来的不利影响。
