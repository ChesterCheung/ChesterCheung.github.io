---
layout: post
title:  "串口通信相关参数"
categories: OS
tags: OS web
author: Chester Cheung
---

* content
{:toc}

### 串行通信定义

串行通信是计算机通信的主要方式之一，起到主机与外设之间以及主机之间的数据传输作用，因此串行通信的接口上计算机系统中的常用接口。







串行通信是指通信双方按位进行，遵守时序的一种通信方式。数据按位依次传输，每位数据占据固定的时间长度，即可使用少数几条通信线路就可以完成系统之间的信息交换。串行总线通信的显著特点是：通信线路少，布线简便易行，施工方便，系统间协议自由度以及灵活度较高。

### 分类

> 同步通信

同步通信是一种连续串行传输数据的通信方式，一次传输只传送一帧信息，其中通常包含若干个数据字符。

这里的信息帧由同步字符、数据字符和校验字符组成，同步字符用于确定数据字符的开始，一半是固定的开头同步字；数据字符在同步字符之后，由所需传输的数据块的长度决定；校验字符有1-2个，用于接收端对接收到的字符序列进行正确性的校验，常用奇偶校验法。

同步通信是按位传输。

>   异步通信

异步通信在发送字符时，所发送的字符之间的时间间隔是任意的，接收端要时刻做好接受的准备。因为能在任一时刻发送字符，所以必须在每一个字符的开始和结束的地方加上标志(开始位，停止位)，因此异步通信的好处是通信设备简单便宜，但是传输效率低下。