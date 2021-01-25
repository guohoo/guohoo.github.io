---
title: 利用VULTR的API定时备份服务器
author: Mashiro
avatar: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/modules/avatar.jpg
authorLink: https://mashiro.nl
authorAbout: 懒人，没技术，自学中......
authorDesc: Love Mashiro forever!
categories: 技术
comments: true
date: 2020-06-08 17:33:00
tags:
  - VULTR
  - API
keywords: 备份
description: 既然选择了国外的服务器，就要做好万全的准备
photos: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/background/article-cover/36490785_p0.webp
---


今天上VULTR账户检查服务器的一些情况时，无意间发现它是有提供[API接口][1]的。  
枉我用了它家快一年的服务器，居然现在才发现。。。  
快速浏览了一下，发现这个API还支持备份服务器，简直就是为我这种懒人量身定做的。  
废话不多说，我们直接开车。

## 开启API接口 ##
直达地址：[https://my.vultr.com/settings/#settingsapi][2]，点击 `Enable API` 来开启。  
开启后会生成一个密钥，并自动把你当前登录的IP地址，加进 `Access Control` 名单里。

## 把服务器IP加入名单 ##
很简单，直接填进 `Access Control` 名单里，子网掩码留默认的32，然后点击add按钮

![][3]
十分不推荐 `Allow All IPv4` 或 `IPv6`,因为一旦你密钥泄露，可以说全世界都能登上你的账户,非常不安全。

## 获取服务器ID ##
在VULTR的控制台点开你的服务器，网址里面"SUBID="后面那串数字就是你的服务器id。

## 利用crontab设置定时任务 ##
进行下面的操作之前请确保你已经装有 `crond` 和 `curl`，没有请先安装。  
不会用 `crontab` 的小伙伴看[这里][4]，设置任务之前我们先看看官方文档给出的备份命令是什么。  
地址：[https://www.vultr.com/api/#snapshot][5]  
命令格式如下:
```shell
curl -H 'API-Key: YOURKEY' https://api.vultr.com/v1/snapshot/create --data 'SUBID=YOURSUBID'
```
可以先运行一下看看能不能创建一个新的备份，成功的话会返回一个"200"的值。  
如果成功则用 `crontab -e` 创建一个新的定时任务，默认是使用vi编辑，想用其他编辑器请在shell命令行输入：`select-editor` 重新选择编辑器。

打开后按"i"进入编辑模式，这里我们定每月1号自动备份：
```
0 0 1 * * curl -H 'API-Key: YOURKEY' https://api.vultr.com/v1/snapshot/create --data 'SUBID=YOURSUBID'
```
编辑完成后按Esc，然后输“**:wq**”保存退出。运行 `systemctl restart crond` 重启一下crond服务，再 `crontab -l` 查看一下已经设定好的定时任务，没什么问题的话就完成啦！

  [1]: https://www.vultr.com/api/
  [2]: https://my.vultr.com/settings/#settingsapi
  [3]: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2020-06/vultr-api.png
  [4]: https://www.runoob.com/linux/linux-comm-crontab.html
  [5]: https://www.vultr.com/api/#snapshot