---
title: vue项目脚手架说明
tags: vue
categories: vue
abbrlink: 35aee3e
date: 2018-05-17 18:20:29
---

## Vue 项目脚手架搭建说明

1.  **安装 node.js 支持；**

    > node.js 安装步骤不于此过多赘述。

2.  **npm 安装 vue-cli 脚手架并创建项目；**

```cmd
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖...
$ cd my-project
$ npm start
```

**创建初始化项目时选项部分说明:**

> **【Project name (vuetest)】** 项目名称，可以自己指定，也可直接回车，按照括号中默认名字（注意这里的名字不能有大写字母)
> **【Project description (A Vue.js project)】** 项目描述，也可直接点击回车，使用默认名字
> **【Author】** 作者
> **【Runtime + Compiler】** 运行时编译
> **【Install vue-router? (Y/n)】** 是否安装 vue-router，官方的路由，大多数情况下都会使用,y+回车
> **【Use ESLint to lint your code? (Y/n)】** 是否使用 ESLint 管理代码，ESLint 是个代码风格管理工具，是用来统一代码风格的，并不会影响整体的运行，为了多人协作，一般项目中都会使用。
> **【Standard】** 代码风格标准
> **【Setup unit tests with Karma + Mocha? (Y/n)】** 是否安装单元测试
> **【Setup e2e tests with Nightwatch(Y/n)?】** 是否安装 e2e 测试
> **【should we run `npm install` for...】** NPM 和 YARN 选项

**依赖例举：**

> **【jquery】** $ npm install jquery
> **【bootstrap】** $ npm install bootstrap
> **【bootstrap-webpack】** $ npm install bootstrap-webpack
> **【style-loader】** $ npm install style-loader

3.  **调整配置文件中 webpack-server-dev 的执行端口**

> 位置: config\index.js

```javascript
module.exports = {
  dev: {
    // ...
    host: "localhost",
    port: 8089
    // ...
  }
};
```

4.  **测试路由 router 对组件的支持**

> 位置: src\router\index.js

```javascript
// 添加测试用组件
const page1 = { template: `<p>this is page1..</p>` };
const page2 = { template: `<p>this is page2..</p>` };
// 装载入路由对象并返回
export default new Router({
  routes: [
    {
      path: "/",
      name: "HelloWorld",
      component: HelloWorld
    },
    {
      path: "/page1",
      name: "page1",
      component: page1
    },
    {
      path: "/page2",
      name: "page2",
      component: page2
    }
  ]
});
```

> 修改 App.vue ,添加路由支持(模拟导航)

```html
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-link to="/">Home</router-link>
    <router-link to="/page1">page1</router-link>
    <router-link to="/page2">page2</router-link>
    <router-view/>
  </div>
</template>
```

> 通过 npm start 启动服务器, 在浏览器观察路由结果

5.  **添加自定义组件**
    > 以 HelloWorld.vue 为参考, 单组件主要划分为以下三部分

```vue
<template>
  <!-- 模板部分内容 -->
</template>

<script>
// js部分内容
</script>

<style>
/* 样式部分内容 */
</style>
```

> **template** 部分模板支持所有 vue 在 html 标签部分的动作应用

```html
<template>
  <section class="col-md-8 col-md-offset-2">
      <form class="form-inline">
          <div class="form-group">
            <label class="sr-only" for="exampleInputAmount">Amount (in dollars)</label>
            <div class="input-group">
              <div class="input-group-addon">找歌:</div>
              <input type="text" v-model="searchKey" class="form-control" id="songController" placeholder="请输入歌手名或歌曲名">
            </div>
          </div>
          <button type="submit" class="btn btn-primary" v-on:click="foo">查询</button>
      </form>
      <table class="table">
        <tr class="active">
          <th>ID</th><th>歌名</th><th>歌手</th><th>播放</th>
        </tr>
        <tr v-for="o in arr" class="">
          <td>{{o.singerid}}</td><td>{{o.songname}}</td>
          <td>{{o.singername}}</td>
          <td>
            <audio :src="o.m4a" controls></audio>
          </td>
        </tr>
      </table>
  </section>
</template>
```

> **script** 部分主要向调用者返回当前组件的实例

```javascript
import $ from "jquery";
export default {
  name: "ABTable",
  methods: {
    foo: function() {
      // 调用jquery的ajax发送请求
      $.post(
        "http://route.showapi.com/213-1",
        {
          showapi_appid: "3***1",
          showapi_sign: "3c1a7*****************b624",
          keyword: this.searchKey
        },
        res => {
          // success 回调函数
          console.log("返回歌曲信息: ", res);
          // 解析并重构数组
          if (res.showapi_res_code == 0) {
            var newArr = res.showapi_res_body.pagebean.contentlist;
            this.arr = newArr;
          }
        }
      );
    }
  },
  data: function() {
    return {
      arr: [
        { singerid: 11, songname: "薛之谦", singername: "暧昧" },
        { singerid: 12, songname: "薛之谦", singername: "丑八怪" },
        { singerid: 13, songname: "薛之谦", singername: "演员" }
      ],
      searchKey: "" // 绑定用户输入的搜索框
    };
  }
};
```

> **style** 部分是组件样式, 可以封装样式不污染其他组件

```css
<style scoped>
  section{

  }
  th,td{
    text-align: center;
  }
</style>
```

6.  **将添加的新组件置入路由对象**
    > 路由位置: src\router\index.js

```javascript
import ABTable from "@/components/ABTable";
export default new Router({
  routes: [
    {
      path: "/",
      name: "HelloWorld",
      component: HelloWorld
    },
    {
      path: "/table",
      name: "ABTable",
      component: ABTable
    }
  ]
});
```

> 顺带一提, 别忘了 App.vue 中修改导航

```html
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-link to="/">Home</router-link>
    <router-link to="/table">table</router-link>
    <router-view/>
  </div>
</template>
```

7.  **发布版导出**
    > 命令行输入`npm run build`用于输出项目至发布版

```cmd
$ npm run build
```

> 当前版本的 vue-cli 在生成发布项目时有一个小 bug,目录的引用都是以"/"为开头,因此生成的静态文件引用是这样的:

```html
<script type=text/javascript src=/static/js/manifest.82563f550e3364c6fa20.js></script>
<script type=text/javascript src=/static/js/vendor.32e48b8738cad2a0755b.js></script>
<script type=text/javascript src=/static/js/app.4c9592f4bed760201e6e.js></script>
```

但是项目目录结构是这样的:

> -- dist
> ----static
> ------- + css
> ------- + fonts
> ------- + img
> ------- + js
> ----index.html

有些浏览器不支持这样的路径引用, 因此可以更改配置文件中关于生成的目录定义:

> 配置文件位置: config\index.js
> 修改 build 对象中的 `assetsPublicPath` 属性为`./`

简单的生产环境 Vue 使用实例，仅用于抛砖引玉。
