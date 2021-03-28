---
title: Sentry性能监控配置
tags: Sentry
categories: Sentry
---

Sentry除了异常监控之外，还集成了强大的性能监控，可以测量吞吐量、延迟等指标。通过监视应用程序的性能，可以查看一项服务中的延迟如何影响另一项服务，从而准确定位线上服务问题。


## 一、安装依赖
```shell
$ yarn add @sentry/tracing -S
```


## 二、配置初始化

```javascript
import { Integrations as TracingIntegrations } from '@sentry/tracing'; // 性能监控

Sentry.init({
  ...
  tracesSampleRate: 1.0, // 配置事件的采样率，范围为0.0至1.0，默认值是1.0指发送100％的错误事件
  // 配置集成
  integrations: [
    new TracingIntegrations.BrowserTracing(), // 性能监控
  ],
});
```


tracesSampleRate：设置采样率，将对应比例的交易发送到Sentry。（比如要发送20％的交易，请设置tracesSampleRate为0.2）


## 三、验证
配置完成后，重新访问页面，然后就可以在 Sentry Performance 面板看到页面加载情况。


详情中可以找到所有请求访问、资源加载、前端console日志，以及设备、操作系统等详细信息。
![性能监控1.png](https://cdn.nlark.com/yuque/0/2021/png/12735713/1616859495709-ffa91a63-e4db-4d12-b63e-31fa12c26de5.png#align=left&display=inline&height=937&margin=%5Bobject%20Object%5D&name=%E6%80%A7%E8%83%BD%E7%9B%91%E6%8E%A71.png&originHeight=937&originWidth=1920&size=188959&status=done&style=shadow&width=1920)![性能监控2.png](https://cdn.nlark.com/yuque/0/2021/png/12735713/1616859503122-1ab7490a-03cc-432e-ad9b-afc10b02c9fc.png#align=left&display=inline&height=937&margin=%5Bobject%20Object%5D&name=%E6%80%A7%E8%83%BD%E7%9B%91%E6%8E%A72.png&originHeight=937&originWidth=1901&size=696390&status=done&style=shadow&width=1901)
