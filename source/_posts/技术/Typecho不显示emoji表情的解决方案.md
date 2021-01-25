---
title: Typecho 不显示 emoji 表情的解决方案
author: Mashiro
avatar: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/modules/avatar.jpg
authorLink: https://mashiro.nl
authorAbout: 懒人，没技术，自学中......
authorDesc: Love Mashiro forever!
categories: 技术
date: 2020-06-07 03:06:00
comments: true
tags: 
  - Typecho
  - Emoji
keywords: Emoji
description: 这其实是编码的问题
photos: https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/background/article-cover/213951520701-1024x1024.jpg
---


其实之前一直没有发现这个问题，因为平时就算插入表情也是用贴图的方式，很少用emoji。但是前两天发布一篇文章后，突然发现文章的内容不见了一半，回到管理页面，里面也是只剩下前半截文章，真就凭空消失了呗！😂 ~~(当时快被气死了)~~

然后看了一下中断的地方，想起后面应该是接了一个emoji表情，初步怀疑是这个原因。后面撰写了一篇带emoji表情的文章进行测试，果真如此。就是一旦你保存了文章，emoji后面的内容都会消失。

一开始试了很多方法，如嵌个p标签进去；使用转义字符......但通通都失败了。后来上网查找资料，找到了一篇真正靠谱的[解决方案][1]。

## 问题原因 ##
因为之前的数据表用的都是 `UTF-8` 的编码方式，但在 `MySQL` 中，`UTF-8` 最多只支持3个字节，而emoji是4个字节，所以就...... 
但是我想起Wordpress的站点没出过这种问题啊，所以我特意查了一下它的数据表编码方式，发现居然是 `utf8mb4`。嗯，吹一波Wordpress大法好。

## 解决方法 ##
### 1.更改数据表编码方式 ###

> 请记得一定要先备份数据库，虽然我可以保证按我说的做一定不会损坏你的数据，但毕竟数据无价

首先登录到Typecho的数据库
```mysql
mysql -u root -p password #连接数据库命令
use <typecho数据库名>; #选择你的 Typecho 数据库
```
然后把下列数据表的编码方式改为 `utf8mb4`
```mysql
alter table typecho_comments convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_contents convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_fields convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_metas convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_options convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_relationships convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_users convert to character set utf8mb4 collate utf8mb4_unicode_ci;
```
改完后用 `show create table <表名>;` 命令，查看一下是否修改成功  
**请大胆放心地修改，因为utf8mb4是兼容UTF-8的，不会影响现有数据**

### 2.更改 Typecho 配置文件 ###
位置就在你Typecho的安装目录里(举例：`/home/wwwroot/你的网站根目录/config.inc.php`)  
把数据库定义参数中 `charset` 的值改为 `utf8mb4`
```php
$db->addServer(array (
    'host'      =>  localhost,
    'user'      =>  'me',
    'password'  =>  'my_password',
    'charset'   =>  'utf8mb4',        #修改这一行
    'port'      =>  3306,
    'database'  =>  '蛤？'
), Typecho_Db::READ | Typecho_Db::WRITE);
```
然后保存更改，就可以愉快地玩耍啦！
最后狗头镇楼🐶 🐶 🐶

  [1]: https://get233.com/archives/show-emoji-in-typecho.html
