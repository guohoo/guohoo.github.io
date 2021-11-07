---
title: Code Runner快速更改Python解释器路径
author: Mashiro
avatar: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/modules/avatar.jpg
authorLink: https://mashiro.nl
authorAbout: 懒人，没技术，自学中......
authorDesc: Love Mashiro forever!
categories: 技术
comments: true
photos: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/background/article-cover/87002546_p0.jpeg
date: 2021-08-02 16:30:45
tags: 'VS Code'
keywords: 
  - 'Code Runner'
  - Python
description: 配合Python插件快速更改成需要的版本
---


> 封面图：[#魔女の旅々 - pixiv](https://www.pixiv.net/artworks/87002546)

## 前言
目前主要使用 VS Code + WSL2 进行开发，经常需要更换 Python 运行环境，但是 Code Runner 插件默认会使用 `python -u` 去执行文件，而无法和 Python 插件进行配合，所以要在配置文件中反复更改解释器路径。另外如果使用默认配置，在 WSL2 中会无法运行文件（Linux 要用 `python3 -u`，而 `python -u` 默认使用 python2 去运行），所以要把 Code Runner 运行配置更改成 `python3 -u`，并在 Windows 多配置一条 python3.exe 软链接，指向所需的解释器才能正常运行。（不建议直接将 python.exe 改名成 python3.exe，实测这样做 pip 会出问题）  
显然这不是一个 Good idea，后来终于在 stackoverflow 找到了一个比较好的办法

## 安装 Python 插件
~~不会吧，不会吧，不会有人用 VS Code 不装插件的吧~~

## 更改 Code Runner 的设置
首先找到拓展，点击 Code Runner 的齿轮，选择拓展设置。  
打开后右上角有一个 **打开设置(json)** 的按钮，点进去切换到 json 格式  
然后定位到 `"code-runner.executorMap"`，修改成如下配置
```json
"code-runner.executorMap": {
        "python": "$pythonPath -u $fullFileName"
    },
```
$pythonPath 表示的是使用上面已经设置好的 pythonPath 的值，此值会随 python 插件更换解释器而更换  
$fullFileName 则是引用当前 python 文件的路径

最后将 `"code-runner.runInTerminal"` 所对应的值改为 `true`，就大功告成啦 ~

## 参考
[1] [How to allow VS Code to take input from users?](https://stackoverflow.com/questions/50689210/how-to-allow-vs-code-to-take-input-from-users)