---
layout: post
title:  "ArrayList & LinkedList"
categories: Algorithm 
tags: Algorithm Java 数据结构
author: Chester Cheung
---

* content
{:toc}


### ArrayList 动态数组的内部实现原理是什么？

+ ArrayList是List接口的一个可变大小的数组的实现

+ ArrayList的内部是使用一个Object对象数组来存储元素的

+ 初始化ArrayList的时候，可以指定初始化容量的大小，如果不指定，就会使用默认大小，为10


+ 当添加一个新元素的时候，首先会检查容量是否足够添加这个元素，如果够就直接添加，如果不够就进行扩容，扩容为原数组容量的1.5倍

+ 当在index处放置一个元素的时候，会将数组index处右边的元素全部右移

+ 当在index处删除一个元素的时候，会将数组index处右边的元素全部左移，最后一个位置设为null

[ArrayList](https://www.cnblogs.com/lierabbit/p/8383683.html)








### LinkedList内部实现原理是什么？

+ 数据存储是基于双向链表实现的。

+ 插入数据很快。先是在双向链表中找到要插入节点的位置index，找到之后，再插入一个新节点。 双向链表查找index位置的节点时，有一个加速动作：若index < 双向链表长度的1/2，则从前向后查找; 否则，从后向前查找。


+ 删除数据很快。先是在双向链表中找到要插入节点的位置index，找到之后，进行如下操作：node.previous.next = node.next;node.next.previous = node.previous;node = null 。查找节点过程和插入一样。

+ 获取数据很慢，需要从Head节点进行查找。

+ 遍历数据很慢，每次获取数据都需要从头开始。

[LinkedList](https://www.cnblogs.com/maohuidong/p/7965764.html)

### ArrayList和LinkedList的区别

1. ArrayList的实现是基于数组来实现的，LinkedList的基于双向链表来实现。这两个数据结构的逻辑关系是不一样，当然物理存储的方式也会是不一样。 

2. 对于随机访问，ArrayList优于LinkedList，时间复杂度是O(1)

3. 对于插入和删除操作，LinkedList优于ArrayList，因为ArrayList需要进行数据搬移操作

4. LinkedList比ArrayList更占内存，因为LinkedList的节点除了存储数据，还存储了两个引用，一个指向前一个元素，一个指向后一个元素。





