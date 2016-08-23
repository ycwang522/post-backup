---
title: 链表的增加及删除结点
date: 2016-07-24 10:52:36
categories: 数据结构
tags:
	- 链表
---


### 概述

链表是一种动态数据结构，是因为链表在创建时，长度为止。

每当插入一个新的结点时，只需要为当前新插入的结点分配内存，然后调整指针的指向来确保新结点被链接到链表当中。

内存分配不是创建链表的时候一次性分配完毕，而是每添加一个结点分配一个结点所需的内存空间。

由于没有闲置的空间，所以链表的空间效率比数组高。

单链表可用如下的方式定义结点：

```c++
struct ListNode
{
	int			m_nValue;
  	ListNode*	m_pNext;
}
```

---

<!-- more -->

### 单链表的插入操作

向该单链表中添加一个结点的C代码如下：

```c++
void AddToTail(ListNode** pHead, int value)
{
	ListNode *pNew = new ListNode();
  	pNew->m_nValue = value;
  	pNew->m_pNext = NULL;
  
  	if(*pHead == NULL)
    	{
  		*pHead = pNew;
	}  
  	else
    	{
  		ListNode *pNode = *pHead;
      	
	      	while(pNode->m_pNext != NULL)
	      {
	  		pNode = pNode->m_pNext;
		}
	      	pNode->m_pNext = pNew;
	}
}
```

---

注*：函数的第一个参数pHead是一个二级指针。往一个空链表中插入一个结点时，新插入的结点即为链表的头指针。

由于此时会改动头指针，因此必须把pHead参数设为指向指针的指针，否则出了这个函数pHead仍然是一个空指针。

如果想在链表中找到它的第i个结点，也只能从头结点开始，沿着指向下一个结点的指针遍历链表，时间效率为O(n)。

以下代码是在链表中找到第一个含有某值的结点并删除该结点的代码

---

### 单链表的删除操作

```c++
void RemoveNode(ListNode** pHead, int value)
{
	if(pHead == NULL || *pHead == NULL)
      return;
  	
  	ListNode* pToBeDeleted = NULL;
  	
  	if((*pHead)->m_nValue == value)
    	{
  		pToBeDeleted = *pHead;
      		*pHead = (*pHead)->m_pNext;
	}
  	else
   	 {
  		ListNode* pNode = *pHead;
      	while((*pNode)->m_pNext != NULL && (*pNode)-> m_pNext->m_nValue != NULL)
        {
  		pNode = pNode->m_pNext;
	}
      	
      	if((*pNode)->m_pNext != NULL && (*pNode)-> m_pNext->m_nValue == value)
        {
  		pToBeDeleted = pNode->m_pNext;
          	pNode->m_pNext = pNode->m_pNext->m_pNext;
	}      
	}
  	
  	if(pToBeDeleted != NULL)
    	{
  		delete pToBeDeleted;
      		pToBeDeleted = NULL;
	}
}
```

---


