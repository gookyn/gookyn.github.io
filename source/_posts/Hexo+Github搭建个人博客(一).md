---
title: Hexo+Github 搭建个人博客 （一）
tags: Hexo
categories: Hexo
---

{% note warning %}
在Github page平台上，使用Hexo搭建属于自己的静态博客
{% endnote %}

本系列分为以下几部分：
* {% post_link Hexo+Github搭建个人博客(一) 一、基础配置篇 %}
* {% post_link Hexo+Github搭建个人博客(二) 二、发布更新篇 %}
* {% post_link Hexo+Github搭建个人博客(三) 三、主题配置篇 %}
* {% post_link Hexo+Github搭建个人博客(四) 四、标签分类篇 %}


## 基础配置篇

### 安装 Node.js
* 下载：[Node.js官网](https://nodejs.org/en/)
* 安装：下载完成后双击安装，一路next，在Custom Setup这一步记得选 ` Add to PATH ` ，自动配置环境变量
* 查看版本：安装完成后，打开命令行窗口  ` node -v ` 查看版本

<!-- more -->

### 安装 Git
* 下载：[Git官网](https://git-scm.com/download/)
* 安装：一路next，在添加环境变量一步注意选择相对应系统
* 查看版本：安装完成后，打开命令行窗口  ` git --version ` 查看版本

### 搭建 Github
* 注册：[Github官网](https://github.com/)
* 创建仓库：点击 "New repository"，新建一个版本库
* 输入仓库名： yourname.github.io (这个就是自己博客的域名，yourname 必须与注册用户名一致，否则就不能使用Github Pages服务) 
* 启用GitHub Page：进入新仓库的 Settings，在Options项中找到GitHub Pages配置，选择初始主题并保存发布
* 配置Github账户信息：在命令行窗口输入以下命令（YourName 和 YourEail都替换成自己的，如果新用户没有添加邮箱，需要在个人设置中进行验证）

```
git config --global user.name "YourName" 
```
```
git config --global user.email "YourEmail" 
```

### 配置 SSH
* 生成公钥

```
ssh-keygen -t rsa -C "Github的注册邮箱地址"
```

* 添加公钥到 Github  
  + 一路Enter，待秘钥生成完毕，会得到两个文件 id_rsa 和 id_rsa.pub，并且在命令行返回的信息中可以找到文件的位置，如果找不见，可以在“我的电脑”全局搜索
  + 打开 id_rsa.pub，复制所有内容
  + 进入Github，右上角 头像 -> Settings -> SSH and GPG keys -> New SSH key
  + 把公钥粘贴到 key 中，title 可以自定义，最后  Add SSH key
  + 在命令行中验证是否添加成功：

	```
	ssh -T git@github.com
	```
  
### 安装 Hexo
* 新建文件夹，存放自己的博客（本文以 blog 文件夹示例）
* 安装Hexo： 命令行窗口进入blog文件夹

```
npm install -g hexo-cli
```

```
npm install hexo --save
```

* 验证是否安装正确

```
hexo -v
```

* 初始化

```
hexo init
```

* 安装依赖

```
npm install
```

### 配置 Hexo
* 打开 _config.yml 文件，找到如下代码并修改

```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://yourname.github.io（yourname 替换为自己的）
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git（yourname 替换为自己的）
  branch: master
```

* 清除缓存

```
hexo clean
```

### 本地运行 Hexo

* 运行hexo（以后每次在本地运行只要输入该命令即可）

```
hexo s -g
```

* 在浏览器输入 localhost:4000，惊艳的时刻到了~

* 停止运行
Ctrl + C 即可

{% note warning %}
本地所有配置完成后，下一步我们尝试发布博客
{% endnote %}
