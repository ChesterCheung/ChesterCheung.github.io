---
layout: post
title:  "ML Three Elements of Statistical Learning"
categories: Machine Learning
tags: ML Statistical AI
author: Chester Cheung
---

* content
{:toc}


统计学习的三要素：模型、策略、算法，简单表示为：



# 方法 = 模型 + 策略 + 算法


## 模型



统计学习的首要目的是要想好需要学习出什么样的模型，在监督学习中，模型就是所要学习的条件概率分布或决策函数(简单来说，模型就是我们要使用的一种函数)。模型的假设空间(hypothesis space)包含有所有可能的条件概率分布或决策函数。比如举个简单的例子：假设决策函数是输入变量的线型函数，那么模型的假设空间就是所有的线型函数所构成的集合，所以假设空间中的模型一般有无数多个。

![1](https://img-blog.csdnimg.cn/2019043023100327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

我们这里说由决策函数表示的模型称为非概率模型，由条件概率表示的模型是概率模型，为了简便起见，当论及模型时，有时也可以只用其中的一种模型。



## 策略



现在有了基本模型的假设空间了，接着要考虑的是依照什么样的准则进行学习或选择最优的模型。我们的最终目标就是在假设空间中选择最优的模型。



**1.损失函数和风险函数
**


监督学习问题是在假设空间中，选取模型f作为决策函数，对于给定的输入x，由f(x)给出相应的输出Y，这个输出的预测值f(x)和真实值Y可能相等也可能不相等，这时我们用一个损失函数或代价函数来度量预测错误的程度。


统计学习中常用的损失函数有以下几种：

![2](https://img-blog.csdnimg.cn/20190430232123553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

并且，损失函数的值越小，证明模型就越好。由于模型的输入输出(X, Y)是未知的，所以遵循联合分布P(X，Y)，所以损失函数的期望(即期望风险)是：

![3](https://img-blog.csdnimg.cn/20190430232605757.png)

这个是理论上模型f(X)关于联合分布P(X,Y)的平均意义下的损失，称作风险函数(risk function)或期望损失(expected loss)。



所谓的**联合分布**，就是多维的分布函数，对于两个随机变量x和y，联合分布就是同时对x和y进行概率分布。

模型f(X)关于给定训练数据集的平均损失函数称为经验风险(empirical risk)或经验损失(empirical loss):

![4](https://img-blog.csdnimg.cn/20190430235752676.png)

期望风险和经验风险的不同点
期望风险是模型关于联合分布的期望损失
经验风险是模型关于样本训练集的平均损失
由于大数定律，当样本容量趋于无穷时，经验风险趋于期望风险；所以我们很自然就能想到，使用经验风险来估计期望风险。但是，现实中的样本并没有那么大，甚至很少，所以用经验风险来预估是不准确的，要对经验风险进行一定的矫正，这里就引出了监督学习的两个基本策略：经验风险最小化和结构风险最小化。

**2.经验风险和结构风险最小化**

经验风险最小化的策略是，经验风险最小的模型是最优的模型(在假设空间、损失函数、训练数据集确定)，在样本数据足够大的情况下可以保证有很好的学习效果，所以按照经验风险最小化求解最优解的模型：

![5](https://img-blog.csdnimg.cn/20190501150219192.png)