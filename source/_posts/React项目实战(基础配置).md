---
title: React项目实战（基础配置）
tags: React
categories: React
---

[React 项目实战](https://github.com/liujinge/react-kaka)，持续完善中...
* {% post_link React项目实战(基础配置) 基础配置 %}
* {% post_link React项目实战(UI组件) UI组件 %}
* {% post_link React项目实战(SCSS配置) SCSS配置 %}
* {% post_link React项目实战(路由配置) 路由配置 %}
* {% post_link React项目实战(HTTP请求) HTTP请求 %}

---

## 基础配置

### 安装 node
下载并安装 [node.js](https://nodejs.org/zh-cn/)

### 安装 create-react-app
使用 React.js 官网推荐的 create-react-app 自动构建，首先进行全局安装：
```
npm install -g create-react-app  
```
<!-- more -->

### 创建项目
```
create-react-app your-project  #your-project替换为自己的项目名称
```
该命令会自动构建项目，并且安装基础依赖。
文件结构如下：

<img src="/images/React项目实战/react-init.png">

* public/index.html  入口页面
* src/index.js  入口文件

{% note warning %}
需要注意的是：
create-react-app 做了很多工作：http服务器配置,自动代开浏览器窗口，react，es6语法编译，babel-core，webpack等等，而且将 webpack.config.js 放在了 node_modules/react-scripts/config 下。
{% endnote %}

### 启动项目
```
cd your-project
npm start
```
成功之后会自动打开浏览器
如果没有自动打开，可以在地址栏手动输入 `http://localhost:3000`

<img src="/images/React项目实战/React-welcome.png">



