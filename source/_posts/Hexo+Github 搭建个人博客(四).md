---
title: Hexo+Github 搭建个人博客 （四）
tags: Hexo
categories: Hexo
---
在Github page平台上，使用Hexo搭建属于自己的静态博客

本系列分为以下几部分：
* 基础配置篇
* 发布更新篇
* 主题配置篇
* 标签分类篇


## 标签分类篇

### 添加标签
在前面，我们修改过 next 主题下 _config.yml 文件中的menu选项，添加了标签选项，但点击发现是个未找到的页面，慌不？

* 新建标签页面
命令行进入 blog 文件夹，输入以下命令，完成后会看到 blog/source 目录下新建了 tags 文件夹，以及其下的 index.md 文件
```
hexo new page tags
```

* 设置页面类型
打开新生成的 index.md 文件，添加 `type: "tags"`，如下
```
---
title: 标签
date: 2017-10-28 16:56:53
type: "tags"
---
```

* 设置具体文章的标签
要为某一篇文章添加标签，只需要在 blog/source/_posts 找到该文章，然后添加 tags，如 hello.md（如果想添加多个标签 `tags: [标签一，标签二]` 即可）
```
---
title: hello
date: 2017-10-27 16:32:13
tags: 标签一
categories: 
---
```

刷新页面再看一下，稳不？(*￣︶￣)

### 添加分类
这下添加分类就简单了，步骤类似

* 新建分类页面
```
hexo new page categories
```

* 设置页面类型
打开 blog/source/categories 下的 index.md 文件，添加 `type: "categories"`，如下
```
---
title: 分类
date: 2017-10-27 16:56:07
type: "categories"
---
```

* 设置具体文章的分类
在 blog/source/_posts 找到该文章，然后添加 categories，如 hello.md
```
---
title: hello
date: 2017-10-27 16:32:13
tags: 标签一
categories: 分类一
---

```

### 添加关于我
* 新建关于我
```
hexo new page about
```

* 修改 blog/source/about/index.md
```
---
title: about
date: 2017-10-28 16:57:17
---
## 关于我
Stay Hungry. Stay Foolish.
```

---

> 开启博客之旅吧...