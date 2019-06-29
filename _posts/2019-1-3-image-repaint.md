---
layout: post
title:  "图像重绘技巧"
categories: Java
tags:  Java image  
author: Chester Cheung
---

* content
{:toc}

一、图形为什么会消失？


接着上节课学过简易画图板的制作以后，在做出的画图板上，如果细心观察的话不难发现一个问题，就是当改变界面大小(最大化、最小化)时，画板上绘制的图形会全部消失，这是为什么呢？原因是这样的：



图形界面时由容器组件和元素组件构成的，而所有的组件都是采用的C和C++的代码，AWT组件就是通过调用操作系统底层的绘图函数来实现的；SWING组件则是在AWT组件的基础上，采用纯Java语言实现的。总而言之一句话：所有的组件都是画出来的。










我们所绘制图形的数据都存储在内存中，在创建窗体时我们已经定义了窗体的大小，如果我们再次改变窗体大小的时候，原来的窗体就不满足显示的需求。这时候就会自动调用组件的绘制方法，将窗体上所有的组件再重新绘制一次，但是不会执行我们所绘制的图形的代码，所以我们看到的就是绘图板界面还在，但是界面上之前绘制的图形消失了。操作系统重绘窗体上的组件时，调用了paint方法，这个方法是定义在JFrame和JPanel中都有的，叫做



Public void paint (Graphics g) { }



想要改变这个方法，就必须定义一个类继承组件，然后才能重写paint方法。最重要的一点就是，该类必须继承JFrame类或是JPanel类，不然不可以重写paint方法。



二、如何让图形不消失？


那么如何能够让绘制的图形不消失呢？想让图形保留，就要将图形的相关数据记录下来，那么数据又要怎样记录呢？


对于直线：x1,y1,x2,y2,color,type;

对于矩形：x1,y1,width,height,color,type;

对于图片：x1,y1,width,height,Image,type;

对于文字：x1,y1,str,color,font,type;

………



**是否需要把每一种图形都定义一个图形类呢？**

不需要。那样的话需要定义很多很多个图形类，这在现实中不太现实，所以需要寻找另外的方法。

实际上，仅仅需要定义数组来存储图形的数据作为内存，但是图形有很多种类型，20,30,100，每一种类型定义一个数组，数组量就太庞大了，怎么减少数组的数量？有两种比较可行的方法：

1.定义一个图形父类，不同的图形就定义这个类的子类，数组类型就可以使用图形父类的；


2.定义一个类，使用方法重载来解决，数组类型就是该类。
那么在什么时候开始重新画呢？窗体改变大小后会自动调用paint方法，此时需要将paint方法进行重写，在重写的重绘方法中，把存储在数组(内存)中的图形再画一次。

三、具体实现：
重新定义一个Data类用来储存数据，在该类得方法中将绘图的相关数据赋给类的属性，如果有某组特殊的数据，需要对方法进行重载，通过改变参数的类型和个数进行重载；例如，对图片的绘制时，就需要多存放一组ImageIcon数据，此时就要对Data类的方法进行重写。

	public int x1,y1,x2,y2,width,height;

	public String type;
	
public Color color;
	
public Graphics g;
	
public ImageIcon icon;

	

public Data(int x1,int y1,int x2,int y2,Color color,String type) {

		this.x1 = x1;

		this.y1 = y1;
	
	this.x2 = x2;
	
	this.y2 = y2;
	
	this.color = color;
	
	this.type = type;

	}
	
	//对Data方法进行重写，针对有特殊属性的数据

	public Data(int x1,int y1,int width,int height, Color color, String type, ImageIcon icon) {

		this.x1 = x1;

		this.y1 = y1;
	
	this.color = color;
	
	this.width = width;
	
	this.height = height;
	
	this.type = type;
	
	this.icon = icon;

	}	

把数据存放到内存以后，还要完成最后一步，就是在Data类中定义重绘方法，在重写调用重绘方法时，把存储在数组中的图形再画一遍。

	public void paint (Graphics g) {
	
	super.paint(g);
	
	System.out.println("已经进行了重绘");

		
//添加循环语句，将之前所的绘图全部进行重绘，等到输出为空时结束循环
	
		for (int i = 0; i<array.length; i++) {

			Data d = array[i];

			if (d != null)

				d.draw(g);

			else

				break;
	
	}

将原本的Drawing类继承父类JFrame，实例化Drawing对象drawing时，如果在drawing对象中找不到的方法就会自动到父类JFrame中去调用，这时就不用多余的实例化JFrame对象，只需要用this关键字表示drawing对象就能调用JFrame中的方法对窗体界面进行设计了。(重点、难点)


**牢记：**
使用this关键字，当属性名和参数名完全一致时，可以用this关键字来做区分，加上this的表示属性，不加的表示参数。与this类似，super关键字相当于是指向当前对象的父类，这样就可以用super.xxx来引用父类的成员、调用父类的方法。
最后图像重绘完成后，无论如何改变窗体的大小，都不会使绘制的图形消失：


![效果](https://img-blog.csdnimg.cn/20190216145049715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)