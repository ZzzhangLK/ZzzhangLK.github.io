---
title: jsDelivr大面积失效，个人站点该怎么办？
date: 2022-05-18 12:12:12
categories: "Thoughts"
tags:
	- 随想
	- blog
	- 图床
---
jsDelivr的官网还曾经介绍“jsDelivr是唯一有中国ICP许可的公开CDN，并在大陆有非常多的节点”，现在看来，它可能就要逐渐离开了。

<!--more-->

### jsDelivr 简介

jsDelivr曾经是最火的前端静态文件库，也是各个站点喜欢用的静态文件CDN，甚至他们自己也推出了新的服务：[esm.run](https://www.jsdelivr.com/esm)，可以直接在module中使用类似 `import crypto-js from 'https://esm.run/crypto-js'` 的方式快速导入依赖，同时还可以使用到CDN。

说的有点多，某天一觉醒来发现自己的ISP的DNS服务器直接将 `cdn.jsdelivr.net` 解析成了 `0.0.0.0`，然后我就明白了，jsd在我家这里也要开始不能使用了。最近在不少博客之间逛，经常发现有博友出现这种情况，估计jsd正在逐步在全国失效。如果你的DNS把 `cdn.jsdelivr.net` 解析成 `127.0.0.1` 或者 `0.0.0.0`，那么说明你的地区jsd也被污染了。可以通过更换本机DNS解决 **你的问题** 。

![DNS污染情况](https://fastly.jsdelivr.net/gh/ZzzhangLK/Image-Hosting-Service/img/speedTest.png)


jsd的官网还曾经介绍“jsDelivr是唯一有中国ICP许可的公开CDN，并在大陆有非常多的节点”，现在看来，它可能就要逐渐离开了。

![jsDelivr官方网络介绍](https://fastly.jsdelivr.net/gh/ZzzhangLK/Image-Hosting-Service/img/jsdeliver.png)



## jsDelivr 备选站

目前jsDelivr有以下备选站，分别由不同的赞助商提供，目前DNS还没有被污染，使用方法和 `cdn.jsdelivr.net` 相同

* `https://fastly.jsdelivr.net/` 由fastly提供
* `https://gcore.jsdelivr.net/` 由G-Core Labs提供
* `https://testingcf.jsdelivr.net/` 由CloudFlare提供

其实 `cdn.jsdelivr.net` 就是由上述几家服务的综合，只不过在特定情况只解析某一个特定服务商。

至于这三个的速度，请自行去类似boce之类的网站上测试（我这里是fastly最好）。**建议有在使用jsDelivr的站长尽快更换一下，不然肯定会有访客访问不了的。**

**只能说DNS污染比cdn前缀要差一些，还是做好替换到别的服务的准备**

## 前端静态文件CDN备选站

**如果你在寻找前端库的CDN，那么有以下几个CDN站值得一试：**

* [https://www.staticfile.org/](https://www.staticfile.org/) – 由七牛云及掘金提供支持
* [https://www.bootcdn.cn/](https://www.bootcdn.cn/) – 由极兔云联合BootStrap中文网提供
* [https://cdn.bytedance.com/](https://cdn.bytedance.com/) – 字节跳动提供，内容与cdnjs一致
* [https://www.sourcegcdn.com/](https://www.sourcegcdn.com/) – 由 [AHDark](https://ahdark.com/) 创立，支持npm及GitHub（白名单）
* [https://cdnjs.com/](https://cdnjs.com/) – 由CloudFlare等提供支持
* [https://unpkg.com/](https://unpkg.com/) – 也是CloudFlare提供支持，**仅限npm包**

> 以上站点可能对于一些包的更新不是那么及时，所以jsd如果没有大面积不可用，还是可以作为最好的选择的。

 **如果你是要加速Github文件** ，那么我目前还没有找到很好的替代，因为jsd真的太方便了。

 **如果你是要加速自己的=个人=图片等资源** ，那么你 `alt+f4` 赶快光速离开，因为这种公共静态文件CDN根本不是给你这种为了个人用的好吧，至少在我自己的思考里这样做就是浪费公共资源。（例如在npm官方发包来当图床的，分明就是在污染npm好吧）。

[查看原文](https://blog.orangii.cn/2022/jsdelivr-alt/)