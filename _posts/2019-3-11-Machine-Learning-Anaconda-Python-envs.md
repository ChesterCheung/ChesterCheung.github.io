---
layout: post
title:  "机器学习之Python&Anaconda环境配置问题"
categories: MachineLearning
tags:  ML Python
author: Chester Cheung
---

* content
{:toc}


最近开始学习Python语言了，准备开始机器学习方面的学习了，在大佬学长的帮助下，先是安装了一个2018.12.0版本的Anaconda3，但是在配置环境时，出了些小毛病没办法解决：有一个jupyter notebook安装好以后电脑仍然显示未安装的提示，在几次死磕之后果断放弃，找学长要了一个低版本的Anaconda3，版本是5.0的，参照CSDN上面的一些文章，花费了三个小时，终于成功配置好了Python机器学习的环境，在这里也把过程写成博客，方便需要安装的同行们有个参考对比的标准。



在安装Anaconda包的时候，基本按照默认的安装推荐来就ok，安装完毕后，在计算机中搜索Anaconda prompt命令行，可能有时候会出现搜索不到的情况，这个时候也不用惊慌，打开“开始菜单”寻找最近安装好的Anaconda文件夹，在里面就有Anaconda prompt命令行，把他打开以后，会出现以下这样的界面：









![1](https://img-blog.csdnimg.cn/20190311222210595.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

在光标所在的位置输入：

	conda create -n py36 python=3.6

计算机可能出现以下提示：

	==> WARNING: A newer version of conda exists. <==
  
	current version: 4.5.11
	latest version: 4.6.7
	

Please update conda by running
	

$ conda update -n base -c defaults conda

不过千万别慌，只要按照提示进行输入就好，继续输入

	conda update -n base -c defaults conda

在产生的**Proceed([y]/n)?**问题下，输入y继续配置进程，如果仍然产生error，则重复输入

	conda create -n py36 python=3.6

早晚会出现一句

	Solving environment: done

这个时候再输入

	conda activate py36

就把Anaconda的从默认的base库切换到新创建的py36库目录下了。此时输入

	conda info --envs

就能看到已经将base库切换到py36的库下面了：

![2](https://img-blog.csdnimg.cn/20190311222536864.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

首先需要先安装一个升级版的pip：

	You should consider upgrading via the 'python -m pip install --upgrade pip' command.

安装成功后会提示出：

	Successfully installed pip-19.0.3

然后开始正式安装pandas环境，在命令行输入

	pip install pandas

安装pandas环境，成功安装后会出现

	Successfully installed numpy-1.16.2 pandas-0.24.1 python-dateutil-2.8.0 pytz-2018.9 six-1.12.0

相同的操作安装numpy 和matplotlib

	(py36) C:\Users\Lenovo>pip install numpy
	Requirement already satisfied: numpy in d:\anaconda\envs\py36\lib\site-packages (1.16.2)

在安装matplotlib时，可能会耗费大量时间，此时需要确保网速足够高，否则很可能出现安装错误的提示，别担心，只要重复进行一下操作即可

	(py36) C:\Users\Lenovo>pip install matplotlib

当pandas、numpy、matplotlib、jupyter notebook都安装好后，在命令行输入

	conda list

就能看见已经安装好的环境

	(py36) C:\Users\Lenovo>conda list
	# packages in environment at D:\Anaconda\envs\py36:
	#
	# Name                    Version                   Build  Channel
	attrs                     19.1.0                    <pip>
	backcall                  0.1.0                     <pip>
	bleach                    3.1.0                     <pip>
	certifi                   2016.2.28                py36_0    	
	colorama                  0.4.1                     <pip>
	cycler                    0.10.0                    <pip>
	decorator                 4.3.2                     <pip>
	defusedxml                0.5.0                     <pip>
	entrypoints               0.3                       <pip>
	ipykernel                 5.1.0                     <pip>
	ipython                   7.3.0                     <pip>
	ipython-genutils          0.2.0                     <pip>
	ipywidgets                7.4.2                     <pip>
	jedi                      0.13.3                    <pip>
	Jinja2                    2.10                      <pip>
	jsonschema                3.0.1                     <pip>
	jupyter                   1.0.0                     <pip>
	jupyter-client            5.2.4                     <pip>
	jupyter-console           6.0.0                     <pip>
	jupyter-core              4.4.0                     <pip>
	kiwisolver                1.0.1                     <pip>
	MarkupSafe                1.1.1                     <pip>
	matplotlib                3.0.3                     <pip>
	mistune                   0.8.4                     <pip>
	nbconvert                 5.4.1                     <pip>
	nbformat                  4.4.0                     <pip>
	notebook                  5.7.6                     <pip>
	numpy                     1.16.2                    <pip>
	pandas                    0.24.1                    <pip>
	pandocfilters             1.4.2                     <pip>
	parso                     0.3.4                     <pip>
	pickleshare               0.7.5                     <pip>
	pip                       19.0.3                    <pip>
	pip                       9.0.1                    py36_1    	
	prometheus-client         0.6.0                     <pip>
	prompt-toolkit            2.0.9                     <pip>
	Pygments                  2.3.1                     <pip>
	pyparsing                 2.3.1                     <pip>
	pyrsistent                0.14.11                   <pip>
	python                    3.6.2                         0    	
	python-dateutil           2.8.0                     <pip>
	pytz                      2018.9                    <pip>
	pywinpty                  0.5.5                     <pip>
	pyzmq                     18.0.1                    <pip>
	qtconsole                 4.4.3                     <pip>
	Send2Trash                1.5.0                     <pip>
	setuptools                36.4.0                   py36_1    	
	six                       1.12.0                    <pip>
	terminado                 0.8.1                     <pip>
	testpath                  0.4.2                     <pip>
	tornado                   6.0.1                     <pip>
	traitlets                 4.3.2                     <pip>
	vc                        14                            0    	
	vs2015_runtime            14.0.25420                    0   
	wcwidth                   0.1.7                     <pip>
	webencodings              0.5.1                     <pip>
	wheel                     0.29.0                   py36_0    
	widgetsnbextension        3.4.2                     <pip>
	wincertstore              0.2                      py36_0

安装好以后还存在一个问题，之前一个学长在安装时碰到了奇怪的问题，当时显示的是无法连接到python，不知道为什么，装的notebook居然都是中文，后来发现是 no connection to kernel。这是版本问题，现在直接 pip install jupyter notebook 时，附带安装的 tornado 是6.0版本的，而能操作的是4.5.3版本的，
博客的链接是：[https://www.cnblogs.com/csu-lmw/p/10502496.html](https://www.cnblogs.com/csu-lmw/p/10502496.html)
最后具体的操作是

	pip install tornado==4.5.3

在安装好所有的环境后，就可以开始愉快而痛苦的及其学习了，虽然也只是刚开始学习，还是什么都不懂的小白，但希望经过2019年的学习，将会有更大的收获，争取在明年能有机会参加机器学习的相关比赛，也算是为枯燥的大学生活多增添一抹新奇的色彩
