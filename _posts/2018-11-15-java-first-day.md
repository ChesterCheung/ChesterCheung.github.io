﻿---
layout: post
title:  "Java类和对象详解"
date:   2018-11-15 19:41:08
categories: Java
tags: Java IT 
---

* content
{:toc}


今天是学习Java语言的第一天，先要对Java语言有一个简单的认识。Java是一门完全面向对象的编程语言，类和对象是面向对象编程的基础和核心。面向对象的思想来源于现实生活，面向对象编程就是使用面向对象编程思想设计的代码格式来模拟现实生活。


首先，在现实生活中有哪些是对象呢？只要是一个具体得而物体或者一个具体的事物就是一个对象，肉眼所能看到的任何一个物体，你所想象的任何一个物体都是一个对象。







比如，某一个人、某一台电脑……知道了什么是对象后，你又能从哪些方面来描述对象呢？分别有什么内容？

* 对于某一个人：特征：外貌、性格、姓名、籍贯、学历、年龄、爱好、身高……
			行为：读书、吃饭、睡觉、上课、喝水、说话、走路……

* 对于某一台电脑：特征：颜色、大小、电池、内存、显卡、cpu、品牌、型号……

	行为：计算、显示、存储、开机、关机……等等等等


接下来介绍下类的组成。在现实生活中是怎样对对象进行分类的呢？本质上是根据对象相似得而特征和相似的*行为/功能/用途*进行分类的，要注意的是生活中的类都是抽象的。那么程序中的类又是怎样的呢？

程序中的类是现实生活沉重对象的**行为**和**特征**，按照程序中类的固定格式进行的抽象定义，只要由两方面构成：属性(根据对象的特征进行定义)和方法(根据对象的行为进行定义)。
关于Java中类的格式，我们定义的类就是一种数据类型


```java
	public class 类名{

		public 数据类型 属性名；

		public 返回值数据类型 方法名(数据类型 参数名，……) {

		}

	}
```
		
注意：Java中类是模板，是编程的基本单位。
还有实例化对象和调用属性方法的格式
实例化对对象的关键字：new

格式：

```php
	类名 对象名= new 类名();


	调用属性方法的格式：

		对象名.属性名

		对象名.方法名(参数值,……)；

```