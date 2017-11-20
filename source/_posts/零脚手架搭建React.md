---
title: 零脚手架搭建 React
tags: React
categories: React
---

## 基本配置
### 开发环境
在搭建过程中，可以使用 npm 构建依赖。因此首先需要 [node.js](https://nodejs.org/zh-cn/)，直接下载安装即可。

### 新建项目
新建一个项目，结构如下：

![](/images/零脚手架搭建React/项目结构-初始化.png)

<!-- more -->

app: 开发目录
  * components 用来存放组件
    * Hello.js 第一个组件文件
  * main.js 整个项目的入口文件
  
build: 打包后的目录
  * index.html 是最终要展示的页面
  ```
	<!DOCTYPE html>
	<html>
	  <head>
	      <meta charset="UTF-8">
	      <title>react</title>
	  </head>
	  <body>
	      <div id="root">

	      </div>
	  </body>
	  <script type="text/javascript" src="bundle.js"></script>
	</html>  
  ```
  
  * bundle.js：打包后的js文件
  
### 创建 package.json
命令行窗口进入项目根目录，执行以下命令生成  package.json 文件
```
npm init
```
执行过程中需要输入基本信息，也可以一路默认回车

![](/images/零脚手架搭建React/init.png)

完成后即可在根目录中看到

### 安装依赖
```
npm install --save react

npm install --save react-dom  处理virtual DOM

npm install --save-dev webpack  模块打包工具

npm install --save-dev webpack-dev-server  支持热加载

npm install --save-dev babel-core babel-loader  babel-preset-es2015 babel-preset-react  安装babel相关，编译js
```

### 配置 webpack
在根目录下新增 webpack.config.js

```
var webpack = require('webpack'); //引入Webpack模块供我们调用，这里只能使用ES5语法，使用ES6语法会报错

//__dirname是node.js中的一个全局变量，它指向当前执行脚本所在的目录
module.exports = { //注意这里是exports不是export
	devtool: 'eval-source-map', //生成Source Maps,这里选择eval-source-map
	entry: ['webpack/hot/dev-server', __dirname + '/app/main.js'], //唯一入口文件,__dirname是node.js中的一个全局变量，它指向当前执行脚本所在的目录
	output: { //输出目录
		path: __dirname + "/build", //打包后的js文件存放的地方
		filename: "bundle.js" //打包后的js文件名
	},

	module: {
		//loaders加载器
		loaders: [{
				test: /\.(js|jsx)$/, //一个匹配loaders所处理的文件的拓展名的正则表达式，这里用来匹配js和jsx文件（必须）
				exclude: /node_modules/, //屏蔽不需要处理的文件（文件夹）（可选）
				loader: 'babel-loader' //loader的名称（必须）
			}
		]
	},

	plugins: [
		new webpack.HotModuleReplacementPlugin() //热模块替换插件
	],

	//webpack-dev-server配置
	devServer: {
		contentBase: './build', //默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录
		historyApiFallback: true, //在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html
		inline: true, //设置为true，当源文件改变时会自动刷新页面
		port: 9000, //设置默认监听端口，如果省略，默认为"8080"
		open: true, // 自动打开浏览器
	}
};
```

### 配置 package.json
修改 package.json，在 `scripts` 添加以下代码，实现热加载:

```
"scripts": {
  "start": "webpack",
  "dev": "webpack-dev-server"
}
```

### 配置 Babel
Babel 可以在 `webpack.config.js` 中配置，但由于其配置项比较复杂，因此可以放在一个单独的文件中，webpack会自动调用.babelrc里的babel配置选项。
在项目根目录下新建 `.babelrc` 文件，注意该文件名只有后缀。

```
//.babelrc
{
  "presets": [
    "react",
    "es2015"
  ]
}
```

### 编辑 React组件并引入
编辑 `app/components/Hello.js`

```
import React from 'react';

class Hello extends React.Component {
	render() {
		return(
			<div> Hello React! </div>
		)
	}
}

export default Hello;
```

编辑 `app/main.js`

```
import React from 'react';
import ReactDom from 'react-dom';
import Hello from './components/Hello.js';

ReactDom.render(
    <Hello />,
    document.getElementById('root')
);
```

### 启动项目
```
npm run dev
```

Hello React！


{% note %}
更多可以参考这两篇文章：
[从零开始，教你用Webpack构建React基础工程](http://www.jianshu.com/p/4df92c335617)
[从零开始搭建一个react项目](http://www.jianshu.com/p/324fd1c124ad)
{% endnote %}