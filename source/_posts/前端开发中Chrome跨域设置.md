---
title: 前端开发中 Chrome 跨域设置
tags: 浏览器
categories: 浏览器
---
{% note warning %}
在前端开发中，经常会遇到浏览器跨域。除了后端配置跨域权限，Chrome 也可以通过更改设置解决跨域问题。
{% endnote %}

## 操作系统
Windows

## 浏览器版本
49版本之后

## 设置方式
* 在电脑上新建一个文件夹，例如：D:\MyChrome

* 在谷歌快捷方式上右击，在下拉列表中选择“属性”。

* 打开属性窗口，切换到“快捷方式”选项卡

* 在“目标”路径后添加，如果目标中的路径含有双引号，则在双引号的外面添加
注意：- -allow前要有一个空格，最后的路径设置成为自己新建文件夹的路径
```
 --allow-file-access-from-files --disable-web-security --user-data-dir=D:\MyChrome
```
![](/images/前端开发中Chrome跨域设置/属性窗口.png)


* 点击应用和确定后关闭属性页面，并重新打开Chrome浏览器，此时发现有“--disable-web-security”相关的提示，说明chrome可以跨域了
![](/images/前端开发中Chrome跨域设置/跨域成功.png)

跨域成功后，首页换成了google的welcome页面，同时原来收藏的链接和历史记录都不见了，而 D:\MyChrome 目录下则生成了个人信息相关的文件。