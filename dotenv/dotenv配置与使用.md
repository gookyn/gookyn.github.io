---
title: dotenv配置与使用
tags: dotenv
categories: dotenv
---

## 简介


将环境变量从 .env 文件加载到中 process.env


## 项目中的作用


- 统一管理各个环境下的全局变量
- 方便切换环境



## 安装及配置


### 安装依赖


```shell
# with npm 
$ npm install dotenv --save-dev
 
# or with Yarn 
$ yarn add dotenv -D
```


### 配置 .env 文件


示例如下：


```
# DEV 环境变量配置

# 环境名
ENV=dev

# 接口域名
API_HOST=https://api.dev.com

# 站点域名
SITE_HOST=https://dev.com
```


### 引入dotenv


在 webpack/config.js 中引入dotenv


```javascript
// 使用 dotenv 设置环境变量，注意 dotenv 调用应该尽量靠前，在使用 process.env 之前调用
const dotenv = require('dotenv');
```


### 读取配置文件


可以校验文件是否存在，以及读取后自定义操作


```javascript
const envConfig = dotenv.config({
    path: path.resolve(__dirname, '../.env'), // 配置文件路径
    encoding: 'utf8', // 编码方式，默认utf8
    debug: false, // 是否开启debug，默认false
}).parsed;

if (!envConfig) {
    console.log('配置文件不存在');
    // 退出程序
    process.exit(1);
}
```


### 区分环境


如果需要区分环境，可以配置不同环境的 .env 文件，然后在 webpack/config.js 中根据环境读取对应配置文件。示例如下（此处通过启动命令来区分环境，具体区分方式可根据业务自行配置）：


```json
// package.json
{
    ...
    "scripts": {
		"start:dev": "cross-env CURRENT_ENV=dev webpack-dev-server --config webpack/dev.js --progress --mode development",
		"start:test": "cross-env CURRENT_ENV=test webpack-dev-server --config webpack/dev.js --progress --mode development",
		"start:prod": "cross-env CURRENT_ENV=prod webpack-dev-server --config webpack/dev.js --progress --mode development",
		"build:dev": "cross-env CURRENT_ENV=dev webpack --config webpack/build.js --mode production",
		"build:test": "cross-env CURRENT_ENV=test webpack --config webpack/build.js --mode production",
		"build:prod": "cross-env CURRENT_ENV=prod webpack --config webpack/build.js --mode production"
	},
	...
}
```


env文件目录参考：


```
.
|-- webpack
  |-- env
    |-- .env.dev
    |-- .env.test
    |-- .env.prod
  |-- build.js
  |-- config.js
  |-- dev.js
```


webpack/config.js 配置修改：


```javascript
// webpack/config.js
// 环境变量配置
const envConfigPath = {
    dev: path.resolve(__dirname, './env/.env.dev'), // 开发环境配置
    test: path.resolve(__dirname, './env/.env.test'), // 测试环境配置
    prod: path.resolve(__dirname, './env/.env.prod'), // 正式环境配置
};

//...

const envConfig = dotenv.config({
    path: envConfigPath[process.env.CURRENT_ENV], // 配置文件路径
    ...
}).parsed;
```


### 包装 dotenv 和 webpack.DefinePlugin


.env文件配置好后，还需要将变量同步为全局环境变量，这样才能在业务场景中使用。这里可以借助一下 dotenv-webpack 插件。


- 安装webpack插件



```shell
# with npm 
$ npm install dotenv-webpack --save-dev
 
# or with Yarn 
$ yarn add dotenv-webpack -D
```


- webpack/config.js 中引入 dotenv-webpack



```javascript
const DotenvWebpack = require('dotenv-webpack');
```


- webpack/config.js 中添加插件，读取env配置文件， 并包装成全局环境变量，功能类似于 webpack.DefinePlugin



```javascript
plugins: [
    ...
    new DotenvWebpack({
        path: path.resolve(__dirname, '../.env'), // 配置文件路径
        // path: envConfigPath[process.env.CURRENT_ENV], // 根据环境配置文件路径
    }),
    
    // 同时可以配置其他的环境变量
    new webpack.DefinePlugin({
        'process.env.CURRENT_NAME': JSON.stringify(currentName),
    }),
]
```



---



至此，所有的配置都已OK，可以直接在业务中使用了 (￣▽￣)／


## 使用


- 业务逻辑中直接可以通过 `process.env.${变量名}` 使用全局环境变量
- 如果需要切环境，直接执行对应环境的命令即可



## 参考文档


- dotenv：[https://www.npmjs.com/package/dotenv](https://www.npmjs.com/package/dotenv)
- dotenv-webpack：[https://www.npmjs.com/package/dotenv-webpack](https://www.npmjs.com/package/dotenv-webpack)
