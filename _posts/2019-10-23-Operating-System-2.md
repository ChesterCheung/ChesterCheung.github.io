---
layout: post
title:  "Operating System 2"
categories: OS
tags: OS Linux
author: Chester Cheung
---

* content
{:toc}


## 一、进程的定义、组成、组织、特征

#### 进程的定义

1. 进程是程序的一次执行过程

2. 进程是具有独立功能的程序在数据集合上运行的过程，**是系统进行调度和资源分配的独立单元**

> 进程实体 = 程序段 + 数据段 + PCB(进程控制块)

**注意区分**：进程和进程实体：进程是动态的，进程实体是静态的

![OS-2](https://zhychestercheung.github.io/photos/OS-2.png)







#### PCB(进程管理块):

为了描述控制进程的运行，系统中存放进程的管理和控制信息的数据结构称为进程控制块（Process Control Block）。它是进程实体的一部分，**1.是操作系统中最重要的记录性数据结构**。它是**2.进程管理和控制的最重要的数据结构**，每一个进程均有一个PCB，在创建进程时，建立PCB，伴随进程运行的全过程，直到进程撤消而撤消。所谓的创建进程和撤销进程，都是指对 PCB 的操作。

程序段: 存放执行的代码；

数据段: 存放程序运行过程中处理的各种数据；

#### Question: 

> 进程控制块的作用是什么？PCB中应包括哪些信息？

进程控制块的作用是：进程控制块用于保存每个进程和资源的相关信息，包括进程标识、空间、运行状态、资源等信息。以便于操作系统管理和控制进程和资源。
PCB中应包括：1、进程标识信息：本进程的标识、父进程的标识、进程所属用户的标识。2、处理机状态信息。保存进程的运行现场信息，包括用户可用寄存器的信息；控制和状态寄存器的信息；栈指针。

#### 进程的组织方式

是指多个进程之间的组织形式

1. 链接方式：
(1)按照进程状态将PCB划分为多个队列
(2)OS持有指向各个队列的指针

2. 索引方式：
(1)根据进程的状态，建立几张索引表
(2)OS持有指向各个索引表的指针

**一个系统中，进程成百上千，必须选择合适的合适的方式进行有效的管理。**

#### 进程的特征

![OS-4](https://zhychestercheung.github.io/photos/OS-4.png)

### 对这块小结下：

![OS-5](https://zhychestercheung.github.io/photos/OS-5.png)

## 二、进程的状态与转换

#### 进程的5种状态

**3种基本状态：**

1. 运行态：占有CPU，并正在运行的进程

2. 就绪态：已经分配有运行的资源和条件，可以说：“**万事俱备，只欠CPU**”

3. 阻塞态：等待资源的分配，这里不考虑CPU时间的分配，反正暂时不能运行

**2种过程中的状态：**

4. 创建态：进程正在创建，OS为其分配资源，初始化PCB

5. 终止态：进程正在撤销，OS回收其资源，撤销PC（或者是由于bug导致进程无法继续执行，需进行撤销）

![OS-6](https://zhychestercheung.github.io/photos/OS-6.png)

> 注意以下内容:

1. 只有就绪态和运行态可以相互转换，其它的都是单向转换。就绪状态的进程通过调度算法从而获得 CPU 时间，转为运行状态；

2. 而运行状态的进程，在分配给它的 CPU 时间片用完之后就会转为就绪状态，等待下一次调度。阻塞状态是缺少需要的资源从而由运行状态转换而来，但是该资源不包括CPU 时间，缺少 CPU 时间会从运行态转换为就绪态；

3. 进程只能自己阻塞自己，因为只有进程自身才知道何时需要等待某种事件的发生；

### 本章小节：

![OS-7](https://zhychestercheung.github.io/photos/OS-7.png)

#### $$ Question：进程创建的主要工作是什么？

1. 接收进程运行现场初始值，初始优先级、执行程序描述，其它资源等参数。
2. 请求分配进程描述块PCB空间，得到一个内部数字进程标识。 
3. 用从父进程传来的参数初始化PCB表。
4. 产生描述进程空间的数据结构，初始化进程空间，建立程序段，数据段、栈段等。
5. 用进程运行现场初始值设置处理机现场保护区；造一个进程运行栈帧。
6. 置好父进程等关系域，同时将进程置成就绪状态。 
7. 将PCB表挂入就绪队列，等待时机被调度运行

## 进程控制(即对进程进行转换)

对系统中的进程进行有效管理，**创建、撤销、状态切换**，通过就绪队列和阻塞队列来实现状态切换

#### 如何实现进程切换？

**原语的特点：**

1. 操作原子性、运行时间短、调用频繁

2. 运行在核心态，权限内非常大、属于特权指令

3. 位于操作系统最底层，最接近硬件的部分

> 实现并发性-->产生中断处理(CPU从用户态切换到核心态)-->关中断指令-->原语代码-->开中断指令-->继续中断处理

**一气呵成，不可分割**

**原语的任务**无外乎3点

1. 更新PCB中的信息，包括进程状态标志、保存运行环境到PCB、从PCB恢复环境等
所有进程的控制原语一定修改进程状态标志，抢夺CPU的使用权，必定保存其运行环境，其进程开始运行前必定恢复运行环境

2. 将PCB插入合适的队列（就绪、阻塞队列）

3. 分配回收合适的资源

> 进程的终止

1. 从PCB中找到终止进程

2. 若程序在运行，立即剥夺CPU，将CPU的时间分配给其他进程

3. 终止其所有子进程

4. 将该进程的所有资源归还给父进程或OS

5. 删除该PCB

> 进程的阻塞和唤（状态切换）

**阻塞和唤醒原语必须成对使用**

1. 阻塞原语：
a. 找到阻塞进程对应的PCB
b. 保持进程环境，PCB信息修改为“阻塞态”，暂停进程的运行
c. 将PCB插入相应事件的等待队列

2. 唤醒原语：
a. 在事件等待队列中找到PCB
b. 将PCB从等待队列中移除，设为“就绪态”
c. 将PCB插入就绪队列，等待被CPU调度

**进程的切换**

1. 将运行环境存入PCB

2. PCB移入相应的队列中

3. 选择另一个进程执行，更新其PCB

4. 根据PCB恢复新进程的运行环境

