---
title: npm 常用命令
tags: npm
categories: npm
---

## 安装依赖

* 默认安装最新版本
```
npm install <name>
```

* 安装指定版本
```
npm install <name>@版本号
```

* 将包安装到全局环境中
```
npm install <name> -g  
```

* 安装的同时，将信息写入package.json中
如果项目路径中有package.json文件，直接使用npm install方法就可以根据 dependencies 配置安装所有的依赖包
```
npm install <name> --save  
```

## 初始化
```
npm init  创建一个package.json文件，包括名称、版本、作者这些信息等
```

## 移除
```
npm remove <name>
```

## 更新
```
npm update <name>
```

## 列出已经安装的所有包
```
npm ls 
```

## 查看当前包的安装路径
```
npm root 
```
 查看全局的包的安装路径
```
npm root -g
```

## 帮助
```
npm help
```

## 安装淘宝镜像
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

