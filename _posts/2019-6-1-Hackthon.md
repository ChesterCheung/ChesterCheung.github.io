---
layout: post
title:  "佩剑温酒走江湖 -- 华中HackFun"
categories: thinking
tags: Hackthon AI idea
author: Chester Cheung
---


* content
{:toc}

> 每个勇士都希望通过一段成功的经历来证明自己，但仰望星空还需脚踏实地，真正的成长道路是：
**输、输、输、输、输、输、输、输、输——赢！**
——HackFun比赛有感

这是最近的最后一个比赛了，其他项目也陆续都结题，这次打完就要用一段时间来沉淀下自己，总结这段时间的收获与体会，争取下次参赛能够做出强者的姿态！见识到很多大牛，了解到外面的世界，眼界宽阔不少；但实力一定要能配得上自己的眼界，否则付出的功夫可能都是泡沫浮云。

![1](https://img-blog.csdnimg.cn/20190528210547152.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)









> ## 惨痛的开场

这次我们组的路人局，没有事先找到靠谱的队友，湖大的两位学姐是真的什么都不会就来参加的(比我更勇敢)；也没有提前选择好合适的项目，造成了我们开局的不利。从比赛9点到场，到当天晚上9点之间，我们换了5个题目，每个题目都存在一定的可行性，但都被Mentor们指出了致命的错误。从知启智幼儿教育、智能货架、教育资源平台、老年人社区，到最后我们做的demo——focus基于深度学习的广告精准投递闭环，我们几乎尝遍了AI这两年普遍使用的几个方向。



但最后选择这个项目，还是因为技术的落地性比较强，在较短的时间内能跑出一个简单的demo，后期功能的拓展性很高，而且整体项目的商业模式是我们做出的一次大胆尝试，虽然最后没有拿奖，但也为我们提供了一种全新的思路，算是收获匪浅。


![2](https://img-blog.csdnimg.cn/20190528205457899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

> ## 项目详情介绍

focus基于深度学习的广告精准投递闭环，通过插件的形式向平台出售，安装插件的平台可以通过调用用户摄像头捕捉到用户的表情；我们通过Tensorflow的LSTM模型进行对用户情感倾向判断，以表情的细微变化判断出用户在阅读广告时的情感倾向，是喜欢、厌烦、焦虑、伤心或者其他什么；我们将收集到的数据整合后反馈给广告商家，商家收到反馈后可以根据广告的实际效果，对广告进行二次精准投放，提高用户的广告体验度和广告触达效率。



有人提到设计用户隐私问题，我们考虑过了，但judge并没有在这个方向提问。我们通过用户授权使用、签署安全条款的方式对用户的隐私进行保护，可以简单化这个问题。



然后现场我们跑出来一个简单的demo，但由于数据集太少，识别的精确度还暂时做不到细微变化的识别，只能识别大概的情感判断。这也是评委最后pass掉我们的一个原因，说我们的产品展示效果不够好，但对比于同组出线的其他两个项目，我真心觉得我们的效果展示还算不错的了(这也是我不服的地方)。



> 效果展示图


下面放出效果展示图看下(比较傻帽别介哈):

![3](https://img-blog.csdnimg.cn/20190528211117660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

![4](https://img-blog.csdnimg.cn/20190528211127382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

![5](https://img-blog.csdnimg.cn/20190528211139359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

能做到这一步不错了，我们的训练集是小的可怜，再加上笔记本算力不够，目前只能做到不是很细微的情感判断，后期可能会继续优化下模型，再通过数据集的提高实现细微表情变化的判断。



总体来说，这次比赛对人工智能的一些功能的应用算是有了一定的尝试，比如百度AI平台提供的多种SDK，提供语音、图像、文本、自然语言处理等多种人工智能的应用，非常方便！还有paddlepaddle和Tensorflow框架，运用都十分方便。最后冠军的项目其实没有太强的技术含量，但他们的公益性目的为他们加了不少分：



林嘉俊和王进林两个化工院的带着两个不干活的计科的同胞，把两项已经成熟的应用结合到一起，想到了一个新的应用场景，帮助没有双手的残疾人使用电脑。通过百度人脸检测得到人脸的72个关键点，然后对返回的参数进行处理，通过头的方向的移动，实现对鼠标的移动操作；通过嘴上的15个关键点，以张嘴实现点击操作、以撅嘴实现双击操作；然后通过长时间张嘴实现输入文字，默认开始进行5秒的录音，将录音结果传送至云端进行解析，把结果返回到鼠标光标所在的位置，这样实现了打字的操作。



输虽然是输了，但总体感觉收获还是蛮大的，算是对这类比赛的套路有了一定的了解，比赛看中的最主要的还是idea和你的效果演示，这两个做好了，加上演讲的生动形象，想不拿奖都难。接下来的时间就是沉淀阶段了，把近期的学习收获好好捋一捋，也算是没有白去体验了。
