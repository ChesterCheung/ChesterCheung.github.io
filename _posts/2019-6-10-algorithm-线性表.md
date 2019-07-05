---
layout: post
title:  "线性表"
categories: algorithm
tags: algorithm 线性表
author: Chester Cheung
---

* content
{:toc}

> ## 一、定义


线性表是由同一个数据类型的数据元素a1,a2,a3…组成的有限序列，除了第一个元素没有前驱元素和最后一个原说说没有后继元素外，其他元素有且只有一个直接元素和一个直接后继元素。



n为线性表的长度，是线性表中包含元素的个数；长度为0的线性表为空表，不包含任何元素。对于非空的线性表，每个元素早表中都有一个确切的位置，即数组元素ai在表中的位置只取决于数据元素本身的序号i。










在逻辑上，线性结构的特点是数据元素之间存在着“一对一”的逻辑关系，这种关系的数据结构通常称为线性结构；同样，任何一个线性结构都可以用线性表表示出来，只需要按照他们逻辑的顺序将它们进行排列即可。



> ## 二、线性表的顺序存储结构


计算机内部可以采取不同的方式存储一个线性表，其中最简单的方式就是用一组地址连续的存储单元来依次存储线性表中的数组元素。这种存储结构称为线性表的**顺序存储结构**，此时的线性表称为**顺序表**。



线性表中所有数据元素都是同一个类型，所以占用的存储空间也大小相同，假设每一个数据元素所占的k个存储单元，那么线性表的第i+1个数据元素ai+1的存储位置和第i个数据元素ai的存储位置之间存在以下关系：

	LOC(ai+1) = LOC(ai) + k

从上式中可以看出，它是用数据元素在机内物理位置上的相邻关系来映射数据元素在逻辑上的相邻关系，即每个数据元素的存储位置与线性表的首地址之间相差一个和数据元素在表中的序号成正比的常数。


由此可见，确定了首地址，线性表中的任何一个数据元素都可以随机存取，因此可以称线性表的顺序存储结构为一种随机存取的存储结构。

![1](https://img-blog.csdnimg.cn/201906101643415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

## 优点：


1.只需存放数据元素本身的信息，无需其他额外的空间开销，存储密度大，空间利用率高


2.构造原理简单直观，已知首地址后，表中的任何一个数据都能用简单、直观的解析式计算出来


3.对于表中的任何数据元素，既可以顺序访问，也可以随机访问，且时间代价是相同的。


## 缺点：


1.需要一片地址连续的存储单元作为线性表的存储空间


2.存储空间的分配需要提前进行，而且需要按照最大需要空间来进行分配，可能导致存储空间开销的浪费；如果估计过小，空间容量的扩充通常比较困难。


3.进行插入、删除等操作时，需要对插入删除等操作后面的所有数据元素进行移动，这样操作的时间效率低。



## 常见操作的实现

插入


1.将线性表的第i个位置的数据元素到第n个之间的所有元素依次向后移动一个位置


2.将新的数据元素item插入到第i个位置上


3.修改线性表的长度为n + 1

	void INSERTLIST(Elem A[ ], int &n, int i, ElemType item)
	{
		int j;
		if (n == MaxSize || i < 1 || i > n)
			ERRORMESSAGE("线性表满或插入位置不正确！")；
		for(j = n - 1; j > i - 1; j --)
			A[j + 1] = A[j];
		A[i - 1] = item;
		n++;
	}

算法的时间复杂度为O(n)；

## 删除
1.将表的第i个数据元素至第n个数据元素依次向前移动一个位置
2.修改线性表的长度为n - 1。

	void DELETELIST(ElemType A[ ], int &n ,int i) {
		int j;
		if (i < 1 || i > n)
			ERRORMESSAGE("表空或删除位置不正确！")；
		for (j = i; j < n; j ++)
			A[j - 1] = A[j];
		n--;
	}

算法的平均时间复杂度为O(n)；

## 确定某元素的位置

	int LOCATE(ElemType A[ ], int n, ElemType item) {
		for (i = 0; i < n; i++)
			if (A[i] == item)
				return i + 1;
		return -1;
	}

算法的时间复杂度为O(n)；

## 删除表中的重复元素

	void PURGE(ElemType A[ ] , int n) {
		int i = 0, j;
		while (i < n){
			j = i +1;
			while( j < n){
				if (A[j] == A[i])
					DELETELIST(A, n, j + 1);
				else
					j ++;
			i ++;
		}
	}

算法的时间复杂度为O(n^2)；



> ## 三、线性链表

前面看到了线型表在顺序存储结构下的很多缺点和不足，为了克服这些问题，我们研究另外一种存储结构——链式存储结构。这种结构对于**在逻辑上相邻的数据元素，不要求在物理位置上也必须相邻**，仅仅通过指针来映射数据元素之间的逻辑关系，所以他克服了顺序表的某些不足，但是也失去了顺序表可以随机存取的特点。



链式存储结构不仅可以存储线性表，也可以用来存储许多非线性的数据结构，如树和图等。



线性链表是使用一组地址任意的存储单元(可连续可不连续)，依次存储线性表中的各个元素，对于每一个数据元素来说，除了需要存储自身的数据信息之外，还需要存储一个直接后继元素位置的信息，这两部分信息组成了一个数据元素的存储结构，成为一个链结点。

![2](https://img-blog.csdnimg.cn/2019061019300026.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

整个链表有一个叫做外指针或头指针的变量来指出，是链表的入口，当链表为空时，有list为null。有一点需要注意：**虽然链表中的各个链结点的存储空间地址不需要连续，但是每一个链结点内部的一系列存储单元在地址上是必须连续的。**

### 因此，指针本质上是逻辑关系的一个映射


## 产生链结点的方法：


1.调用系统中已有的动态分配过程或者函数，由系统统一分配链结点的空间(动态链表)


2.利用程序中已经声明的数组的数组元素来产生链结点(静态链表)



## 和顺序表对比：


1.对线性表进行插入删除操作时，只需要修改相关链结点的指针地址就ok，方便、省时、快捷；


2.但是每一个链结点除了数据域外，还设有一个指针域，从这一点说，链式存储结构的存储空间开销比顺序结构要付出的代价大



所以，本质上链表是牺牲空间、来节约时间



## 常见操作的实现



**1.建立线型链表**

	LinkList CREAT(int n) {
		LinkList p, r, list = null;
		ElemType a;
		int i;
		for (i = 1; i <= n; i++) {
			READ(a);
			p = (LinkList)malloc(sizeof(LNode));
			p->data = a;
			p->link = null;
			if (list == null){
				list = p;
			else
				r - >link = p;
			r = p;
		}
		return (list);
	}

**2.求线性链表的长度**

	int LENGTH(LinkList list){
		LinkList p = list;
		int n = 0;
		while(p != null) {
			n++;
			p = p->link;
		}
		return n;
	}

或者我们有种时间复杂度更低的算法，依靠递归来实现其功能

	int LENGTH(LinkList list) {
		if (list != null) 
			return 1 + LENGTH(list -> link);
		else
			return 0;
	}

**3.测试线性表是否为空表**

	int ISEMPTY(LinkList list) {
		return list->link == null;
	}

此算法的时间复杂度与链表的长度无关，所以时间复杂度为O(1)；

**4.确定item在线性表中的位置**

	LinkList FIND(LinkList list, ElemType item) {
		LinkList p = list;
		while(p != null && p->data != item) 
			p = p->link;
		return p;
	}

算法的时间复杂度为O(1)；

**5.非空线性链表的第一个节点前插入链结点**

	void INSERTLINK(LinkList &list, ElemType item) {
		LinkList p;
		p = (LinkList)malloc(sizeof(LNode));
		p->data = item;
		p->link = list;
		list =  p;
	}

算法的时间复杂度为O(1)，与链表的长度无关，下图展示出了整个过程的逻辑思路

![3](https://img-blog.csdnimg.cn/20190612090429593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

**6.在非空线性链表的末尾插入一个链结点**

	void INSERTLINK(LinkList list, ElemType item) {
		LinkList p,r;
		r = list;
		while(r->link != null)
			r = r->link;
		p = (LinkList)malloc(sizaof(LNode));
		p ->data = item;
		p->link = null;
		r->link = p;
	}

**7.在线性链表中某个节点的后面插入一个链结点**

	void INSERTLINK(LinkList &list, LinkList q, ElemType item) {
		p = (LinkList)malloc(sizeof(LNode));
		p->data = item;
		if (list == null) {
			list = p;
			p->link = null;
		}
		else{
			p->link = q->link;
			q->link = p;
		}
	}

**8.在线性链表中第i个链结点后插入链结点**

这里的i不是节点的地址，而只是一个序号，我们需要先从第一个节点出发，找到第i个节点，再将新的链结点插在后面。

	int INSERTLINK(LinkList list, int i, ElemType item) {
		int j;
		j = 1;
		while(j < i && q != null) {
			q = q->link;
			j++;
		}
		if (j != i || q != null) {
			ERRORMESSAGE("链表中不存在第i个节点")；
			return -1;
		}
		p = (LinkList) malloc(sizeof(LNlde));
		p-> data = item;
		p->link = q-link;
		q->link=p;
		return 1;
	}

**9.按值有序链接的线性链表中插入链结点**

	void INSERTLINK(LinkList &list, ElemType item) {
		LinkList p,q,r;
		p = (LinkList)malloc(sizeof(LNode));
		p->data = item;
		if (list == null || item < list -> data) {
			p->link = list;
			list = p;
		}
		else {
			q = list;
			while(q != null && item >= q->data) {
				r = q;
				q = q->link;
			}
			p->link = q;
			r->link = p;
		}
	}

**10.从非空线性表中删除q所指向的链结点**

由于我们不知道被删除链结点的直接前驱结点，所以需要添加一步操作，就是寻找前驱链结点的过程，这个过程相对较容易，只要先将r指向链表的第一个链结点，然后反复执行r = r->link 直到r -> link == q，算法实现如下：

	void DELETELINK(LinkList &list, LinkList q) {
		LinkList r;
		if (q == list) {
			list = q->link;
			free(q);
		}
		else {
			r = list;
			while((r->link != q) && (r->link != null))
				r = r->link;
			if (r->link != null) {
				r->link = q->link;
				free(q);
			}
		}
	}

后续还会继续补充其他关于线性表的应用......