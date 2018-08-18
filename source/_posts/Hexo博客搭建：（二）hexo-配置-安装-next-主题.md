---
title: 'Hexo博客搭建：（二）Hexo 配置,安装 Next 主题'
date: 2018-04-29 01:06:47
tags: 
    - Hexo
    - Next
categories: hexo
---

## 前言

Hexo 所初始化的站点文件夹根目录下的 `._config.yml` 文件声明了网站的配置信息，我们可以通过修改此文件的配置参数以个性化网站。

### 配置文件_config.yml

Hexo 生成的网站的配置信息均保存在 `._config.yml` 文件中，该文件位于站点文件夹下的根目录下。

我们可以通过修改 `._config.yml` 文件中的参数配置来自定义 Hexo 生成的静态站点。

使用文本编辑器打开该文件，我们会发现其中有详尽的参数，这里只选取一部分具有代表性的参数配置进行介绍。详情戳这里 ⇨ [配置](https://hexo.io/zh-cn/docs/configuration.html)。



##### 网站相关

* `title` ：网站标题
* `subtitle` ：网站副标题
* `description` ：网站描述
* `author` ：作者名字

##### 网址相关

* `url` ：网址
* `root` ：网站根目录

##### 文章相关

* `auto_spacing` ：在中文和英文之间加入空格，默认值为 false
* `external_link` ：在新标签中打开链接，默认值为 true
* `render_drafts` ：显示草稿，默认值为 false
* `post_asset_folder` ：开启文章地资源管理文件夹，默认值为 false
* `highlight` ：代码块的设置，包括有 enable、line_number、auto_detect 和 tab_replace 属性可设置。不过一般不用修改，大多数主题都默认是支持代码语法高亮等设置的。

##### 分页相关

* `per_page` ：每页显示的文章数，默认值为 10，值为 0 时会关闭分页功能

### 新建菜单页

Hexo 生成的站点默认菜单也有限，如果我们想自定义添加菜单页该怎么操作呢？比如说，我们想新建一个名为 `Abou` 的菜单页。

站点根目录下，命令行中输入：

```bash
    $ hexo new page "about"
```

上面的命令生效以后，根目录下的 `source` 文件夹中会新增一个名为 `about` 的文件夹，里面有个 `index.md` 文档。我们将想要在 `About` 菜单页中显示的内容，按照博文格式写在这个文档里即可。

然后，修改 `./themes/your-theme-name/_config.yml` 文件中的 `menu` 项，在下面添加一行 `About: /about` 即可。

### 示范
下图是我修改了网站相关配置参数值之后的网站首页，可以很明显地看出与网站标题与副标题发生了变化。

![示范](https://ws4.sinaimg.cn/large/006a7eb0gy1fue0pmz4p5j30mf05cjw1.jpg )

### 安装 Next 主题

* 下载主题
  
  两种方式下载主题，第一种则是 git，第二种则是下载安装包，解压出来。

  + git 方式

  ```bash
    $ cd your-hexo-site
    $ git clone https://github.com/iissnan/hexo-theme-next themes/next
  ```
  + 下载安装包

    ![2018-08-18-18-19-13](http://ox54z18lh.bkt.clouddn.com/2018-08-18-18-19-13.png)

    解压所下载的压缩包至站点的 themes 目录下， 并将 解压后的文件夹名称（hexo-theme-next-0.4.0）更改为 next。

* 主题启用

    与所有 Hexo 主题启用的模式一样。 当 克隆/下载 完成后，打开 站点配置文件， 找到 theme 字段，并将其值更改为 next。

    ```bash
        theme: next
    ```

    到此，NexT 主题安装完成。下一步我们将验证主题是否正确启用。在切换主题之后、验证之前， 我们最好使用 hexo clean 来清除 Hexo 的缓存。

* 验证主题

    首先启动 Hexo 本地站点，并开启调试模式（即加上 --debug），整个命令是 hexo s --debug。 在服务启动的过程，注意观察命令行输出是否有任何异常信息，如果你碰到问题，这些信息将帮助他人更好的定位错误。 当命令行输出中提示出：

    ```bash
        INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
    ```

    此时即可使用浏览器访问 http://localhost:4000，检查站点是否正确运行。

    ![主题](https://ws2.sinaimg.cn/large/006a7eb0gy1fue0ynyk7sj30lf0bxmxx.jpg )

    看到以上图片，则 next 主题安装成功 ⇨ [其他配置](https://theme-next.iissnan.com/getting-started.html#select-scheme)