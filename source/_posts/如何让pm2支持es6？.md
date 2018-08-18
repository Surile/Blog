---
layout: posts
title: 如何让pm2支持es6？
date: 2018-08-18 16:02:30
categories: node 
tags: 
    - node
    - pm2
---

#### 前言

 > 就如同标题一样如何让pm2支持es6，现在 es6已经是满天飞了，写 node 的时候肯定也要用es6咯，所以才会出现如何将pm2支持es6。

#### PM2官网介绍

![PM2官网介绍](https://wx2.sinaimg.cn/large/006a7eb0gy1fudx270iqrj30jg07gt9o.jpg "PM2官网介绍")

问题是我按照官网介绍使用之后还是不能让 pm2 支持es6，好烦。。。

重新看了下官网文档，看到有这么一段

![PM2](https://ws4.sinaimg.cn/large/006a7eb0gy1fudxcy6thcj30jg03mq3u.jpg "pm2")

其实这段代码 我在对应的ecosystem.config.js（简单说 就是pm2的配置文件，类似npm的package.json）里写过对应的，但没起作用。看到这我才明白为啥没起作用： `pm2默认服务是负载均衡的，注意红框中的话：没错，它只能工作在fork模式下。`所以我那就一直没跑起来。

但如果不用负载均衡，感觉又何必用pm2呢？它的优势不就是这个吗？所以又往下看。果然，pm2提供了解决方法：

单写一个js文件，内容如下。pm2 直接运行这个文件即可。既解决了koa的es6编译的问题，又能使用pm2的负载均衡。`以下内容只能解决 import`

![Import](https://wx3.sinaimg.cn/large/006a7eb0gy1fudxha82aqj30jg05q0t6.jpg "import")

运行起来，PM25的状态是 online，但是查看 PM2 logs 时发现报错了。特么我以为以上命令是支持所有 es6，没想到却报以下错误

![2018-08-18-16-38-29](http://ox54z18lh.bkt.clouddn.com/2018-08-18-16-38-29.png "2018-08-18-16-38-29")

wtf，竟然又报错。我就去谷歌，找了半天找到了。


还是一样单写一个 js，内容如下。pm2直接运行这个文件就可以了。

![2018-08-18-16-39-42](http://ox54z18lh.bkt.clouddn.com/2018-08-18-16-39-42.png "2018-08-18-16-39-42")

我在去查看 logs 时，终于没在报错了。(*^▽^*)。完美的解决了我的问题。hhhhhh








