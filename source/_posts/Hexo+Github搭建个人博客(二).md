---
title: Hexo+Github 搭建个人博客 （二）
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


## 发布更新篇

### 新建文章
* 在 /source/_posts/ 文件夹下手动新建md文件，或者输入以下命令自动生成（位置：/source/_posts/）

```
hexo new "hello"
```

* 打开 hello.md 文件，进行简单编辑并保存

```
---
title: hello
date: 2017-10-27 16:32:13
---
Hello，hexo！

```

### 发布文章
* 安装hexo git插件

```
npm install hexo-deployer-git --save
```

* 发布

```
hexo d -g
```

* 访问 yourname.github.io

{% note warning %}
博客搭起来了，不过，这个界面。。。还是换一个吧
{% endnote %}