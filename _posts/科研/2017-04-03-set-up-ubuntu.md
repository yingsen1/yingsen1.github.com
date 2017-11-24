---
layout: post
title: Ubuntu16.04的双系统安装
category: 科研
tags: Ubuntu
keywords: 
description: 适用于双显卡，6代cpu，N卡问题
---

## 感悟
1. **网上搜索教程或者问问题，多去英文网站去搜索**（stack_over_flow/aksubuntu），以过了六级的英文水准，明白作者的意思完全没问题。* （之前一直在贴吧去问问题，一是没有即使回复，而是成功的经验太少了）*  

2. **在做事之前，做一些调研，快速去动手，遇到问题，再去寻找解决方案，并且要尝试自己通过错误提示去解决**  *（下载了UBUNTU后，一直都没有尝试，在网上找了很多教程，都各自有自己的一套说话，并且因为问题很多，就一直没有尝试，生怕弄坏了电脑。结果在尝试之后，进度才会大大有长进）*  

3. 

## 整个过程
因科研需要安装linux，所以才会有需求要安装双系统。在所有发行版中，ubuntu算是比较大众的。之前在网上找了很多教程，很多事情我都想先尝试自己解决之后再问人，在机缘巧合之下，学长说帮我装，这当然最好不过了。不过在安装kylin ubuntu的时候，像网上说的一样，会卡在安装程序的LOGO处，在一番折腾BIOS之下，学长也无法解决这个问题。在贴吧上有人说Debian可以安装，所以尝试了一下，果然可以，但是由于界面不是unity，不习惯，还是偏向于安装ubuntu。所以接下来又是一番折腾。期间整整折腾了三天，每天都收获了一些知识，最终解决了所有问题，期间的汗水和绝望，非体验者不能体验，所以写下来，为了以后的“自己”。  

- 先是添加了引导参数成功解决了安装的问题
- 而后因为安装NVIDIA驱动导致了无限循环登录问题，这个问题折磨了我好久。
- 有点绝望，因为在度娘上搜不到解决的方案，并且看到有一个贴吧的楼主说，在输入 'lspci | grep VGA'后显示只有显卡，那就说明无法装载双显卡的驱动。所以有点绝望，但是之后才理解到，这个说法是说驱动桌面的确是用Intel核显，但是加载3d或者并行运算的时候，还是可以运用独显的。
- 机缘巧合之下，百度关键词' linux gtx965显卡驱动 '后，成功找到了有针对性的解决方案，顿时感觉看到了希望。
- 在安装完NVIDIA驱动之后，就是cuda、cudNN、caffe、MATLAB的安装了，期间虽然有一些问题，但机智的我还是成功解决了这些问题。最后跑通了第一个深度学习算法——MDnet。 

## 双系统的安装Ubuntu
设备：msi GP62-1071笔记本
系统：ubuntu16.04
CPU：i7-6700hq
显卡：gtx965m

[参考链接--贴吧](http://tieba.baidu.com/p/4987077178?pn=1)
插入u盘后，在安装的第一个界面下，按‘e’进入引导参数的编辑，在quiet splash 后面输入'$vt _handoff acpi_osi=linux nomodeset '，就能解决卡Logo这个问题。之后就按照流程来进行安装。  

进入系统后，发现分辨率很低，别担心，之后会有解决方案。这是我之后才发现的，在成功安装好独显驱动后，就可以删除nomodeset参数，来加载核显驱动桌面（不会卡logo或者黑屏）。  

如果忍受不了超低的分辨率，可以'sudo vi /etc/default/grub'下编辑分辨率。

## 显卡驱动的安装
这里特指N卡驱动，我的电脑是GTX965m显卡，我认为其他型号的显卡驱动也是适用的。  

[参考链接](http://www.linuxdiyf.com/linux/29084.html)  

因为过程太复杂了，就直接看上面的链接吧，按照指导的操作来进行，就能成功安装显卡驱动。

## cuda及cudNN的安装
[参考链接](http://www.cnblogs.com/xia-Autumn/p/6228913.html)  

建议CUDA还是安装最新版本的吧，链接上的和我安装的都是cuda8.0。  

 按照步骤来就好了，其中Caffe的安装，遇到了一些问题，等待解决吧。
 
## matlab的安装
就说说我遇到的问题吧。安装及破解的方式适用于所有版本。  

[安装破解教程](http://blog.csdn.net/lanbing510/article/details/41698285)  

我安装版本为2017a，安装文件为两个iso文件，需要分别解压下来，再将CD2的文件复制到CD1的目录下，然后对两个文件夹的权限进行更改，cd进入目录1进行安装就可以了。  

在运行程序的时候，碰到gcc版本不支持的问题。  

解决方案可以安装不同的gcc版本，再根据需要，进行版本的切换  
[gcc版本切换教程](http://blog.sina.com.cn/s/blog_54dd80920102vvt6.html)



## 补充
若出现PCI-ERROR等问题，在[GRUB上添加代码](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1521173)即可解决。

### 参考  
[gcc版本切换教程](http://blog.sina.com.cn/s/blog_54dd80920102vvt6.html)  

[matlab安装破解教程](http://blog.csdn.net/lanbing510/article/details/41698285)  

[cuda安装教程](http://www.cnblogs.com/xia-Autumn/p/6228913.html)  

[6代U安装Ubuntu教程](http://tieba.baidu.com/p/4987077178?pn=1)  

[Linux下GTX965M显卡驱动安装教程](http://www.linuxdiyf.com/linux/29084.html)
