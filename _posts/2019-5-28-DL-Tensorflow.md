---
layout: post
title:  "Tensorflow深度学习"
categories: MachineLearning
tags: ML DL Tensorflow AI Python
author: Chester Cheung
---

* content
{:toc}

神经网络模型图一般包括：一个输入层、一个或读个隐藏层、一个输出层，这样就搭建起来一个基本的神经网络。



输入层是输入数据形态的，我们把输入数据的每一种数叫做一个输入节点，一般用x来命名。当有多个输入数据时，就用x1，x2，x3来代替。



隐藏层是我们设计的网络中最重要的部分，隐藏层可能有一层或多层，每一层都会有一个或多个神经元，叫做神经元节点或隐藏节点。













每一个节点都是接受上一层的数据而进行一定的计算操作后，向下一层输出数据。在这个过程中，我们对于神经元上的这些操作统称为“计算操作”。





![1](https://img-blog.csdnimg.cn/20190517124011223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

下面我们通过一个简单的例子来对Tensorflow的运用有一个整体的入门了解：



某校评选三好学生，学校会根据学生的品德、体育、学习来计算一个总分(按照权重)。
现在有两个家长，他们知道各自孩子的三项成绩和总分，他们想 通过神经网络来推断出德育、体育、学习分别占到的权重大小。(这两个家长的思想真的超前…)



我们现在来到模型本身：这个是标准的向前反馈的神经网络，信号总是向前反馈的，我们输入了x1，x2，x3三个节点；由于数据的简单，我们只设计了一个隐藏层来对数据进行处理，处理的具体过程是将三组数据分别乘以对应的权重w1，w2，w3，最终将各项得分加起来得到输出数据y，代码实现如下：



	import tensorflow as tf
	
x1 = tf.placeholder(dtype=tf.float32)
	
x2 = tf.placeholder(dtype=tf.float32)

	x3 = tf.placeholder(dtype=tf.float32)

	
w1 = tf.Variable(0.1,dtype=tf.float32)
	
w2 = tf.Variable(0.1,dtype=tf.float32)
	
w3 = tf.Variable(0.1,dtype=tf.float32)
	

n1 = x1*w1
	
n2 = x2*w2

	n3 = x3*w3
	

y = n1+n2+n3
	

sess = tf.Session()
	
init = tf.global_variables_initializer()

	sess.run(init)
	

result = sess.run([x1,x2,x3,w1,w2,w3,y], feed_dict={x1:90,x2:80,x3:70})

	print(result)



> ## 定义神经网络模型


在windows环境下使用Tensorflow需要导入他的Python包，我们用pip install tensorflow-Python
来实现导包，然后import一下就可以直接使用了。


上述代码中


x1 = tf.placeholder(dtype=tf.float32)


x2 = tf.placeholder(dtype=tf.float32)


x3 = tf.placeholder(dtype=tf.float32)


我们在训练这个模型的时候，将x1，x2，x3作为输入数据输入到模型中，这种等待模型运行时才会输入的节点，我们称为占位符placeholder，就是在编写程序时还不确定输入什么数据，在模型运行时才知道输入什么数的就是占位符，相当于一个可以随时替换掉的临时变量。



接下来是定义w1，w2，w3这种在训练过程中会经常变化的神经元参数，Tensorflow中把他们叫做是变量、或可变参数。在定义这三个参数时，我们除了用dtype给参数指定类型，还传入了另一个初始值参数，即把初始值设置为0.1。



接下来隐藏层中的三个节点n1，n2，n3的定义不难理解，让nn分别等于xn乘以wn就ok。



输出层也不难理解，就让三个隐藏层相加就能得到最后的结果。



> ## 运用神经网络进行运算


我们对神经网络的定义已经完成，下面要看如何在这个神经网络中输入数据并进行运算得到结果。
	
	
sess = tf.Session()


这句语句定义了一个sess变量，其中包含了一个Tensorflow的会话session对象，可以简单把他看作是管理神经网络的一个对象，或者是之前建立的神经网络的一个实例化对象，有了他我们的神经网络就可以正常运行了。


[因此，在每次定义完成神经网络后，在运行前都需要一个会话对象，才能运用这个模型去进行预测计算]



会话对象管理神经网络的第一步，就是先将所有的可变参数初始化，也就是给所有的可变参数一个初始值

	

init = tf.global_variables_initializer()

	sess.run(init)


sess.run的意思就是在sess这个函数中运行init这个初始化函数；具体给每个可变函数赋什么初值，是我们刚才定义的w1，w2，w3这三个可变参数决定的。或者也可以将刚刚的语句合并起来：

	

sess.run(tf.global_variables_initializer())



然后，我们再次执行神经网络的计算：



	result = sess.run([x1,x2,x3,w1,w2,w3,y], feed_dict={x1:90,x2:80,x3:70})


这次不是初始化，而是真正进行运算，sess.run函数的第一个参数是一个数组，代表我们需要查看哪些结果项；另一个参数是命名函数feed_dict，代表我们要输入的数据，最后再将result的值输出。



> ## 训练神经网络


上面的神经网络可以运行，但是还不能实际运用，因为最终的运算结果是存在误差的，在投入使用前，我们需要进行训练，主要包含以下步骤：



1.输入数据


2.计算结果


3.计算误差：将计算结果与期待结果对比，看看误差是多少


4.调整神经网络中可变参数：根据相对误差的多少，使用反向传播算法，对神经网络中的可变参数进行相应的调节。


5.再次训练：重复以上步骤进行训练，直到误差低于我们理想的水平。

![2](https://img-blog.csdnimg.cn/20190524105813394.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)



	import tensorflow as tf

	

x1 = tf.placeholder(dtype=tf.float32)
	
x2 = tf.placeholder(dtype=tf.float32)
	
x3 = tf.placeholder(dtype=tf.float32)


	yTrain = tf.placeholder(dtype=tf.float32)

	

w1 = tf.Variable(0.1,dtype=tf.float32)

	w2 = tf.Variable(0.1,dtype=tf.float32)
	
w3 = tf.Variable(0.1,dtype=tf.float32)

	

n1 = x1*w1
	
n2 = x2*w2

	n3 = x3*w3

	
y = n1+n2+n3


	loss = tf.abs(y-yTrain)	 #损失函数，用来表示误差值
	

optimizer = tf.train.RMSPropOptimizer(0.001) #优化器，可以调整可变参数
	

train = optimizer.minimize(loss) #将可变参数传入优化器中，按照最小化原则调整可变参数
	

sess = tf.Session()
	
init = tf.global_variables_initializer()

	sess.run(init)
	

result = sess.run([train,x1,x2,x3,w1,w2,w3,y,yTrain,loss], feed_dict={x1:90,x2:80,x3:70,yTrain:85})
	
print(result)
	result = sess.run([train,x1,x2,x3,w1,w2,w3,y,yTrain,loss], feed_dict={x1:98,x2:95,x3:87,yTrain:96})
	
print(result)



这次多定义了一个占位符yTrain，这是在训练时传入每一组输入数据我们期待对应结果值的，一般称为：目标计算结果、目标值。



我们用tf.abs(y - yTrain)来表示计算结果和期待结果之间的误差。



然后定义了一个优化器变量optimizer，就是用来调整神经网络可变参数的对象。Tensorflow中有多种优化器，我们选用Alphago使用的RMSPropOptimizer，其中的参数0.001是这个优化器的学习率(learning rate)，学习率可以理解为，优化器每一次调整参数的幅度的大小，赋一个常用的值0.001。



这之后，我们又定义了一个训练对象train，代表了我们如何来训练这个网络。我们把train定义为optimizer.minimize(loss)，也就是让优化器按照将loss最小化的方式来调整可变参数。

![3](https://img-blog.csdnimg.cn/20190524111931308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)



> ## 

简化深度学习神经网络


**先看下张量的定义**：

在神经网络中，神经元节点接受数据后经过一定的计算操作输出的结果对象，张量在模型图中表现为各层的节点的输出数据，有时也可以把神经网络模型图中的节点看作是张量。
而节点加上连线所组成的整个神经网络模型图表现的是张量在神经网络中流动的过程，也是Tensorflow的名称的由来。



注意在Tensorflow中用print输出张量和可变函数时，并不会输出其中的具体数值，而是输出他们对应的操作和数据类型等信息。如果要查看他们的具体数值，需要在训练过程中用sess.run函数获得的返回值来查看。



> ## 通过向量重新组织输入数据，简化模型


还是之前的例子，我们如果要再德育、智育、体育的基础上再加参考的其他指标，比如艺术分数，输入层需要增加节点x1，隐藏层也需要增加一个n4节点，其实整套逻辑没有变，但还要去修改整套模型，这就很麻烦了。



所以我们通常将输入数据组织成向量来送入神经网络进行学习，向量中有几个数字，就是有几个维度，刚刚看到的就是一个三维向量。改进后的代码：



	import tensorflow as tf


	x = tf.placeholder(shape=[3], dtype=tf.float32)

	yTrain = tf.placeholder(shape=[], dtype=tf.float32)

	w = tf.Variable(tf.zeros([3]), dtype=tf.float32)
	

n = x * w
	
y = tf.reduce_sum(n)

	loss = tf.abs(y-yTrain)


	optimizer = tf.train.RMSPropOptimizer(0.001)
	

train = optimizer.minimize(loss)
	
sess = tf.Session()
	
init = tf.global_variables_initializer()


	sess.run(init)
	

result = sess.run([train,x,w,y,loss], feed_dict={x:[90,80,70],yTrain:85})

	print(result)
	

result = sess.run([train,x,w,y,loss], feed_dict={x:[98,95,87],yTrain:96})

	print(result)


这样的运行效果和之前是一样的，仅做了几处改动，在变量x的定义语句中，其中增加了一个命名参数shape，是表示变量x的形态的，取值是[3]，表示输入占位符x的数据是一个有3个数字的数组，也就是一个三维向量；后面的w1，w2，w3也被相应的缩减成了一个三维向量w：



其中tf.Variable函数的第一个参数还是定义这个可变参数的初始值，这里定义的初始值为[0,0,0]；



	yTrain = tf.placeholder(shape=[], dtype=tf.float32)


yTrain因为只是普通数字，不是向量，所以用空的[ ]来代表他的形态



由于我们把原来3个隐藏层节点n1，n2，n3缩减成了一个向量n，输出层节点y的计算也要变：



	y = tf.reduce_sum(n)


tf.reduce_sum函数的作用是把作为他的参数的向量中所有维度的值相加求和，和原来y = n1 + n2 + n3的含义是相同的。



后面对模型的训练过程基本是一样的，唯一不同在于输入数据时，所有的输入都简化成了一个x，所以feed_dict也要相应变化。



	result = sess.run([train,x,w,y,loss], feed_dict={x:[90,80,70],yTrain:85})


> ## 简化后的神经网络

模型
按照之前的简化步骤，得到简化后的神经网络模型：

![4](https://img-blog.csdnimg.cn/20190524120422580.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)



**用softmax来规范可变函数**：



wn = tf.nn.softmax([w1,w2,w3])，在这条语句里，让他等于tf.nn.softmax(x)的返回值，“nn”是neural Network的缩写，他可以把一个向量规范化后得到一个新的向量，这个新的向量中所有数值加起来保证为1。softmax经常被用来在神经网络中处理分类问题，但在这里，我们暂时只用它来解决所有权值相加等于1的需求。