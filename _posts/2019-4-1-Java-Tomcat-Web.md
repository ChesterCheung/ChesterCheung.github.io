---
layout: post
title:  "JavaWeb开发之Tomcat"
categories: Java
tags:  Java Web Tomcat
author: Chester Cheung
---

* content
{:toc}

## Web开发相关基础知识



web在英文中是website网站的意思，用于表示Internet主机上供外界访问的资源，而供外接访问的资源分为以下两种：



> 1.静态web资源（html界面）：web页面中人们浏览的数据始终是不变的


> 2.动态web资源：web页面中供人们看到的数据是由程序产生的，不同时间截点访问web看到的内容不同，会用到JSP/Servlet、ASP、PHP等。

![1](https://img-blog.csdnimg.cn/20190330023519342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)








在Java中动态web资源的开发技术称为javaweb，学习的重难点就是就是使用Java技术开发动态web页面。学习web开发时，需要先安装一台web服务器，然后在web服务器中学习开发相应的web资源，以供用户在使用浏览器时访问。



在小型的应用系统或有特殊需要的系统中，可以使用免费的web服务器Tomcat，这个服务器支持JSP以及Servlet规范，它本身是一个优秀的Servlet容器，完全由Java语言编写。Tomcat本身由一系列可配置的组件构成，核心是Servlet容器组件，他是所有其他Tomcat容器组件的顶级容器，简单介绍下Tomcat的组成结构：

![2](https://img-blog.csdnimg.cn/20190330023403107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

```php
	<Server>代表整个Servlet容器组件，可包含多个元素
	<Service>包含一个元素或多个,这些Connector共享一个。
	<Connector/>接收客户请求，向用户返回响应
	<Engine>每个只能包含一个元素，他处理在同一个中所有的接收到的用户的请求
	<Host>每个中可以包含多个它代表一个虚拟主机(网站)，他可以包含一个或多个web应用
	<Context/>使用最频繁的元素，代表了在虚拟主机上的单个web应用
```


![3](https://img-blog.csdnimg.cn/20190330023445748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)



如果有两家公司的web应用都发布在同一个Tomcat服务器上，可以为两家公司分别创建一个虚拟主机，尽管这两个服务器位于同一个主机，但是当客户端通过以上两个不同的虚拟主机名访问web应用时，会发现这两个应用分别拥有独立的主机，如需在web服务器中配置一个网站，需要使用Host元素进行配置，比如

```php
	<Host name = "site1" appBase = "c:\\app"></Host>
```

配置的主机(网站)要想在外部被访问，必须在DNS服务器或windows系统中注册。

## 用Tomcat的管理平台还可以管理web应用的声明周期