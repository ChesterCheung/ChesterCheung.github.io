---
layout: post
title:  "Java视频特效处理/PC版美颜相机"
categories: Java
tags:  Java openCV image
author: Chester Cheung
---

* content
{:toc}


前期get到了一波对指定图片进行处理的操作，但是整体看上去B格不是很高，而且有同学跟我反映说这个根本就没什么高级的，她用手机也一样能做到相同甚至更好的效果，对此我竟无言以对，所以这两天搞了一波更高级的骚操作，就是调用摄像头，对捕捉到的图像进行实时处理，在这个基础上产生不同的效果，这个拿手机应该是做不到的。

首先调用摄像头需要安装几个Java安装包，在这里把地址给大家：









链接：https://pan.baidu.com/s/1pTr1jeVAubqvch33cjtq3Q 密码：paub



下载好Java驱动包之后，把他按照常规操作添加到我们eclipse的环境变量中，preference中先添加后，在工程文件夹的Build Path中添加变量，然后就具备了调用摄像头的初步能力了，我在网上找到了一段代码，发现可以直接搬过来使用，而且一个错都没有报，所以在我自己调试过后也给大家了，对于小白来说copy是比较好上手的一种方式

	public static void main(String[] args) throws InterruptedException {

		//Webcam方法调用摄像头
		Webcam webcam = Webcam.getDefault();
		//设置可见摄像框的大小
		webcam.setViewSize(WebcamResolution.VGA.getSize());
		//调用一个摄像头面板，或者不调用采取其他方式也ok 
		WebcamPanel panel = new WebcamPanel(webcam);
	
		panel.setFPSDisplayed(true);
		panel.setDisplayDebugInfo(true);
		panel.setImageSizeDisplayed(true);
		panel.setMirrored(true);
	
		JFrame window = new JFrame("Test webcam panel");
	
		window.add(panel);
	
		window.setResizable(true);
	
		window.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	
		//在没有加window.pack()前，窗体的默认大小是(0,0)，加上以后窗体大小会自动调整到最佳大小
		window.pack();
	
		window.setVisible(true);
	
	}

我自己在前人的基础上做出了一点修改写出了注释，也方便大家理解，自己也好查阅。这段代码修改完成后，我们就已经可以正常调用摄像头进行调用即时的图像了，在这个基础上可以再把平面图像处理的东西拿过来做些修改，就可以将摄像头拍到的图像进行实时加滤镜了。



我在这里加装了几种简单的效果图，比如模糊，黑白，卡通，X光，镜像等效果，还是相对比较好上手的，毕竟都是小白嘛，在画图方面是有些技巧的，如果处理图片时，单纯的把每个像素点都记录在二维数组中，通过改变像素点的颜色而附加图像效果，在画出的时候调用画笔的g.drawRect方法，会发现图像画的相当慢，这个也是难免的，处理平面图像时慢一些还是可以忍受的，但是视频如果也是这么慢，就未免太难受了，所以必须使用别的方法来画出图像。



我们这个使用BufferedImage图片流的方法，将webcam.getImage()获取到的图像转化成图片流，给图片流赋画笔，进行滤镜效果的修改后，再将BufferedImage画出来，这样通过建立缓冲图层画图，相当于是在系统内先把图像提前画好，在将画好的图片drawImage到我们窗体界面上，这样可以极大的提高画图的效率。

	//绘制模糊效果
	public void draw1() {
		
		BufferedImage image = webcam.getImage();//建立图片缓冲区，把图片画在缓冲区内，然后再画在窗体上比较快
		int w = window.getWidth();
		int h = window.getHeight();
		
		bg = new BufferedImage(w, h, BufferedImage.TYPE_INT_RGB);//实例化一个要画的BufferedImage对象，确定长宽和窗体的大小相同
		pp = bg.getGraphics();//把画布传给实例化的BufferedImage对象，在他的上面画出经过处理后的图片
		g = window.getGraphics();
		
		if(image != null) {
			for (int i = 0;i < w;i+=3) {
    			for (int j = 0;j < h;j+=3) {
    				int pixel = image.getRGB(i, j);
    				Color color = new Color(pixel);
        			
        				pp.setColor(color);//在每一个像素点设置画布的颜色并画出相应颜色的一个像素点
        				pp.drawRect(i, j, 20, 20);
        			
    			}

    		}
			g.drawImage(bg, 0, 60, null);
		}
		Thread.yield();
	}
	
	//绘制X光效果
	public void draw2() {
		
		BufferedImage image = webcam.getImage();
		int w = window.getWidth();
		int h = window.getHeight();
		
		bg = new BufferedImage(w, h, BufferedImage.TYPE_INT_RGB);
		pp = bg.getGraphics();
		g = window.getGraphics();
		
		if(image != null) {
			for (int i = 0;i < w;i++) {
    			for (int j = 0;j < h;j++) {
    				int pixel = image.getRGB(i, j);
    				pixel = 16777216 - pixel;
    				Color color = new Color(pixel);
        			
        				pp.setColor(color);
        				pp.drawRect(i, j, 1, 1);
        			
    			}
	
    		}
			g.drawImage(bg, 0, 60, null);
		}
		Thread.yield();
	}
	
	//绘制卡通效果
	public void draw3() {
		
		BufferedImage image = webcam.getImage();
		int w = window.getWidth();
		int h = window.getHeight();
		
		bg = new BufferedImage(w, h, BufferedImage.TYPE_INT_RGB);
		pp = bg.getGraphics();
		g = window.getGraphics();
		
		if(image != null) {
			for (int i = 0;i < w;i++) {
    			for (int j = 0;j < h;j++) {
    				int pixel = image.getRGB(i, j);
    				Color color = new Color(pixel);
        			int colorred = (int)color.getRed();
        			int colorgreen = (int)color.getGreen();
        			int colorblue = (int)color.getBlue();	        		
        			if ((colorred+colorgreen+colorblue)/3 > 100) {

        				Color colo = new Color(255,255,255);
        				pp.setColor(colo);
        				pp.drawRect(i, j, 1, 1);
        			} else {
        			
        				Color colo=new Color(0,0,0);
        				pp.setColor(colo);
        				pp.drawRect(i, j, 1, 1);
        			}
    			
    			}
	
    		}
		g.drawImage(bg, 0, 60, null);
		
		}
		Thread.yield();
	}

	//绘制黑白效果
	public void draw4() {
		BufferedImage image = webcam.getImage();
		int w = window.getWidth();
		int h = window.getHeight();
	
		bg = new BufferedImage(w, h, BufferedImage.TYPE_INT_RGB);
		pp = bg.getGraphics();
		g = window.getGraphics();
		
		if(image != null) {
			for (int i = 0;i < w;i++) {
    			for (int j = 0;j < h;j++) {
    				int pixel = image.getRGB(i, j);
    				Color color = new Color(pixel);
    				int colorred = (int) (color.getRed() * 0.3);
        				int colorgreen = (int)(color.getGreen() * 0.59);
        				int colorblue = (int)(color.getBlue() * 0.11);
    				int gray = colorred + colorgreen + colorblue;
        				Color colo=new Color(gray,gray,gray);
        				pp.setColor(colo);
        				pp.drawRect(i, j, 1, 1);
        		
    			}
    		}
		g.drawImage(bg, 0, 60, null);
		}
	
		Thread.yield();
	}

	//绘制镜像效果
	public void draw5() {
		BufferedImage image = webcam.getImage();
		int w = window.getWidth();
		int h = window.getHeight();
		
		bg = new BufferedImage(w/2, h, BufferedImage.TYPE_INT_RGB);
		pp = bg.getGraphics();
		g = window.getGraphics();
		
		if(image != null) {
			for (int i = 0;i < w/2;i++) {
    			for (int j = 0;j < h;j++) {
    				int pixel = image.getRGB(i, j);
    				Color color = new Color(pixel);
        				pp.setColor(color);
        				pp.drawRect(i, j, 1, 1);
        		
    			}
    		}
		g.drawImage(bg, 0, 60, null);
		}
		if(image != null) {
			for (int i = w/2;i > 0;i--) {
    			for (int j = 0;j < h;j++) {
    				int pixel = image.getRGB(i, j);
    				Color color = new Color(pixel);
    				pp.setColor(color);
        			pp.drawRect(w/2-i, j, 1, 1);
    			}
    		}
		g.drawImage(bg, w/2, 60, null);
		}
	
		Thread.yield();
	}

## 还要说明一点，这里的Thread.yield()方法的作用。



因为系统每一时刻所能运行的线程数目是有限的，而电脑上的上千个线程都是轮流调用的，因此如果不加上这句话的话，Java的线程就会占用电脑的线程而不允许其他线程使用，会造成电脑十分卡且运行缓慢，加上Thread.yield()后，程序执行完这段代码后，就会自动让出占用的线程，提供给其他线程使用，在需要时进行再次调用。