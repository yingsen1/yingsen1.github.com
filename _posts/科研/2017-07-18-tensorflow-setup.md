---
layout: post
title: tensorflow的安装（ubuntu16.04+cuda8.0+cuDNN6）
category: 科研
tags: 深度学习
keywords: 
description: 深度学习 tensorflow ubuntu
---

## 感悟
1. 遇到问题不要过于着急地去尝试新的方法，应该将整一个步骤看完。  

2. 学会看错误报告，并且学会在网上找解决方法和不要惧怕英文网站。  


## 整个过程
实验室要求搭建一个GOOGLE NMT翻译系统，需要用到tensorflow,刚好笔记本有ubuntu16.04和CUDA8.0，所以想都没想就打算安装在LINUX上,也对，但是每次关于环境的配置，总是遇到很多问题，并且走了很多弯路，最后，莫名奇妙地就解决了。
刚开始在[tensorflow中文网站](http://www.tensorfly.cn/)找到了解决方法，并且也开始尝试了，但是too young too simple，就用了pip的方法来安装tensorflow，由于是安装已编译的版本，所以如果用GPU的版本，只能用规定的cuda7.5和cudnn5，由于我不想在电脑上配置太多的版本，所以这种方法后来就失败了。经过一系列百度后，得知应该git clone源代码下来自己进行配置编译，这样的话，就能实现cuda8.0+cudnn6了。按照这个方法，虽然中间遇到了一些问题，最后莫名其妙地多尝试了几遍，忽然就成功了。所以把重要的资料记录下来，如果读者或者自己遇到同样的情况，就能找到成功的解决方法了。

## 电脑配置
设备：msi GP62-1071笔记本  

系统：ubuntu16.04  

CPU：i7-6700hq  

显卡：gtx965m  


[tensorflow官方安装指南](http://www.tensorfly.cn/tfdoc/get_started/os_setup.html)  

不要选择pip的安装方法，选择用源码下载。

## clone源码
```
$ git clone --recurse-submodules https://github.com/tensorflow/tensorflow
```
## bazel安装
- 安装JAVA环境  

```
sudo apt-get update
sudo apt-get install default-jre
sudo apt-get install default-jdk
```
- 添加bazel的源  

```
$ echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list #如果这里你用的是jdk7，要把jdk1.8替换为jdk1.7
$ curl https://storage.googleapis.com/bazel-apt/doc/apt-key.pub.gpg | sudo apt-key add -
```
- 安装bazel  

```
$ sudo apt-get update && sudo apt-get install bazel
```
- 测试  
在terminal中输入bazel来测试是否安装成功。  

## 下载其他依赖文件  

- 配置  

```
# For Python 2.7:
$ sudo apt-get install python-numpy swig python-dev python-wheel
# For Python 3.x:
$ sudo apt-get install python3-numpy swig python3-dev python3-wheel
```
## 编译tensorflow
cd进入tensorflow的目录，先进行对tensorflow的配置和符号链接  

```
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]:
Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] N
No Google Cloud Platform support will be enabled for TensorFlow
Do you wish to build TensorFlow with GPU support? [y/N] y
GPU support will be enabled for TensorFlow
Please specify which gcc nvcc should use as the host compiler. [Default is /usr/bin/gcc]:
Please specify the Cuda SDK version you want to use, e.g. 7.0. [Leave empty to use system default]: 7.5
Please specify the location where CUDA 7.5 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify the cuDNN version you want to use. [Leave empty to use system default]: 5
Please specify the location where cuDNN 5 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size.

Setting up Cuda include
Setting up Cuda lib
Setting up Cuda bin
Setting up Cuda nvvm
Setting up CUPTI include
Setting up CUPTI lib64
Configuration finished
```
注意这里要根据需要选择y和n，如果要用GPU的话，记得正确选择。  

- 编译

```
$ bazel build -c opt --config=cuda //tensorflow/cc:tutorials_example_trainer
$ bazel-bin/tensorflow/cc/tutorials_example_trainer --use_gpu
# Lots of output. This tutorial iteratively calculates the major eigenvalue of
# a 2x2 matrix, on GPU. The last few lines look like this.
000009/000005 lambda = 2.000000 x = [0.894427 -0.447214] y = [1.788854 -0.894427]
000006/000001 lambda = 2.000000 x = [0.894427 -0.447214] y = [1.788854 -0.894427]
000009/000009 lambda = 2.000000 x = [0.894427 -0.447214] y = [1.788854 -0.894427]
```
## 创建pip包并安装  
```
$ bazel build -c opt //tensorflow/tools/pip_package:build_pip_package

# To build with GPU support:
$ bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package

$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

#The name of the .whl file will depend on your platform.
#这一步之前,你要进入/te//tensorflow_pkg目录查看具体生成的whl文件的名称，这不是固定的
$ sudo pip install /tmp/tensorflow_pkg/tensorflow-0.10.0rc0-py2-none-any.whl
```
## 玄学的一步
做完以上的步骤后，进入Python，依然显示我没有配置tensorflow。  
此时灵机一动，进入ipython之后，居然能够成功运行。  
更神奇的是，进入python也可以运行。
到这里就已经成功了，如果还出现问题，参考下面的三个链接。




## 参考资料
[tensorflow中文官方指南](http://www.tensorfly.cn/tfdoc/get_started/os_setup.html)  
[深度学习主机环境配置: Ubuntu16.04+GeForce GTX 1080+TensorFlow](http://www.52nlp.cn/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E4%B8%BB%E6%9C%BA%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-ubuntu16-04-geforce-gtx1080-tensorflow)
[ubuntu14.04+GTX1080+cuda8.0+cudnn5.1+源码编译tensorflow安装教程](http://blog.csdn.net/u010900574/article/details/52201808)