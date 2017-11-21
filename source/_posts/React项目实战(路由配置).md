---
title: React项目实战（路由配置）
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

## 路由配置

在准备开发的时候，发现 React Router 已经更新到了 4.0，但最重要的是：4.0竟然不向下兼容，而且还拆分出了 `react-router`、`react-router-dom`、`react-router-native`、`react-router-redux` 等多个相互独立的包。

这...好吧，我们还是跟上潮流就好！ 

{% note warning %}
在使用路由的整个过程中，务必深深理解一句话：路由即组件！
{% endnote %}

<!-- more -->

### 选择
刚起步就遇到问题了：该选择 `react-router` 还是 `react-router-dom` 呢？

对于这个问题，搜索了很久，也没有发现特别好的解释。不过很多朋友说只要 `react-router-dom` 就好了，于是就大胆尝试了一把。没想到还不错，目前的需求竟然都满足了。

### 安装
```
npm install react-router-dom --save
```

### 配置
很多朋友将路由直接配置在了入口文件，但这样做可能会出现一个问题：当路由配置过多时，入口文件会特别难维护。

因此，可以尝试将路由的相关配置直接独立出来，在需要的时候引入即可，这样整个项目结构会特别清晰。

{% note warning %}
以下会出现很多陌生标签，只要晓得都是 react-router-dom 特有的标签就好。
{% endnote %}

在 src 目录下新建 router 目录，并在其下创建 index.js 文件，用于存放所有的路由配置。文件基本内容大致如下：

```
// 路由
import React from 'react';
import { Route, Redirect } from 'react-router-dom';

import Home from './../pages/home/home';
import Subscribe from './../pages/subscribe/subscribe';
import User from './../pages/user/user';

class Router extends React.Component {
  render() {
    return(
      <div className="page-wrapper">
        <Route exact path="/" render={() => (
          <Redirect to="/home"/>
        )}/>
        <Route path="/home" component={ Home }/>
        <Route path="/subscribe" component={ Subscribe }/>
        <Route path="/user" component={ User}/>
       </div>
    );
  }
}

export default Router;
```

细心的朋友会发现：这不就是一个 React 组件吗？
对了，不知还记得否：路由即组件！


### 链接
这里，我在 components 下有一个 tab.js，作用类似于手机App的 tab。内容很简单，就是一些路由的链接：

```
// tab
import React from 'react';
import { Link } from 'react-router-dom';

class Tab extends React.Component {
  render() {
    return(
      <div className="tab-wrapper">
        <Link to="/home" className="tab-item t-c">首页</Link>
        <Link to="/subscribe" className="tab-item t-c">订阅</Link>
        <Link to="/user" className="tab-item t-c">我的</Link>
      </div>
    );
  }
}

export default Tab;
```

### 引入
既然路由是组件，那引入就很简单了。

比如直接将路由组件引入到主容器 app.js 中：
```
// 主容器
import React from 'react';
import { BrowserRouter } from 'react-router-dom';
import Router from './../../router';
import Tab from './../../components/tab';

class App extends React.Component {
  render() {
    return(
      <div id="app">
        <BrowserRouter>
          <div className="main-wrapper">
            <Router />
            <Tab />
          </div>
        </BrowserRouter>
      </div>
    );
  }
}

export default App;
```

{% note warning %}
特别注意：
`Route`、`Redirect`、`Link` 等标签必须在同一个 `BrowserRouter` 之中。
{% endnote %}

这样，一个简单的路由就完成了。

{% note %}
关于更多的用法，可以参考这几篇文章：
[初探 React Router 4.0](http://www.jianshu.com/p/e3adc9b5f75c)
[ReactRouter升级 v2 to v4](https://www.cnblogs.com/libin-1/p/7067938.html)
[react-router-dom v4](http://www.cnblogs.com/dudeyouth/p/6617059.html)
{% endnote %}