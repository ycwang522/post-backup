---
title: 计算机网络（四）：因特网的路由选择协议
date: 2016-07-07 10:37:23
categories: 计算机网络
tags:
	- RIP
	- OSFP
---

## 路由选择协议的几个基本概念

### 理想的路由算法

**路由选择协议的核心就是路由算法**。
理想的路由选择算法应该具有如下一些特点：
- 算法必须是正确和完整的。正确是指：沿着各路由表所指引的路由，分组一定能够最终到达目的网络和目的计算机。
- 算法在计算上应该简单
- 能够适应通信量和网络拓扑的变化，即具有自适应性
- 具有稳健性
- 公平性，对所有用户（除了少数优先用户）都是平等的。
- 算法应该是最佳的

从算法能否自适应进行调整来划分：
- <font color="green">静态路由选择策略</font>，非自适应路由选择：简单和开销较小，适用于简单的小网络。
- <font color="green">动态路由选择策略</font>

### 分层次的路由选择协议

因特网采用的路由选择协议主要是自适应的，分布式路由选择协议。主要原因有：

- 因特网规模超大
- 许多单位不愿意外界了解自己单位网络的布局细节和本部门所采用的路由选择协议。

基于以上原因，因特网将整个互联网划分为许多较小的**自治系统，记为AS**。
</br>
<font color="brown">尽管一个AS使用了多种内部路由选择协议和度量，但重要的事一个AS对其他AS表现出的是一个单一的和一致的路由选择策略。</font>

目前的因特网中，一个大的ISP就是一个自治系统。这样因特网就把路由选择协议划分为两大类。
- **内部网关协议（IGP）** 自治系统内部使用的路由选择协议。RIP和OSPF
- **外部网关协议（EGP)** BGP-4

<!-- more -->

## 内部网关协议（IGP）——RIP

**路由信息协议RIP**是内部网关协议IGP中最先得到广泛使用的协议。
> RIP是一种分布式的基于距离向量的路由选择协议，是因特网的标准协议，最大的特点是简单。

RIP协议要求网络中的每一个路由器都要维护从它自己到其他每一个目的网络的距离记录（**距离向量**）。
- 一路由器到直接连接的网络的距离定义为1
- 一路由到非直接连接的网络的距离定义为**所经过的路由器数加1.**

<font color="green">**RIP允许一条路径最多只能包含15个路由器。因此“距离为16即相当于不可到达。”**</font>
	
	可见RIP只适用于小型互联网 


RIP协议的特点：
- 仅和相邻路由交换信息
- 路由器交换的信息是当前本路由器所知道的全部信息，即自己的路由表。即交换的信息是“我到本自治系统中所有网络的最短距离，以及到每个网络应该经过的下一跳路由器。”
- 按固定的时间间隔交换路由信息。

路由表中最重要的信息就是：
**到某个网络的距离，以及应经过的下一跳地址。**
**路由表更新的原则是找出到每个目的网络的最短距离。**

### 路由信息协议的工作过程

路由信息协议RIP的设计思想，要求路由器周期性地向外发送路由刷新报文。路由刷新报文主要由若干（V，D）组成的表。
V代表标识该路由器可以到达的目的网络或目的主机。
D代表对应路由上的跳步数。

工作过程如下：
- 路由表的简历。对路由表（V，D）进行初始化，初始化时，路由器只包含所有与该路由器直接相连接的网络。此时（V，D）表中的距离都为0.
- 路由表信息的更新。路由表建立之后，路由器周期性地向外广播其（V，D）表中的内容。例如，假设路由1与路由2是一个自治系统中两个相邻的路由。路由器1接收到路由器2发送的（V，D）报文，路由器按照以下规律进行更新路由表信息：
	- 如果路由器1中的路由表没有这一项记录，则在1中增加该项纪录，同时D加1.
	- 如果路由器1的路由表的该项纪录比路由器2发送的记录距离D减1还要大，则路由器1在路由表中修改该项，距离D值根据路由器2提供的值加1.如下图所示。

![](https://ituku.tk/di/61DA6/class-diagram.png)
<center>**图1.路由表信息更新的工作过程**</center>

**路由选择协议RIP的特点是实现简单，只需要考虑距离，而不考虑连接路由器链路的带宽，但是它不适应大型或路由变化剧烈的互联网环境，对于中小规模的网络是是用的。**

## 内部网关协议（IGP）——OSPF

开放最短路径优先OSPF。
是为了克服RIP的缺点开发出来的。

	名词解释：
	开放：OSPF协议不受厂商的限制
	最短路径优先：指使用了Dijkstra提出的最短路径优先（SPF）算法

与RIP相比较，OSPF协议具有以下特点：
- OSPF主要特征是使用分布式链路状态协议，而RIP使用的是距离向量协议。
- OSPF要求路由器发送的信息是本路由器与哪些路由器相邻，以及链路状态的度量。“度量”主要指：费用、距离、延时、带宽等。
- OSPF链路状态改变时，使用**洪泛法**向所有路由器发送信息。而RIP仅向与自己相邻的路由器发送。
- OSPF协议的路由器频繁交换链路状态信息，因此所有的路由器最终都能建立一个链路状态数据库。这个数据库存储着全网的拓扑信息。
- 为了适应很大规模的网络，OSPF协议要求将一个自治系统再划分为更小的范围，叫做区域。一个区域内的路由数不超过200个。

## 典型外部网关协议BGP

BGP-4采用了路由矢量路由选择协议。

### 基本思想

配置BGP时，每一个自治系统的管理员都要选择至少一个路由器（一般是BGP边界路由器）作为该自治系统的“BGP发言人。”

**BGP发言人**与其他自治系统的发言人交换路由状态信息，如增加了新的路由，或是撤销过时的路由等。

### BGP路由选择协议的工作过程

- 边界路由器初始化。刚运行时，与边界的BGP路由交换整个路由信息。而在此后，只有当发生变化时才更新有变化的部分，而不是像RIP或者OSFP那样定期更新。
- BGP路由选择协议的4种分组
	- 打开分组，与相邻的BGP发言人建立关系
	- **更新分组**，发送某一路由信息，或撤出多条路由
	- 保持分组
	- 通知分组

---

## 路由器的构成

路由器是一种具有多个输入端口和多个输出端口的专用计算机，任务是**转发分组。**

工作模式：从某个输入端口收到的分组， 按照分组要去的目的地，把该分组从路由器的某个合适的输出端口转发给下一跳路由器。下一跳的路由器也按照这种方法处理分组，知道该分组到达终点为止。

路由器分为两个部分：
- 路由选择：控制部分，核心构件是路由选择处理机（根据路由选择协议构造出路由表，和相邻的路由器交换路由信息）
- 分组转发：交换结构（根据转发表对分组进行处理）、一组输入端口和一组输出端口。



## 虚拟专用网络VPN

指明了一些专用地址，这些地址只能用于一个机构的内部通信，而不能用于和因特网上的主机通信。换言之，**专用地址只能用做本地地址而不能用作全球地址。**
在因特网中的所有路由器，对目的地址是专用地址的数据报一律不进行转发。RFC1918指明的专用地址是：

    10.0.0.0到10.255.255.255（或记为10/8，它又称为24位块）
	172.16.0.0到172.31.255.255（或记为172.16/12，它又称为20位块）
	192.168.0.0到192.168.255.255（或记为192.168/16，又称为16位块）

上面的地址相当于一个A类网络、16个连续的B类网络和256个连续的C类网络。

专用IP也称为可重用IP

虚拟专用网络VPN（virtual private Network）

实现：IP隧道技术。

## 网络地址转换NAT

**网络地址转换NAT**（Network Address Translation）需要在专用网连接到因特网的路由器上安装NAT软件。

装有NAT软件的路由器叫做NAT路由器，它至少有一个有效的外部全球IP地址。
这样，所有使用本地地址的主机在和外界通信时，都要在NAT路由器上将其本地地址转换成全球IP地址，才能和因特网连接。

![](https://ituku.tk/di/G482J/163018-ucmb-128568.jpg)

<center>**图2.NAT路由器的工作原理**</center>