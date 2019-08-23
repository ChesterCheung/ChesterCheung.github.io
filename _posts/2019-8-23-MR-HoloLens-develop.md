---
layout: post
title:  "基于MRTK的Microsoft HoloLens开发入门"
categories: MixedReality
tags: MR VR AR
author: Chester Cheung
---

* content
{:toc}

### 基于MRTK的Microsoft HoloLens开发

> 背景介绍

前段时间刚回到长沙，碰巧远大张公子的建筑公司最新开发出一种建筑材料，想要借助HoloLens MixedReality进行现场动态展示，所以我们团队将项目承接了过来，进行HoloLens真机的入门级别项目开发，并后期计划教公司的技术人员快速上手开发流程。在对方投资的一台HoloLens真机的协助下，基于MRTK进行了一些入门开发，开发过程中由于国内进行相关开发的团队过少，且微软官方z近期已将原HoloToolKit改版为MixedRealityToolKit也即MRTK，能搜到的有关HoloLens开发的博客很少，且大都存在时效性，给现今开发Hololens的团队带来了巨大的挑战；而官方文档又只有英文版且对入门级别不太友好，造成开发过程中难题不断。

所幸，在团队夜以继日连续几天的开发环境配置/查看源码/项目开发的努力下，最终将初期任务圆满完成，我已经把初版Demo的项目源码传到了GitHub上，以下是项目地址：

[https://github.com/zhyChesterCheung/HoloLens-Development](https://github.com/zhyChesterCheung/HoloLens-Development)

![2019-8-23-0](https://zhyChesterCheung.github.io/photos/2019-8-23-0.jpg)








> 开发必备条件

下面就是我们团队配置了一晚上才勉强通过的环境，大家千万小心，版本更新太快了......

名称|环境
--|:--:
OS(macOS特别说明)|Windows 10(家庭版除外)
Visual Studio|Visual Studio Community 2017及以上版本
Unity3d|Unity 2018.4.6f1及以上版本

### OS系统推荐

强烈推荐使用windows进行U3d的开发及Visual Studio的调试，因为HoloLens本来就是微软自家的孩子，用来开发当然理所因当，而其他系统的兼容性和支持性就明显不如windows那么友好👬了，因为k配置环境到一半就会发现少了一个类似SDK的东西，叫做UWP(universal windows platform)，x想将项目部署到HoloLens平台上，这个UWP是必须的，否则下一步就不要进行了，我劝你换台电脑再继续开发。

> 对于macOS系统，我z找遍了网上所有说mac可以做开发的博客，并最终总结出：mac开发b需要装双系统或者虚拟机，其实还是逃不开windows10系统，所以劝你还是乖乖认命吧。

### Visual Studiopj配置

Visual Studio的配置是否正确决定了项目能否成功从Unity中构建导出，进而设计能否在Hololens生成APP及调试。下面是团队经多次尝试后确认的正确配置所需选择的，建议大家不要轻易修改：

![2019-8-23-1](https://zhyChesterCheung.github.io/photos/2019-8-23-1.png)

![2019-8-23-2](https://zhyChesterCheung.github.io/photos/2019-8-23-2.png)

![2019-8-23-3](https://zhyChesterCheung.github.io/photos/2019-8-23-3.png)

![2019-8-23-4](https://zhyChesterCheung.github.io/photos/2019-8-23-4.png)

在进行Hololens的开发时，我们需要集成微软官方提供的 MixedRealityToolKit项目。MixedRealityToolKit ，即原来的HoloToolkit-Unity项目，简称MRTK，是微软官方的开源项目，用于帮助开发者快速开发 HoloLens 应用，能够快速为项目集成基本输入、空间映射和场景匹配等特性。

关于该项目的详细介绍，可以参考MRTK官方说明文档：
[https://microsoft.github.io/MixedRealityToolkit-Unity/README.html](https://microsoft.github.io/MixedRealityToolkit-Unity/README.html)

在Github上下载MRTK项目代码：
[https://github.com/microsoft/MixedRealityToolkit-Unity](https://github.com/microsoft/MixedRealityToolkit-Unity)

![2019-8-23-5](https://zhyChesterCheung.github.io/photos/2019-8-23-5.png)

将下载的ZIP解压，使用Unity以打开工程的方式打开解压得到的文件夹，右击Assets，选择Export Package，将所有MRTK前缀的包全部选上，导出得到一unitypackage格式的文件，即是后续在Unity项目中可直接导入的MRTK工具包。

![2019-8-23-6](https://zhyChesterCheung.github.io/photos/2019-8-23-6.png)

### Unity3d开发配置

Unity3D是进行Hololens开发的主要平台，也是我们开发VR/AR/MR等等各种Reality的主要平台，对于各种建模和3D模型对有相对较好的支持，可以让美工/设计/开发人员共同操作的开发平台，下面是配置的步骤：
使用Unity新建一个3D项目，由左上角选项栏沿Assets-import package-custom package途径引入上一步中导出的unitypackage文件。
在成功导入后选项框上会出现一个新的选项——Mixed Reality Toolkit，点击并选择Add to Scene and Configure,选择添加图中高亮的MixedRealityToolKitConfigurationProfile，随即左侧框中出现MRTK及MRPlaySpace。

![2019-8-23-7](https://zhyChesterCheung.github.io/photos/2019-8-23-7.png)

![2019-8-23-8](https://zhyChesterCheung.github.io/photos/2019-8-23-8.png)

![2019-8-23-9](https://zhyChesterCheung.github.io/photos/2019-8-23-9.png)

由于Hololens内装的是UWP版的Windows 10系统，而Unity默认创建项目运行的平台即标准版本，与之不符，因而需在左上角选项栏中沿File-Build Settings去转换平台为UWP版，相关设置更改如下图，且勿漏选，错选。（笔者使用的是Unity 2018.4.6f1版本）选好后点击Switch Platform即可。

![2019-8-23-10](https://zhyChesterCheung.github.io/photos/2019-8-23-10.png)

仍在Building Settings中点击左下角的Player Settings，在Unity右侧的Inspector中选择XR Settings，勾选其中的Virtual Reality Support 和WSA Holographic Remoting supported。

![2019-8-23-11](https://zhyChesterCheung.github.io/photos/2019-8-23-11.png)

截至此步基本配置已完成，可通过Holographic Remoting Player与设备连接，点击开始即快捷地调试已有项目。注意Holographic Remoting Player是在Hololens上安装，电脑端通过Window-XR-Holographic Emulation途径打开下面的界面，Emulation Mode 选择Remote to Device，在Hololens上打开Holographic Remoting Player后即可获取Hololens的ip地址，输入到Remote Machine中即可。

详细使用方法见：[https://docs.microsoft.com/zh-cn/windows/mixed-reality/holographic-remoting-player](https://docs.microsoft.com/zh-cn/windows/mixed-reality/holographic-remoting-player)

![2019-8-23-12](https://zhyChesterCheung.github.io/photos/2019-8-23-12.png)

强烈推荐在Windows Store（Windows自带）上下载Microsoft HoloLens，可实时获取设备第一视角的直播，及进行实时照相，录屏等功能，方便团队开发。

![2019-8-23-13](https://zhyChesterCheung.github.io/photos/2019-8-23-13.png)

![2019-8-23-14](https://zhyChesterCheung.github.io/photos/2019-8-23-14.png)

本片博客的环境配置大多是靠队友支撑起来的，因为我用的是macOS电脑，所以最后我的电脑是无法跑起来的，但是这并不影响我对项目作出自己的贡献，我主要在项目中是负责C#脚本的编写和测试。

这篇博客也是从队友那边参考了一部分，这里也算是注明出处：

[https://blog.csdn.net/Brant_Stark/article/details/100043862](https://blog.csdn.net/Brant_Stark/article/details/100043862)
