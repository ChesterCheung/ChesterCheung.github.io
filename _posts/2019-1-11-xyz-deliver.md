---
layout: post
title:  "参数传递规则"
categories: Java
tags:  Java 数据结构
author: Chester Cheung
---

* content
{:toc}


## 一、参数传递的分类。

最近学习了Java中的一个重要的基础内容，就是参数传递。参数传递在很多时候都有着十分重要的作用，首先先来看一下Java的相关数据类型（需要将基本数据类型和引用类型都牢记）：



1.**基本数据类型：
**

（每一种基本类型都对应有一个封装类）

byte、short、int、long、boolean、float、double、char一共8种

Byte、Short、Integer、Long、Boolean、Float、Double、Character



2.**引用类型：类、抽象类、接口
**
Class、abstract class、interface

不管是你自己定义的类，还是Java给你提供的类，本质上都是属于引用类型，他们本身都继承有一个共同的父类。









因此Java中的所有类(class、abstract class)都有一个共同的父类：Object。就算定义的某个类已经继承了一个父类，但只要他的父类没有继承的父类，就会默认继承Object父类。正因为这样，可以说所有的类都直接或间接的继承有父类Object。


举个简单的例子：

	Public claa UNStudent extends Student {
	}
	Public class Student extends People {
	}
	Public class People {
	}

在这个事例中，表面上看，UNStudent类继承Student类，Student类继承People类，而People类没有继承的类；但实际上，UNStudent的父类是Student，Student的父类是People，People的父类是Object，同时，在整个继承链上，所有的子类都可以称为是Object父类的子类。



## 二、值传递


值传递适用于Java的基本数据类型8种情况。除此以外，有一个特殊的类——String，他本身是一个类，但在这种传参的情况下和基本数据类型是一致的。


举一个形象的例子：拿身份证去复印，复印后，得到身份证+复印件，此时如果把复印件上内容改掉，和原件当然没关系；如果把原件上的内容改掉，复印件上的内容可能会变也可能不变，在复印之前改就会变，在复印之后改就不会变。


这个例子就说明了，把变量中的数据拷贝一份，传入到另一个变量中，这时候就是两个独立的变量了，我们对任何一个进行修改，对另外一个不会产生影响。

	public class Test {
	

	public static void main(String[] args) {

			int a = 5;
		
    	fun(a);
		
    	System.out.println(a);

		 }


		Public  static void fun(int a) {

			a += 1;
		
}
	}


输出的结果是5

## 三、引用传递


引用传递适用于Java的引用数据类型。除此以外，需要格外注意的是，数组也是属于引用数据类型。简单两个例子，


int a = new int [10];

int b = new int [] {1,2,2,3,3};



### 注意，虽然String也是类，本应该属于引用传递，但是String类除外。



对于引用传递来说，对象名中存储的是对象在堆内存中的首地址，而不是具体的值；传递时就是把对象名中的首地址，拷贝一份传入到另一个对象名(变量)中，此时两个或者n多个对象名中存储的地址就是一致的，不管用任意的哪一个对象名去修改他所指向对象的属性或方法，其他的对象名也会跟着改变，因为他们通过地址指向的变量是同一个，而不像值传递时改变一个对其余的对象没有影响。


	Public class Test {


		Public static void main(String args[] ) {


		Student stu1 = new Student("小朋友", 60);

		Student stu2 = new Student();

		stu2.name = "初中生";

		stu2.score = 80;


		System.out.println(stu1.toString());
		System.out.println(stu2.toString());


		stu2 = stu1;

		stu1.name = "张三";


		System.out.println(stu1.toString());

		System.out.println(stu2.toString());

		}
	
}

对相关内容做一个**注释**：Student stu1:声明一个对象名，叫做stu1
= : 把内存空间的首地址赋给stu1
；
new : 不是用来定义一个新的Student类对象，而是开辟内存空间，内存空间的大小根据Student类来决定

Student(“小朋友”,60)：将Student类中的属性和方法，写入到内存中，把”小朋友”,60数值赋给属性，在内存中形成一个对象。
