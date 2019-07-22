---
layout: post
title:  "Mysql经典高逼格/命令行操作"
categories: Mysql
tags:  database Mysql
author: Chester Cheung
---

* content
{:toc}


由于要学习搭建服务器和数据库，所以最近开始自学sql语言了，至于写数据库就用比较基础的Mysql数据库了，虽然Mysql已经被互联网公司所淘汰掉了，他们都在使用Nosql，SQL server等sql语言，最终仍然决定从基础入手。经过简单的决定之后，就用逼格极高的cmd命令行来写了。Mysql数据库的安装方法这里就不给出详细的教程了，网上有好多安装教程可以自行选择安装。






> 1.第一步，我们要先在Mysql中建立一个库



以Mysql5.0为例，安装好以后从命令行登录Mysql：
在命令行输入：mysql -u root(用户名) -p
然后根据提示输入密码后，登录数据库；

![1](https://img-blog.csdnimg.cn/20190331001501504.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

登陆后，输入show databases查看数据库中哪些库：

![2](https://img-blog.csdnimg.cn/20190331001513888.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

这里面其中的：

```php
	information_schema,
	Mysql,
	Test,
	Performation_schema
```

这三个库是mysql安装后自带的，不用去使用他们就ok，接下来创建自己的数据库来使用：
输入create database Cheung，然后使用自己创建的数据库，输入use Cheung；

![3](https://img-blog.csdnimg.cn/20190331002658480.PNG)

出现上面这样的界面就表示我们当前要使用的数据库是Cheung，准备工作这就ok了，接下来开始正式的sql语句的练习。

> 2.下面学习创建表的操作：

在命令行输入以下的操作就是在创建数据库中的表，看到有的操作会在每个数据的名字上面加上单引号，这个可以不用加上的，两者的效果是相同的：

![4](https://img-blog.csdnimg.cn/20190331003859636.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

这样就表示要使用的表tab已经创建完毕了，我们可以通过输入：desc tab
来查看所见的表是否正确

![5](https://img-blog.csdnimg.cn/20190331004109274.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

在输入每个数据的之间注意要加上逗号隔开，否则就会出现建表错误ERROR的情况 ，这个时候一般是很尴尬的，所以一定要多注意细节，在最后一个括号和数据之间就不要多加括号了，因为加上就会又出现错误。



> 3.更新数据库中表的结构



更新表的定义，给表加上一行使用alter table + 表名 的命令：

![6](https://img-blog.csdnimg.cn/20190331004956803.PNG)

如果要删除表中的一列，就要用到关键字Column了，具体的操作如下：

![7](https://img-blog.csdnimg.cn/20190331005213732.PNG)

如果要把整个表都删掉，就直接输入：


![8](https://img-blog.csdnimg.cn/20190331005255654.PNG)

> 4.使用Insert插入数据

先看下现在tab表的结构是怎样的：

![9](https://img-blog.csdnimg.cn/20190331005655872.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

向表中插入数据，就要使用Insert语句，格式为：

```php
	Insert into 表名(列名1,列名2,…)  values(值1,值2,…)
```

下面展示下插入一组完整的数据：

![10](https://img-blog.csdnimg.cn/20190331012518308.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

要注意，我这里没有提前设置id的值能够自增，即没有在创建表时候写上：

```php
	id int not null auto_increment
```

所以表中的id列不能进行自加操作，因此需要在插入数据的时候将id这一项也写上，否则就会报错。

之后要做的就是把多组数据同时插入到所建的表中去：

![11](https://img-blog.csdnimg.cn/2019033101524363.PNG)

这些操作第一遍写的时候都是历经千辛万苦才搞定的，终于还算是功夫不负有心人，在我半夜3点的死磕下，终于把正确的答案磕出来了。

对于查询表中数据这块反倒是相对容易一些，这里就不多说了

![12](https://img-blog.csdnimg.cn/20190331015545654.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

> 5.使用update语句更新某一列

格式是：update 表名 set 属性1= 新值1，属性2 = 新值2 where 属性3 = ?
格式不难懂，关键是把他运用熟练。

![13](https://img-blog.csdnimg.cn/2019033102002695.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

最后就是删除表数据：

![14](https://img-blog.csdnimg.cn/20190331020450231.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

以及最后的清空整个表：

![15](https://img-blog.csdnimg.cn/20190331020509898.PNG)

这样一份完整的Mysql命令行高逼格操作就完成了，本人在学习过程中由于没有接触过类似的数据库语言就直接上手命令行，在过程中踩了不少坑，在这里也帮大家排排雷，以后如果有类似的问题也好解决了，以后也将继续学习其他的sql语言。