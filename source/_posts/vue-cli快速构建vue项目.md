---
title: vue-cli 快速构建 vue 项目
tags: vue
categories: vue
---

## 安装 node.js

下载并安装 [node.js](https://nodejs.org/en/)
安装完成之后，在命令行输入 ` node -v `，如果出现相应版本号，则证明安装成功

## 安装淘宝镜像
```
npm install -g cnpm --registry=http://registry.npm.taobao.org
```
 
## 安装 vue-cli
```
cnpm install -g vue-cli 
```
安装完成后，检验是否安装成功
```
vue -V (在此注意V为大写)
```

## 构建 vue 项目
选定要存放 vue 项目的位置，在命令行窗口进入该目录，初始化项目（demo 是项目的名称）
```
vue init webpack demo
```
运行过程中，会提示输入基本信息，如项目名称，描述，作者等，可以自定义或者跳过

{% note warning %}
注意： 
1、当提示 ` Use ESLint to lint your code?` 时，指是否使用 ESLint 规范代码，由于 ESLint 较为严格，一个空格错误都将报错，如果想避免不必要的麻烦可以选择 No
2、后两项为单元测试，可以选择 No
{% endnote %}

结束之后，会自动生成 demo 文件夹

![](/images/vue-cli快速构建vue项目/初始文件结构.png)

项目的结构框架已经完成，但项目需要依赖的资源还没有安装

## 安装依赖
打开 demo 根目录下的 package.json

![](/images/vue-cli快速构建vue项目/项目依赖.png)

命令行窗口进入 demo 文件夹，安装项目所需的依赖
```
cnpm install
```
完成之后，会生成 node_modules 文件夹，这就是我们项目需要的依赖包资源

![](/images/vue-cli快速构建vue项目/安装依赖后文件结构.png)

依赖包安装成功之后，我们试着运行一下项目

## 运行项目
在项目命令行窗口，执行
```
npm run dev 
```
等待运行完成，会自动打开浏览器 localhost:8080（如果没有自动打开，可以先手动输入）
如果浏览器打开之后，没有加载出页面，有可能是本地的 8080 端口被占用，打开 config > index.js，修改端口号（建议将端口号设为不常用端口）

![](/images/vue-cli快速构建vue项目/修改端口.png)

![](/images/vue-cli快速构建vue项目/运行成功.png)

如果看到这个页面，恭喜，第一个 vue 项目运行成功！