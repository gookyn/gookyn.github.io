---
title: Hexo+Github 搭建个人博客 （三）
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


## 主题配置篇

{% note warning %}
Hexo初始化之后默认的主题是 landscape，可以在 [Hexo 主题](https://hexo.io/themes/) 找自己喜欢的，我选的是  Next 主题，简单优雅
{% endnote %}

### 安装 Next
下载并拷贝主题文件到站点目录的 themes 目录下，然后修改配置文件

<!-- more -->

### 启用主题

打开 __站点配置文件__（根目录下的 _config.yml 文件），修改（theme 的值与下载的主题文件夹名字相同即可，此处为了方便直接改用 next）

```
theme: next
```

### 验证主题
启用完成后，首先清除缓存（对于之后的设置，如果没有效果，建议先执行该命令再预览）

```
hexo clean
```

开启调试模式

```
hexo s --debug
```

访问 localhost:4000，检查站点是否正确运行

### 选择 Scheme

通过切换 Scheme，可以选择不同的版式，默认是 Muse

打开 __主题配置文件__ （themes/next/_config.yml），搜索 scheme 关键字，选择需要启用的，删除 #，并保证其它都有 # 即可

```
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

### 设置基本信息
编辑 __站点配置文件__ 

```
# Site
title: 博客名
subtitle: 副标题
description: 博客描述
author: 自己的大名啦
language: zh-Hans
timezone: 
```

{% note warning %}
将 language 设置成所需要的语言（如简体中文）。
{% endnote %}

### 设置菜单
* 设定菜单内容，编辑 __主题配置文件__，对应的字段是 menu
```
menu:
  home: / || home
  categories: /categories/ || th
  archives: /archives/ || archive
  tags: /tags/ || tags
  about: /about/ || user
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
```
  
* 设置菜单的显示文本，以简体中文为例，若需要更改菜单项 archives，编辑 
themes/next/languages/zh-Hans.yml ：
```
menu:
  home: 首页
  archives: 记录
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  schedule: 日程表
  sitemap: 站点地图
  commonweal: 公益404
```

* 设定菜单项的图标，编辑 __主题配置文件__，对应的字段是 menu_icons
enable 可用于控制是否显示图标
```
menu_icons:  enable: true
  home: home
  about: user
  categories: th
  tags: tags
  archives: archive
  commonweal: heartbeat
```

### 设置侧栏
修改 __主题配置文件__ 中的 sidebar 字段来控制侧栏的行为
* 设置侧栏的位置，修改 sidebar.position 的值
* 设置侧栏显示的时机，修改 sidebar.display 的值

### 设置头像
编辑 __站点配置文件__，新增字段 avatar，值可以是网络地址或者站点内的地址
* 如果是网络图片，可以为：
```
avatar: http://example.com/avtar.png
```

* 如果是本地图片，需要将头像放置在 themes/next/source/uploads/ （若不存在uploads目录，需要新建） 
配置为：
```
avatar: /uploads/avatar.png
```
  或者放置在 themes/next/source/images/ 目录下 
  配置为：
```
avatar: /images/avatar.png
```

整个界面已经清爽很多，更多个性化设置就得自己探索咯...

{% note warning %}
关于 next 主题的更多配置，可以访问 [next](http://theme-next.iissnan.com/)
{% endnote %}

