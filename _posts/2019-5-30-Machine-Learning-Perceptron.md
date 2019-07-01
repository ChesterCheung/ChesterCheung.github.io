---
layout: post
title:  "ML--Perceptron感知机"
categories: Machine Learning
tags: ML DL AI Perceptron
author: Chester Cheung
---

* content
{:toc}


感知机(perceptron)是**二分类的线型模型**，输入特征向量，输出输出其所属的类别，取+1和-1为二值，很显然，感知机属于将输入实例划分为正负两类的模型，因此属于判别类型。



感知机的目的是将训练集的数据进行线性划分，分离得到超平面。在实际操作中，导入基于错误分类的损失函数，利用梯度下降法对损失函数进行极小化，即可求得感知机模型。这种分类方法一看就简单易于实现，比起logistic regression显然要简单不少，主要分为原始形式和对偶形式，是神经网络支持向量机的基础(SVM)。



输入空间：X属于R^n，输出空间是：Y={+1, -1}








由输入空间到输出空间的函数如下：

	f(x) = sign(w*x + b)

这个就称为感知机，w和b为感知机模型参数，w叫做权值或权值向量，b叫做偏置，w*x表示w和x的内积，sign是符号函数，当x>=0时，等于+1；当x<0时，等于-1


感知机是线性分类模型，属于判别模型，感知机的假设空间是定义在特征空间中的所有线性分类器，即函数集合是{f| f(x) = w*x + b}



> ## 感知机的学习策略


给定一个数据集，我们能够将数据集中所有的正实例点和负实例点都完整的划分到超平面的两侧，这种时候我们称为线型可分数据集，否则就是线型不可分。



感知学习的策略：


为了找到能将正负实例点完全正确分开的超平面，需要确定一个学习模型，我们这里采取的方法是定义损失函数并将损失函数最小化方法。



损失函数的一个自然选择是错误分类点的总数，但是这个损失函数不是线性可求导的，不容易进行优化或定性分析；所以我们这里采用的是另外一种方法：



误分类点到超平面S的总距离，这是感知机所采用的策略，为此，我们先要写出输入空间中任意点到

![1](https://img-blog.csdnimg.cn/20190524160043431.png)

超平面的距离：

这里的||w||是w的L2范数。假设超平面S的误分类点集合为M，那么所有误分类点到超平面S的总距离

![2](https://img-blog.csdnimg.cn/20190524160421868.png)

不考虑||w||，我们就得到了感知学习的损失函数：

![3](https://img-blog.csdnimg.cn/20190524160620310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

这个损失函数就是感知机学习的经验风险函数。



> ## 感知机学习算法


有了前面的铺垫，我们把感知机学习问题转化为了求解损失函数式的最优解问题，最优化的方法是随机梯度下降法(stochastic gradient descent)。



首先，任意选取一个超平面w0，b0，然后用梯度下降法不断地极小化目标函数。极小化过程中不是一次使M中的所有误分类点的梯度下降，而是一次随机选取一个误分类点使其梯度下降



算法1.1(感知机学习算法的原始形式)



![4](https://img-blog.csdnimg.cn/20190524161825478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

这种算法就是我们前面学习的基本的算法思路，当有实例点被误分类，即位于分离超平面的错误的一侧时，调整w和b的值，使分离超平面向该误分类点的一侧移动，以减小该误分类点到超平面的距离，直至超平面超过该误分类点使其被正确分类。这是感知机的最基本算法，与后面学习的对偶形式相对应，称为原始形式算法。



感知机学习算法在采用不同的初值或选取不同的误分类点时，解可能不同。



下面给出前几步详细地计算步骤，仅供参考：

![5](https://img-blog.csdnimg.cn/2019052417262391.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

下面就是求解感知机的迭代过程：

![6](https://img-blog.csdnimg.cn/20190524172733319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

感知机学习算法原始形式的收敛性

经过有限次迭代可以得到一个将训练数据完全正确划分的分离超平面及感知机模型。具体的证明就不再给出了，自己理解也还不到位，还要继续加强。不过不理解证明的过程也没有关系，不影响我们的正常使用，只要默认他是收敛的就ok。

![7](https://img-blog.csdnimg.cn/20190524212855903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

这里需要怎么理解(2)定理呢？


如果R越大，也就是max||x^ i||越大，此时相当于有一个点距离原点很远，在初始化时，我们常常初始化w=0，b=0，所以我们要到达正确的分界面所需要的迭代次数也就越多，因此上界越大；
如果γ越小，即min{yi(wopt⋅xi+bopt)}越小，也就是说，对于点xi，yi来说，虽然它被正确分类，但是它离最优分界面wopt，bopt很近，所以很容易就因为在对其它点进行更新时，导致这个点被误分类，因而迭代次数的上界越大。



算法1.2(感知机学习算法的对偶形式)



感知机学习算法的原始形式和对偶形式与后面要学习的支持向量机学习算法的原始形式和对偶形式相对应。


对偶形式的基本思路和想法是，将w和b实例xi和标记yi的线性组合的形式，通过求解其系数而得到w和b。为了确保一般性，我们假设初始值为w0，b0都是0，对误分类点有：


w <- w + nyixi


b <- b + nyi


逐步修改调整w和b，最后得到的w和b分别表示为:

![8](https://img-blog.csdnimg.cn/20190524204946255.png)

当n = i时，表示第i个实例点由于误分而进行更新的次数，实例点更新的次数越多，意味着他距离分离超平面越近，也越难进行分类，这样的实例对机器学习的影响最大。

[知识点补充]：
Gram矩阵的定义，主要用于度量各个维度自己的特性以及各个维度之间的关系：

![9](https://img-blog.csdnimg.cn/20190524210318861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

我们这里列出来求解感知机对偶形式的基本思路：

![10](https://img-blog.csdnimg.cn/20190524210440196.png)

参考博客：[https://blog.csdn.net/qq_29591261/article/details/77945561](https://blog.csdn.net/qq_29591261/article/details/77945561)
这篇博客里面有详细的代码实现，可以动手跑一跑尝试。

> ## 原始形式和对偶形式的对比

感知机学习算法是基于随即梯度下降法的对损失函数的最优化算法，有原始形式和对偶形式，算法简单且易于实现。

原始形式中，首先任意选取一个超平面，然后用梯度下降法不断极小化目标函数，在这个过程中，一次随机选取一个误分类点使其梯度下降。