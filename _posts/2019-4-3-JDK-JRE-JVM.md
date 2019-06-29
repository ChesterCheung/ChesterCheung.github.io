---
layout: post
title:  "快速弄懂 JDK/JRE/JVM 之间的区别与联系"
categories: Java
tags:  Java JDK JRE JVM
author: Chester Cheung
---

* content
{:toc}


## JDK/JRE/JVM的区别和联系

首先看张图，这张图清晰简明的总结出了JDK/JRE/JVM之间的关系：

![1](https://img-blog.csdnimg.cn/2019033017450637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

## JDK介绍



JDK，（Java Development Kit)，是针对Java开发员的产品，是Java开发工具包，也是整个Java开发的核心，其下包括了JRE、Java工具和Java基础类库。



JDK中包括JRE，在JDK的安装目录下有一个名为JRE的目录，里面有两个文件夹bin和lib，可以简单的认为bin里面的就是JVM，lib中的则是JVM工作所需要的类库，而JVM和类库lib加起来就是我们说的JRE。



JDK是整个Java的核心，包括了Java运行的环境JRE，很多的Java工具(javac/java/jdb)和Java基础的类库(jar等)。



## JRE介绍



JRE，(Java Runtime Environment)，是运行基于Java语言编写的程序的不可或缺的环境，通过JRE，Java的开发者可以讲自己开发的环境发布到用户手中，让用户使用。



JRE中包括了JVM，以及其他的Java工具，这些都是运行Java程序的必要组件。要注意的是，JRE不是Java的开发环境，而是Java运行环境，其中并没有包括任何开发工具，，只是针对使用Java的用户而适用的。



## JVM介绍



JVM，(Java virtual machine)，他是整个Java实现跨平台的最核心的部分，所有的Java环境会首先被javac.exe编译工具编译成.class的类文件，这种文件可以在JVM虚拟机上运行。



也就是class并不能直接与机器的操作系统相对应，而是通过JVM虚拟机而间接与操作系统交互，有虚拟机讲程序解释给本地系统执行。但是只有JVM还不能生成class的执行文件，因为在解释class的时候JVM还需要调用解释所需要的类库lib，而JRE中包含有lib类库。