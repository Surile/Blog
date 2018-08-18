---
layout: posts
title: '部署项目到 GitHub'
date: 2018-08-18 17:21:48
categories: git 
tags: 
    - git
    - node
---

## git subtree 教程

---

   > 关于子仓库或者说是仓库共用，git官方推荐的工具是git subtree。 我自己也用了一段时间的git subtree，感觉比git submodule好用，但是也有一缺点，在可接受的范围内。所以对于仓库共用，在git subtree 与 git submodule之中选择的话，我推荐git subtree。

### git subtree是什么？为什么使用git subtree

---
   git subtree 可以实现一个仓库作为其他仓库的子仓库。

   ![subtree](https://ws4.sinaimg.cn/large/006a7eb0gy1fudzdrze4mj30m809pjt7.jpg "subtree")

   使用git subtree 有以下几个原因：
   + 旧版本的git也支持(最老版本可以到 v1.5.2).
   + git subtree与git submodule不同，它不增加任何像`.gitmodule`这样的新的元数据文件.
   + git subtree对于项目中的其他成员透明，意味着可以不知道git subtree的存在.
   当然，git subtree也有它的缺点，但是这些缺点还在可以接受的范围内：
   + 必须学习新的指令(如：git subtree).
   + 子仓库的更新与推送指令相对复杂。

### git subtree 的使用
---
   git subtree的主要命令有：
   ```bash
    git subtree add   --prefix=<prefix> <commit>
    git subtree add   --prefix=<prefix> <repository> <ref>
    git subtree pull  --prefix=<prefix> <repository> <ref>
    git subtree push  --prefix=<prefix> <repository> <ref>
    git subtree merge --prefix=<prefix> <commit>
    git subtree split --prefix=<prefix> [OPTIONS] [<commit>]
   ```
### 准备
---
   我们先在github 中创建vue-login，然后按照 GitHub 上的提示将项目克隆到本地

   ```bash
        git clone https://github.com/surile/vue-login.git
   ```

   vue-login的路径为https://github.com/surile/vue-login.git，仓库里的文件有：

   ```bash
        vue-login
        |
        |-- build
        |-- pulish
        |-- src
        |-- webpack.common.js
        |-- webpack.prod.js
        |-- webpack.dev.js
        \-- README.md
   ```

   以下操作均位于根目录中。
   我们执行以下命令把master 分支中的 build 文件推送到远程仓库gh-pages分支中：

   ```bash
       yarn build || npm run build
       git checkout -b gh-pages
       git add -f build
       git commit -m 'created gh-pages'
       git subtree push --prefix build origin gh-pages
   ```

   这步做完之后就可以在 GitHub 上看见 gh-pages 分支了。GitHub 会自动部署 gh-pages 里的静态文件。
   点开 Settings，将会看到以下内容。勾出绿色部分则是你的URL
   ![2018-08-18-17-50-53](https://static.mhecy.com/2018-08-18-17-50-53.png "2018-08-18-17-50-53")
   
   当我们修改项目完成之后，肯定是要从前 build 的。这时使用以下命令，则就没用了。
   ```bash
        yarn build || npm run build
        git add -f build
        git commit -m '重新 build'
        git subtree push --prefix build origin gh-pages
   ```
   会出现以下错误。
   ![2018-08-18-17-58-08](https://static.mhecy.com/2018-08-18-17-58-08.png "2018-08-18-17-58-08")

   出现这种错误，根据提示我们可以看出来，是要让我们强行将 build 推送到远程gh-pages 分支上，则可以用以下命令

   ```bash
        git push origin `git subtree split --prefix build master`:gh-pages --force
   ```

   ![2018-08-18-18-02-06](https://static.mhecy.com/2018-08-18-18-02-06.png "2018-08-18-18-02-06")

   运行命令，则可以强制将 build 文件夹推送到远程 gh-pages 分支上了。

   

