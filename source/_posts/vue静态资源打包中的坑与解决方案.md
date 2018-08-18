---
layout: posts
title: vue静态资源打包中的坑与解决方案
date: 2018-08-18 23:15:32
categories: vue
tags: 
    - vue
    - vue-cli
---

### vue静态资源打包中的坑与解决方案
---

#### 前言

 > 记录一下在使用 vue-cli 时的坑，vue-cli 脚手架生成的默认打包配置文件情况下运行 npm run build || yarn build 打包后静态文件出现404

![2018-08-18-23-21-17](http://ox54z18lh.bkt.clouddn.com/2018-08-18-23-21-17.png )

以上图，就是出现的问题

如何解决：
    vue-cli 官方文档已经说明。请看图。
    ![2018-08-18-23-26-27](http://ox54z18lh.bkt.clouddn.com/2018-08-18-23-26-27.png )
    vue 官方文档说明是在 vue.config.js 将 baseUrl 修改
    按照官方文档修改，发现再次运行build时，就没有出现静态文件404
    



