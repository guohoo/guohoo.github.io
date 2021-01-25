---
title: Docker容器安装软件踩坑及换源
author: Mashiro
avatar: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/modules/avatar.jpg
authorLink: https://mashiro.nl
authorAbout: 懒人，没技术，自学中......
authorDesc: Love Mashiro forever!
categories: 技术
date: 2020-08-13 08:23:00
comments: true
tags: 
  - Docker
  - Linux
keywords: Ubuntu
description: 记录一下，虽然不是什么技术难题
photos: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/background/article-cover/67974799.png
---


换源网上一搜多的是教程，但是这些教程都没讲清楚，一不小心可能就掉坑里。  
没错，说的就是你，CSDN垃圾回收站。

因为最近重装了一次电脑，而 `Docker Desktop` 默认装在C盘，所以重装时也一并清掉了，只能重新安装。另外看到Ubuntu挺多人用的，于是没继续用之前的CentOS镜像，打算换换口味。但是在安装软件时却遇到了一些问题，特此记录一下。~~真就把Docker当成Linux用呗~~

打开容器，本来打算修改一下文件，结果给我返回来一个 `nano:command not found`。嗯？What the fu**  
Docker你的官方镜像也太精简了吧！真就一个白板系统。没办法，只能自己装个vim。

## 遇到的问题 ##
### 坑1 E: Unable to locate package vim ###
这个简单，`apt-get update` 同步一下 `/etc/apt/sources.list` 和 `/etc/apt/sources.list.d` 中列出的源的索引即可。正常情况下应该就可以安装软件了，除了速度有亿点点慢。~~默认用官方的源，能快就见鬼了~~  
不过因为我开了全局代理，所以速度还可以，直到又出来一个奇奇怪怪的错误......

### 坑2 Failed to fetch Connection failed [IP: 91.189.88.149 80] ###
一开始还以为是梯子掉线了，结果不然。Ping了一下那个IP也可以Ping通，不应该啊！于是直接用浏览器打开官方的仓库，找到没下载成功的组件，发现也可以正常下载，真的是莫名奇妙。  
不过没下载成功的组件不多，所以后面我直接手动下载下来，拷进容器里重新安装。但是为了避免以后再有这样的麻烦，所以决定对apt进行换源。

## 更改国内镜像源 ##
国内有很多这样的镜像站，像阿里镜像，网易镜像，还有清华镜像等等，选一个访问速度快的即可。看了一下网上的换源教程，都是直接扔给你一段配置文件，也不说清楚注意的事项，极其不负责任。要是直接用了他们的配置，可能没加上速，反而还有一堆的报错。

### 1.准备工作 ###
首先要明确你当前Linux的版本等信息，以及确保自己的Codename在镜像站上面找得到。像我的就是 `Ubuntu`，`Codename=focal`。( Codename可以用 `lsb_release -a` 查看，但Docker的白板镜像是没有 `lsb_release` 的，所以换成 `cat /etc/os-release` )以阿里的镜像站为例，到 [http://mirrors.aliyun.com/ubuntu/dists/][1] 上面看一下有没有自己的Codename，没有就换其他镜像站看看。接着才是修改 `sources.list` 文件。

### 2.修改文件 ###
修改前建议先备份一下：`cp /etc/apt/sources.list /etc/apt/sources.list.bak`  
然后再进入 `/etc/apt` 目录，打开 `sources.list` 文件，改成如下的形式：
```
deb http://mirrors.aliyun.com/ubuntu/ $Codename main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-backports main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-proposed main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-security main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-updates main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-backports main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-proposed main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-security main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-updates main multiverse restricted universe
```
**PS**：$Codename改成你自己的Codename即可，另外如果不需要源码编译安装的，可以把deb-src开头的几行注释掉

### 3.更新apt ###
最后再运行一次 `apt-get update`，没问题的话就算配置完成了。

## 一键脚本
为了照顾懒人，我写了个bash脚本，大家也可以参考一下
```bash
#!/bin/bash
cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
Codename=$((lsb_release -a)|awk '{print $2}'|tail -n 1) && \  #没有lsb_release的话把这行改成Codename="XXX"这种形式
echo "\
deb http://mirrors.aliyun.com/ubuntu/ $Codename main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-backports main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-proposed main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-security main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ $Codename-updates main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-backports main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-proposed main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-security main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ $Codename-updates main multiverse restricted universe">sources.list && \
sudo apt-get update
```

  [1]: http://mirrors.aliyun.com/ubuntu/dists/