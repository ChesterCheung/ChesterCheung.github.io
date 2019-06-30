---
layout: post
title:  "ML Basic Definition"
categories: Machine Learning
tags: ML
author: Chester Cheung
---

* content
{:toc}


## 机器学习

机器学习(Machine Learning)，是一门基于统计学的学习，他其中包含了很多的数学知识，所以，数学没学好，ML两行泪。



参考论文：[http://cs229.stanford.edu/notes-spring2019/lecture1_slide.pdf
](http://cs229.stanford.edu/notes-spring2019/lecture1_slide.pdf)(英文水平低者慎入)



Arthur Samuel (1959)是这样评价机器学习的: **Machine Learning is the field of study that gives the computer the ability to learn without being explicitly programmed
**就是让机器在没有明确的程序的情况下，给予机器自我学习的一种能力



这在本质上和人类学习新事物的过程是相同的： **都是基于模型的构建**。










就拿高考来举个例子，我们备考过程中会做很多的练习题，但是考场上的题目都是我们没有见过的，那么这该怎么办？其实，在做题的过程中，我们的头脑中已经生成了一种模型(model)，通过这种模型，我们可以解决和这道题同类型的题目；机器学习也是相同的过程，我们首先通过给机器输入一个数据集(dataset)，通过对数据进行分析和处理后，给机器训练出来一个模型，然后将想要预测的数据输入到模型中，机器通过模型来预测出可能出现的结果。

这个过程不难理解，但是实际操作起来可能就比较困难了。



## ML分类



机器学习主要分为三个种类，**监督学习(Supervised learning)**，**监督学习(Unsupervised learning)**，**强化学习(Reinforcement learning)**。有时候他们不仅仅是机器学习的一种分类，也可以把他们看作机器学习的方法或工具。



对于机器学习来说，两种基础的算法就是**分类**(classifition)和**回归**(regression)，如果将数据输入到模型中所得到的y值是连续的，就是回归问题，如果是离散的就是分类问题。



监督学习和无监督学习**主要的区别**就在于，监督学习输入的训练集(train)有标签(label)，而无监督学习输入的训练集没有标签，也称作聚类(Clustering)。但是，有无标签的监督学习不是非黑即白的，他们之间存在一种过渡的种类叫做半监督学习(Semi-supervised learning)，其部分数据集是有标签的，但是无标签的数据远远多于有标签的数据(也符合现实情况)。



监督学习在CV方向的应用：图像分类、目标定位与检测
在NLP方面的应用：机器翻译(Machine translation)



无监督学习也有不少应用：潜在语义分析、词嵌入(Word embeddings)、相似含义词聚类(Hierarchically)等。



强化学习简单说，就是通过交互式的采集数据，来提高预测的准确性，相当于我们说的以战养战，形成良性循环

![1](https://img-blog.csdnimg.cn/20190424180955186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

## 聚类



上面提到的大部分都比较浅显易懂，只有聚类这方面的知识听起来比较懵，所以就侧重学习一下聚类：



聚类(Clustering)：是让计算机像人一样按照人思考问题的方式将数据集中的数据进行有差异性分组的方法，简单讲就是把有某一共性的数据分成一组。



**K-means算法**



K-means算法是聚类算法中最简单而且基础的一种了，但是其中实现的理念却不算简单。聚类数据无监督学习，我们熟悉的回归、朴素贝叶斯、SVM等都是有类别标签y的；而**聚类的样本中却没有给定y，而只给定了特征x**：比如我们假设沙滩上的石子可以表示为三维空间中的点集(x, y, z)，聚类的目的是找到每个样本x潜在的我们看不到的特征y，然后将相同类别的y放在一起，比如上面的石子，聚类后的结果是一个个石堆，石堆内部的点之间的距离比较近，而石堆之间的石子的距离就比较远。



简单来说就是，一组没有给定是什么类型的混乱的数据，通过彼此之间进行比较，将相似的数据分为一个类，相当于我们人为对混乱数据处理后，给每组数据加上了一个标签x，方便后期利用数据进行建模。(之所以存在无监督学习，就是因为现实中许多数据都是没有标签的，需要人为加上标签，但是人为加的成本又太过高，所以出现了无监督学习来提高算力)

在聚类问题中，给我们的训练样本是![2](https://img-blog.csdnimg.cn/20190425004423676.png) 每个![3](https://img-blog.csdnimg.cn/20190425004448441.png),都没有了y。

而K-means算法是将样本聚类成k个簇（cluster），具体算法描述如下：

![4](https://img-blog.csdnimg.cn/20190425004540533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

1.K是我们事先给定的聚类数，就是共分为多少组标签，


2.c(i)代表样例i与k个类中距离最近的那个类，c(i)的值是1到k中的一个，他表示的是每一个石子。


3.质心的坐标Uj代表我们对属于同一个类的样本中心点的猜测。

拿石堆模型来解释就是要将所有的石子聚成k个石堆，首先随机选取k个场地中的点（或者k个石子）作为k个石堆的质心，然后第一步对于每一个石子计算其到k个质心中每一个的距离，然后选取距离最近的那个石堆作为c(i)，这样经过第一步每一个石子都有了所属的石堆；第二步对于每一个石堆，重新计算它的质心Uj（对里面所有的石子的坐标求平均）。重复迭代第一步和第二步直到质心不变或者变化很小。



下图就是对n个样本进行K-means聚类的过程演示：

![5](https://img-blog.csdnimg.cn/20190425010546840.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

这里的算法强调的结束条件是，迭代到质心不变或变化很小，所以很大的问题就是，如何才能保证样本的收敛呢？想要明确收敛性，必须定性的进行描述，我们可以给前后两次的距离设定一个很小的值，比如0.0000001，这样在质心的位置变化很小的时候，对算法的递归就会结束。
或者我们来定性的描述一下收敛性：定义畸变函数(distortion function):

![6](https://img-blog.csdnimg.cn/20190425182924960.png)

很显然，J函数表示每个样本点到其质心的距离平方和。K-means是要将J调整到最小。假设当前J没有达到最小值，那么有两种方法可以进行：1.可以固定每个类的质心，调整每个样例的所属的类别c(i)来让J函数减少，2.同样，固定c(i)，调整每个类的质心Uj也可以使J减小。


这两个过程就是在内循环中使J单调递减的过程。当J递减到最小时，Uj和c也同时收敛。（在理论上，可以有多组不同的U和c值能够使得J取得最小值，但这种现象实际上很少见）。



但是由于畸变函数J是非凸函数，意味着我们不能保证取得的最小值是全局最小值，可能存在部分局部最小值也能使J和Uj趋近于变化很小或不改变，也就是说**k-means对质心初始位置的选取比较感冒**，但一般情况下k-means达到的局部最优已经满足需求。但如果你怕陷入局部最优，那么**可以选取不同的初始值跑多遍k-means**，然后取其中最小的J对应的和c输出(有点笨但是效果很好)。