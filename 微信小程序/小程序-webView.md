---
title: 小程序-webView
tags: 小程序
categories: 小程序
---

## 概念


> [官方](https://developers.weixin.qq.com/miniprogram/dev/component/web-view.html)介绍：承载网页的容器。会自动铺满整个小程序页面，个人类型的小程序暂不支持使用。



简单理解：web-view 是一个和当前页面一样大小的容器，容器中展示的是 src 引用的网页内容。
类似于 html 中的 iframe ，不同的是 iframe 可以自定义大小、边框等样式，而 web-view 会自动铺满整个页面，并覆盖其他组件。


## 优劣势


### 优势

- 小程序中不支持 html 标签，会限制页面表现形式（比如公众号文章在小程序中展示不太理想）。使用 web-view，可以优化这类型页面的展示。
- 降低开发成本，一套 H5 可以运行于多个环境内（比如公众号、小程序、以及未来的其他应用场景）
- 热更新更加方便，减少因小程序审核周期长而带来的影响
### 劣势

- 引用的网页中资源如果未压缩，会增大消耗流量
- 加载时不如原生小程序流畅
- 普通网页中信息复杂，小程序开放嵌入网页功能，可能会出现鱼龙混杂情况
### 限制

- 个人类型的小程序暂不支持使用
- webview 可打开关联的公众号的文章，其它网页需配置业务域名。域名必须为 经过ICP备案的 https 协议，而且需要从配置后台下载校验文件放在域名根目录下，验证通过后才能配置成功。网页中的 iframe 也要在域名白名单中。一个小程序最多可添加20个域名。
- 一个页面只能放置一个 web-view，且会覆盖其他的组件铺满屏幕
- 目前支持的 jssdk 接口还比较少



## 配置


- 登录[小程序管理后台](https://mp.weixin.qq.com/)，在“开发-开发设置”中配置业务域名
- 如果是第三方开发小程序，需要登录[微信开放平台](https://open.weixin.qq.com/cgi-bin/index?t=home/index&lang=zh_CN)，在“管理中心-第三方平台”中配置



## 引用


在小程序中引入网页地址


```html
<!-- 小程序 wxml -->
<!-- src为域名白名单中的页面路径 -->
<web-view src="https://mp.weixin.qq.com"></web-view>
```


## 通信


### 由小程序到网页


- 小程序通过 url 拼接参数，页面解析 url 获取参数信息



```html
<!-- 小程序 wxml -->
<web-view src="https://mp.weixin.qq.com?id=1"></web-view>
```


### 由网页到小程序


- 网页通过 `wx.miniProgram.navigateTo` 、 `wx.miniProgram.redirectTo` 等方法跳转小程序（支持的接口参考官方文档），参数可拼接在 url 后，小程序页面在 `onLoad` 方法中获取



```javascript
// H5 js
// 跳转小程序页面
wx.miniProgram.navigateTo({
    url:'/pages/detail/detail?id=1',
    success: function(){
        console.log('success');
    },
    fail: function(){
        console.log('fail');
    }
});
```


```javascript
// 小程序 js
/**
 * 小程序页面--监听页面加载
 */
onLoad: function (options) {
    console.log(options) // { id: 1 }
}
```


- 网页通过  `wx.miniProgram.postMessage`  向小程序发送消息，在特定时机（小程序后退、组件销毁、分享）触发组件的 message 事件



```javascript
// H5 js
// 页面向小程序发送消息
wx.miniProgram.postMessage({ data: {foo: 'bar'} })
```


```html
<!-- 小程序 page.wxml -->
<web-view src="{{webViewUrl}}" bindmessage="getMessage"></web-view>
```


```javascript
// 小程序 page.js
getMessage: function(e) {
    // e.detail.data 是多次 postMessage 的参数组成的数组
    console.log(e.detail) // { data: [{foo: 'bar'}] }
}
```


## Tips


- 小程序清除 web-view 缓存，可在 src 添加版本号或者随机参数



```javascript
// 小程序 page.js
// 添加时间戳
this.setData({
    webViewUrl: 'https://mp.weixin.qq.com?id=1&amp;timestamp=' + new Date().getTime()
})
```


- 小程序中刷新网页，可更新 src，但如果直接修改，会增加浏览历史，可以采用条件渲染



```html
<!-- 小程序 page.wxml-->
<web-view wx:if="{{webViewUrl}}" src="{{webViewUrl}}"></web-view>
```


```javascript
// 小程序 page.js
// 首先销毁webView，再重新赋值更新
const tmpUrl = this.data.webViewUrl;
this.setData({
    webViewUrl: ''
}, () => {
    // setData异步执行，否则会提示一个页面只能嵌入一个web-view
    this.setData({
        webViewUrl: tmpUrl
    })
})
```


- 在链接中带有中文字符时，在 iOS 中会有打开白屏的问题，需要 `encodeURIComponent`
- 网页中共享小程序的登录态，可以在 url 中传递 cookie、token 等
- 改变 web-view 的标题，可以在网页中修改 `document.title`
