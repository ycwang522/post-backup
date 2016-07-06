---
title: 面向对象的五个基本原则
date: 2016-06-25 17:08:05
categories: C++
tags:
	- C++
	- 面向对象
---

- <font color="green">单一职责原则</font>（Single-Resposibility Principle）：一个类，最好只做一件事，只有一个引起它的变化。单一职责原则可以看做是低耦合、高内聚在面向对象原则上的引申，将职责定义为引起变化的原因，以提高内聚性来减少引起变化的原因。 


- <font color="green">开放封闭原则</font>（Open-Closed principle）：软件实体应该是可扩展的，而不可修改的。也就是，对扩展开放，对修改封闭的。 

- <font color="green">Liskov替换原则</font>（Liskov-Substituion Principle）：子类必须能够替换其基类。这一思想体现为对继承机制的约束规范，只有子类能够替换基类时，才能保证系统在运行期内识别子类，这是保证继承复用的基础。 

- <font color="green">依赖倒置原则</font>（Dependecy-Inversion Principle）：依赖于抽象。具体而言就是高层模块不依赖于底层模块，二者都同依赖于抽象；抽象不依赖于具体，具体依赖于抽象。 

- <font color="green">接口隔离原则</font>（Interface-Segregation Principle）：使用多个小的专门的接口，而不要使用一个大的总接口