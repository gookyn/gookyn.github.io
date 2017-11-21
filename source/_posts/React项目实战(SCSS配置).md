---
title: React项目实战（SCSS 配置）
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

## SCSS 配置

create-react-app 做了很多事，却去掉了默认支持的 Sass 等预处理器。虽然官方文档提供了[配置方法](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-css-preprocessor-sass-less-etc)，按照说明折腾了一番后，不仅麻烦，出错率还高。因此就问了问度娘，发现可以这样做：

<!-- more -->

### 安装依赖
```
npm install sass-loader node-sass --save-dev
```

### 修改 webpack.config.dev.js
在 node_modules/react-scripts/config 下找到 webpack.config.dev.js 文件

在 exclude  添加
 ```
 /.scss$/
 ```

在 loaders 添加
```
{
    test: /\.scss$/,
    loaders: ['style-loader', 'css-loader', 'sass-loader'],
},
```

<img src="/images/React项目实战/scss配置.png"/>

哦了！！！

{% note warning %}
如果要在生产环境中生效，还需要在 webpack.config.prod.js 做同样的配置。
{% endnote %}
