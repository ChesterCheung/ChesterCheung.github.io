---
layout: post
title:  "机器学习必备之Pandas(一)"
categories: MachineLearning
tags: ML Pandas Python
author: Chester Cheung
---

* content
{:toc}


Pandas含有能使数据分析工作变得更快的高级数据结构和操作工具，Pandas是基于Numpy构建的，他让Numpy为中心的应用变得更简单了。


其中，Pandas具有按轴对齐数据的功能，可以防止许多由于数据未对齐而导致的错误；既能处理时间序列的数据也能处理非时间序列的数据；灵活处理缺失的数据等。
首先在使用Pandas库之前，我们需要先将pandas导入：









```php
	[In] from pandas import Series, DataFrame

	[Out] import pandas as pd
```

> ## Pandas的数据结构

想要使用pandas，就必须要使用他的两个主要的数据结构：Series和DataFrame，下面分别进行介绍：

> Series

Series是一种类似于一维数组的对象，他由2部分组成：一组数据和一组索引(数据标签)：

```php
	[In] from pandas import Series, DataFrame
	[In] obj = Series(['a','b','c','d'],index = [9,2,3,4])
	[In] obj
	[Out] 	9    a
		  2    b
		  3    c
		  4    d
		  dtype: object
```

Series 的字符串表现形式为，索引在左边，数据在右边，如果我们没有为数据创造一个索引，则会自动创建一个从0–N-1的索引。

与数组的操作类似，我们可以通过索引来选取Series中的某个或多个值。

```php
	[In] obj[2]
	[Out] 'b'
```

有关于Numpy的数组运算(根据布尔类型进行过滤、标量乘法、应用数学函数等)都会保留索引和值之间的联系

```php
	[In] obj[obj.index>5]
	[Out] 9    a
	  dtype: object
	  
	[In] obj * 2
	[Out]   9    aa
		2    bb
		3    cc
		4    dd
		dtype: object
```

可以将Series看作是一个有序的字典，因为他是索引值到数据值的一个映射，还可以用在许多原本需要字典参数的函数中去。

如果数据是存放在一个Python字典中，也可以直接通过这个字典来直接创建Series的：

	[In]  import pandas as pd
	[In] from pandas import Series , DataFrame
	[In] sdata = {'a':None,'b':678, 'c':139,'d':999}
 	[In] obj2 = Series(sdata)
 	[In] obj2
 	[Out]  a      NaN
		b    678.0
		c    139.0
		d    999.0
		dtype: float64

可以用pandas来处理检测缺失数据，用到isnull和notnull函数，对于缺失的数据，在pandas中我们用到NaN来表示(not a number)

```php
	[In]pd.isnull(obj2)
	[Out]a     True
   	 b    False
   	 c    False
   	 d    False
   	 dtype: bool
   	 
	[In] pd.notnull(obj2)
	[Out] a    False
	  b     True
	  c     True
	  d     True
	  dtype: bool
```

Series对象本身及其索引都有一个name属性，该属性跟pandas其他的功能关系非常密切；索引可以直接进行修改，使用.index就可以通过赋值的方式就地修改了，这里就不再多说。



> DataFrame



DataFrame是一个表格形式的数据结构，它含有一组有序的列，DataFrame既含有行索引，也含有列索引，可以简单看作Series组成的字典。
构建DataFrame的方法有很多，直接传入一个等长的列表或Numpy数组就是最好的办法：

```php
	[In] from pandas import Series , DataFrame
	[In] import pandas as pd
	[In] data = {'a':[1,2,3], 'b':[6,7,8],'c':[11,12,13], 'd':[11,22,33], 'e':[77,88,99]}
	[In] data_frame = DataFrame(data)
	[In] data_frame
	[Out]
```

![1](https://img-blog.csdnimg.cn/20190414154740356.png)

他的结果中会自动加上索引，且全部都被有序排列，如果通过设置属性

```php
	data_frame = DataFrame(data_frame, columns = ['a', 'e', 'b', 'd', 'c'])
```

就能给DataFrame的列按照指定的顺序进行排列；
和Series一样，如果传入的列在数据中找不到，就会产生一个NaN值；

```php
	[In] data_frame['a'] = np.arange(3) #将列表或数组的值赋给某个列时，其长度必须和DataFrame的长度相匹配
	[Out] 
	 a	b	c	d	e
	 0	0	6	11	11	77
	 1	1	7	12	22	88
	 2	2	8	13	33	99
```

列可以通过赋值的方式来进行修改，比如我们给‘a’这一列进行修改，就能看出来他的值被我们修改了。


另外一种创建DataFrame的方式就是通过嵌套字典(就是字典里面的字典)，如果将它传给DataFrame，就会被解释成：外层的字典作为列索引，内层的字典作为行索引；同样我们也可以对数组进行转置：

```php
	[In] data_frame = data_frame.T
```

下面给出可以输入到DataFrame构造器的数据

![2](https://img-blog.csdnimg.cn/20190414160911415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

如果设置了相关的数据，比如name、index等，则在对DataFrame进行输出的时候，这些信息也会被显示出来；
跟Series相同，value属性也会以二位数组ndarray的形式返回DataFrame中的数据

![3](https://img-blog.csdnimg.cn/20190414161122658.png)

就像这样(随便找了一个DataFrame)	

![4](https://img-blog.csdnimg.cn/20190414161221626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

> 索引对象

pandas的索引对象负责管理轴标签和其他元数据，构建Series和DataFrame时所用到的任何数组和其他序列的标签都会被转换成一个Index：

![5](https://img-blog.csdnimg.cn/20190414172428508.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

并且，Index对象是不可以修改的(immutable)，不可对其进行修改。这种不可修改性非常重要，他保证了Index对象在多个数据结构之间安全共享。

![6](https://img-blog.csdnimg.cn/2019041417285261.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

上图中列出了pandas中内置的Index类，Index甚至可以被继承从而实现特别的轴索引功能；

虽然大多数用户不需要知道太多的关于Index的细节，但是Index确实是Pandas数据模型的重要组成部分。

下面也介绍一些Index的一些方法和属性：

![7](https://img-blog.csdnimg.cn/20190414173252945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

> ## Series和DataFrame基本功能

**重新索引**

reindex的作用是创建一个适应新索引的新对象

```php
	[In] obj = Series([1,2,3,4,5], index = ['e', 'b', 'c', 'a', 'd'])
	[In] obj
	[Out] e    1
	  b    2
	  c    3
	  a    4
	  d    5
	  dtype: int64	  
	[In] obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
	[In] obj2
	[Outf]  a    4
		b    2
		c    3
		d    5
		e    1
		dtype: int64
```

对于时间序列等有序数据，重新索引时可能需要一些插值处理，method即可达到这样的效果，
使用ffill或pad向前填充(或搬运)，
使用bfill或backfill向后填充(或搬运)值

![8](https://img-blog.csdnimg.cn/2019041417595765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

对于DataFrame来说，reindex可以修改行索引、列索引，或者两个都修改；如果进传入一个序列，则会重新索引行：

![9](https://img-blog.csdnimg.cn/20190414180305187.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

使用columns关键字即可重新索引列：

![10](https://img-blog.csdnimg.cn/20190414180345201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

也可以同时对行和列索引进行重新索引，注意：此时的插值只能按行应用(即轴0)：

![11](https://img-blog.csdnimg.cn/20190414180451729.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

在这里也列出reindex的相关参数以及简单说明：

![12](https://img-blog.csdnimg.cn/20190414180539639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

**丢弃掉指定轴上的项**

只需要有一个索引数组或列表即可，drop方法返回的是一个在指定轴上删除了指定值的新对象：

![13](https://img-blog.csdnimg.cn/20190414180755800.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

对于DataFrame，可以删除任意轴上的索引值：

![14](https://img-blog.csdnimg.cn/201904141808469.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

![15](https://img-blog.csdnimg.cn/20190414180904396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

pandas最重要的一个功能是，他能对不同的索引进行算数运算。在将对象相加时，如果存在不同的索引对，则结果的索引就是该索引的并集额，看个简单的例子

![16](https://img-blog.csdnimg.cn/2019041418333140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

对于DataFrame，对其操作会同时发生在行和列上。


在对不同索引的对象进行算数运算时，如果默认的情况下，两个索引对象中没有重叠索引的位置就会产生NaN值；为了解决这种问题，我们可以给fd1使用add算数方法，传入df2和fill_value参数，这样没有重叠的位置就会出现默认填充值为0


还有一些其他的算数方法，这里也一并给出：

![17](https://img-blog.csdnimg.cn/2019041418485581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

DataFrame和Series之间的运算类似于二维数组和一维数组之间的运算，直接拿DataFrame和Series之间的运算来举例：

![18](https://img-blog.csdnimg.cn/20190414190816602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

(这个其实就叫广播)
在默认情况下，DataFrame和Series之间的算术运算会将Series的索引匹配到DataFrame的列，然后沿着行一直向下进行广播：

![19](https://img-blog.csdnimg.cn/20190414191031775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

但是如果某个索引值在DataFrame或者Series的索引中找不到，则参与运算的两个对象就会被重新索引以形成并集

![20](https://img-blog.csdnimg.cn/20190414191150291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

