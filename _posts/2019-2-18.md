---
layout: post
title:  "图像重绘加强版"
categories: Java
tags:  Java image
author: Chester Cheung
---

* content
{:toc}

之前学习画图板时我们已经学习了如何对画图板进行重绘，也已经知道了当改变界面大小(最大化、最小化)时，画板上绘制的图形会全部消失，原因是由于：



图形界面时由容器组件和元素组件构成的，而所有的组件都是采用的C和C++的代码，AWT组件就是通过调用操作系统底层的绘图函数来实现的；SWING组件则是在基于AWT组件的基础上，采用纯Java语言实现的。当改变窗体大小时，Java会调用组件的绘制方法，根据新的大小将组建重新画一次，绘制组件的方法叫paint(Graphics g)。



**对于轻量级组件**：SWING组件只有顶级容器组件中才会有paint方法的实现

**对于重量级组件**：AWT组件中每一个组件都有一个paint的方法的实现。



但是在组建进行重绘时，并没有把你之前画过的图像重新再画一次，所以要再画一次：



首先继承JFrame类，重写paint(Graphics g)方法，使用super调用父类中的方法代码，再添加需要的新的代码；其次，图形要储存在链表、向量、数组、堆、栈、二叉树、哈希……且每个图形有多个数据，数据的类型也不一定相同，这该如何处理？按照上节课学习的内容，要定义一个图形父类，因为类的属性可以有不同类型的数据，每一个图形就是一个图形对象，然后再重绘方法中，遍历数据结构中的内容，获取图形对象，根据对象的数据，将图形重新画一次。



但今天，我们选择不同的的方式来进行图像重绘，具体操作方案如下：



通过把界面上之前画的像素存储起来，在重绘方法时再画出，进而实现图像重绘。在存储像素时，把窗体中的内容转换为一个BufferedImage的图形对象 (在使用时需要重写BufferedImage中的getImage方法)，调用Robot中的robot.createScreenCapture方法获取像素点；重新定义一个数组getRGBarrys，用它来存放像素点数据，再重新定义一个Capture方法，里面执行获取像素并存放进入数组中去，具体代码如下:

	public BufferedImage getImage() {

		// TODO Auto-generated method stub

		return image;
	
}
	
public int [][] getRGBarrays () {
	
	// TODO Auto-generated method stub

		return arrays;

	}
	
public void Capture() {
	
	Rectangle rect = new Rectangle(0,0,frame.getHeight(),frame.getWidth());
	
		image = robot.createScreenCapture(rect);

		arrays = new int [image.getHeight()][image.getWidth()];

		for(a = 0; a <image.getHeight(); a++) {
	
			for(b = 0; b< image.getWidth(); b++) {

				arrays[a][b] = image.getRGB(a, b);
			
		}

		}
	
}

同时还要，继承JFrame类，在Drawing的主函数中重写paint(Graphics g)方法，别忘记使用super调用父类中的代码，然后再添加新的代码。

	public void paint(Graphics g) {

		super.paint(g);

		BufferedImage image = dl.getImage();
	
	if (image != null) {

			g.drawImage(image, 0, 0, this);

		}

	
	int arrays[][] = dl.getRGBarrays();

		
if (arrays != null) {

			for(a = 0; a <arrays.length; a++) {

				for(b = 0; b< arrays[a].length; b++) {

					Color color = new Color (arrays[a][b]);
	
				g.setColor(color);

					g.drawLine(a, b, a, b);				
		}

			}

		}

	

}

但这种方法实现重绘有一个缺点，或者说还未实现的一个问题，就是：

Drawing在调用方法重绘时，将界面上的所有内容都进行了重绘，即从电脑屏幕左上角开始进行重绘，会画上很多没有用的信息，暂时不能从画图界面的左上角开始重绘，这个问题仍然有待解决，解决后会在本博客上面进行补充说明，如果大家谁有好的解决方法，也希望能提出您的宝贵意见，大家互相交流经验。