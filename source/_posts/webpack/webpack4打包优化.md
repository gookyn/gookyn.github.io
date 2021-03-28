## 一、前言


随着项目迭代，需要的前端依赖、js、css、静态资源等不断增加，代码越来越冗余，导致打包后的文件过大，打包速度也变得越来越慢。最近的项目打包时间已经近5分钟，再加上jenkins部署时的依赖安装、文件上传等，一次构建需要7-8分钟，这样的速度，严重影响到了开发及发版效率。


因此特针对此问题，进行了一次项目构建及打包优化，希望可以提高团队的工作效率。


在开始时，发现项目中用的还是webpack3，原本不想改动太多，尽量减小对项目的影响范围，在webpack3的基础上做了一些优化，但多次对比之后，效果总是不太理想，索性就升级到了webpack4，当然，肯定需要改动很多匹配的依赖、webpack配置等，这里也耗费了很多精力。


关于升级到webpack4，这里不多阐述，具体可以根据项目自行实现。以下主要记录在webpack4基础上配置的一些优化：


## 二、构建帮助工具


在优化之前，可以先了解两个webpack的工具插件，帮助我们更方便地看到优化过程及结果。


### 构建速度检测


`speed-measure-webpack-plugin` [[https://www.npmjs.com/package/speed-measure-webpack-plugin](https://www.npmjs.com/package/speed-measure-webpack-plugin)]


这个插件可以帮助我们检测webpack打包过程中的构建速度，比如可以看到plugins、loaders等的执行时间，以更明确地将精力集中在需要优化的地方。


![打包速度检测.png](https://cdn.nlark.com/yuque/0/2021/png/12735713/1615127239582-c0d355a3-f1e7-479c-8955-2c3c4dd83a28.png#align=left&display=inline&height=597&margin=%5Bobject%20Object%5D&name=%E6%89%93%E5%8C%85%E9%80%9F%E5%BA%A6%E6%A3%80%E6%B5%8B.png&originHeight=597&originWidth=412&size=18696&status=done&style=shadow&width=412)


配置过程很简单，就是一个webpack插件，安装依赖，引用并配置就可以：


#### 1、安装依赖


```shell
# npm
$ npm install --save-dev speed-measure-webpack-plugin

# or yarn
$ yarn add -D speed-measure-webpack-plugin
```


#### 2、配置插件
build.js


```javascript
// 引用插件
const SpeedMeasurePlugin = require('speed-measure-webpack-plugin');

const smp = new SpeedMeasurePlugin();

// 配置插件，包装整个配置对象
const webpackConfig = smp.wrap({
  plugins: []
});
```


OK，执行一下build命令就可以看到效果了！


### 打包可视化


`webpack-bundle-analyzer` [[https://www.npmjs.com/package/webpack-bundle-analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)]


打包可视化分析，这个插件可以直观地看到打包后的文件有哪些、文件大小、模块包含关系、依赖项等等，针对这些，我们可以进行文件拆分和合并，非常方便。


![优化前可视化.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/12735713/1615127287767-d2a29525-3f3d-40c2-9436-e113e57ec5f8.jpeg#align=left&display=inline&height=937&margin=%5Bobject%20Object%5D&name=%E4%BC%98%E5%8C%96%E5%89%8D%E5%8F%AF%E8%A7%86%E5%8C%96.jpg&originHeight=937&originWidth=1920&size=341613&status=done&style=shadow&width=1920)


配置方法如下：


#### 1、安装依赖


```shell
# npm
$ npm install --save-dev webpack-bundle-analyzer

# or yarn
$ yarn add -D webpack-bundle-analyzer
```


#### 2、引用并配置插件
build.js


```javascript
// 引用插件
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer')

// 配置插件
module.exports = {
  plugins: [
    new BundleAnalyzerPlugin({
      analyzerPort: 3030 // 端口号
    })
  ]
}
```


**运行**
执行build命令，浏览器会自动打开 `[http://127.0.0.1:3030/](http://127.0.0.1:3030/)`，可以看到如上图的效果啦，这里的端口号就是刚刚在插件中配置的端口。

---

OK，工具有了，接下来就配置webpack优化啦！


## 三、优化loader


先说下 `loader`，由于webpack只能打包js文件，对于css、图片、字体这些格式的文件，是无法识别的。那就需要引用第三方模块来执行，`loader` 就是这样的，可以将这些webpack无法识别的模块进行编译、压缩、打包，最终转换为js。


在我们的前端项目中，这些非js模块大多都分散在不同的文件目录中，如果在打包过程中，对全局所有的非js模块都检测并处理，比如`node_modules`，其实并没有必要；而且，有些文件嵌套层级太深，loader需要一级一级搜索，这样肯定会影响效率。


那我们首先需要精简项目中的文件目录层级，建议不要超过3级；


同时，优化loader搜索范围，忽略不需要编译的目录。


```javascript
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: [resolve('../node_modules')], // 屏蔽不需要处理的文件（文件夹）（可选）
        use: ['babel-loader']
      },
      {
        test: /\.css$/,
        // exclude: [resolve('../node_modules')], // css-loader 需要注意项目中是否有直接引用依赖中的css文件，如果有，则不能排除依赖目录
        use: ['style-loader', 'css-loader']
      },
      {
        test: /\.less$/,
        exclude: [resolve('../node_modules')], // 屏蔽不需要处理的文件（文件夹）（可选）
        use: ['style-loader', 'css-loader', 'less-loader']
      },
      {
        test: /\.(png|jpg|jpeg|gif)(\?.+)?$/,
        exclude: [resolve('../node_modules')], // 屏蔽不需要处理的文件（文件夹）（可选）
        use: ['url-loader']
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        exclude: [resolve('../node_modules')], // 屏蔽不需要处理的文件（文件夹）（可选）
        use: ['url-loader']
      },
    ]
  }
}
```


注意：
`css-loader`：需要查看项目中是否有直接引用 `node_modules` 中的css文件，如果有，则不能排除依赖目录，否则打包会报错。


## 四、压缩图片、字体


在上面的loader配置中，我们把图片、字体等文件用 `url-loader` 进行了编译处理，但是这里需要注意下，不是所有的图片、字体都需要用loader。


因为 `url-loader` 会把图片、字体等打包成base64，有些大图片的base64可能比原内存还大，所以需要限制一下可打包的资源大小，对于超过限制的资源，直接作为单独的文件以路径方式引入即可。


```javascript
module.exports = {
  ...
  module: {
    rules: [
      ...
      {
        test: /\.(png|jpg|jpeg|gif)(\?.+)?$/,
        exclude: [resolve('../node_modules')], // 屏蔽不需要处理的文件（文件夹）（可选）
        loader: 'url-loader',
        options: {
          esModule: false, // 这里设置为false，否则src为"[object Module]"
          limit: 10000, // url-loader 包含file-loader，这里不用file-loader, 小于10000B的图片base64的方式引入，大于10000B的图片以路径的方式导入
          name: isDev ? 'image/[name][hash:8].[ext]' : 'image/[name].[contenthash:8].[ext]'
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        exclude: [resolve('../node_modules')], // 屏蔽不需要处理的文件（文件夹）（可选）
        loader: 'url-loader',
        options: {
          limit: 10000, // 小于10000B的字体base64的方式引入，大于10000B的字体以路径的方式导入
          name: isDev ? 'font/[name][hash:8].[ext]' : 'font/[name].[contenthash:8].[ext]'
        }
      },
    ]
  }
}
```


这里注意：


- 图片资源的 `esModule` 需要设置为 `false`，否则引用的 `src` 为 `[object Module]`，无法显示
- 这几种类型文件建议分开打包到不同的目录，方便管理
- 打包后的文件使用 `contenthash` 命名，这样不经常变动的文件就可以缓存，提高二次打包速度



## 五、分离css


在默认的配置中，css是以内嵌形式打包在js文件中，这样肯定会导致js文件很大，因此我们需要分离代码中的css，作为单独目录和文件，这样css和js就可以并行打包，同时还有一点好处就是可以并行下载，更快地加载样式和脚本。


这里我们使用 `mini-css-extract-plugin` 插件（webpack3中有 `extract-text-webpack-plugin`，但webpack4不支持）：


### 1、安装依赖


```shell
# npm
$ npm install --save-dev mini-css-extract-plugin

# or yarn
$ yarn add -D mini-css-extract-plugin
```


### 2、配置插件


config.js


```javascript
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      filename: isDev ? 'css/[name][hash:8].css' : 'css/[name].[chunkhash:8].css',
      chunkFilename: isDev ? 'css/[id][hash:8].css' : 'css/[id].[chunkhash:8].css',
    })
  ],
  module: {
    rules: [
      ...
      {
        test: /\.css$/,
        ...
        // 本地开发环境下，style-loader和MiniCssExtractPlugin.loader有冲突
        use: [isDev ? 'style-loader' : MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        test: /\.less$/,
        ...
        // 本地开发环境下，style-loader和MiniCssExtractPlugin.loader有冲突
        use: [isDev ? 'style-loader' : MiniCssExtractPlugin.loader, 'css-loader', 'less-loader']
      },
    ],
  },
};
```


注意：


- 本地开发环境下，`style-loader` 和 `MiniCssExtractPlugin.loader` 有冲突，需要单独设置
- 建议单独放在css目录下
- 打包后的文件使用 `chunkhash` 缓存不经常变动的文件，提高二次打包速度



## 六、压缩css


webpack4比较好的一点就是可以配置 `optimization.minimize` [https://webpack.js.org/configuration/optimization/#optimizationminimizer]， 默认会对css、js等文件进行压缩，同时还可以支持插件集成。


开启css压缩，减小css文件大小，这里可以使用 `optimize-css-assets-webpack-plugin` 插件：


### 1、安装依赖


```shell
# npm
$ npm install --save-dev optimize-css-assets-webpack-plugin

# or yarn
$ yarn add -D optimize-css-assets-webpack-plugin
```


### 2、配置插件


build.js


```javascript
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin') // css压缩

module.exports = {
  ...
  optimization: {
    minimizer: [
      // 压缩css
      new OptimizeCSSAssetsPlugin({
        cssProcessorOptions: {
          // 移除注释
          discardComments: {
            removeAll: true
          }
        }
      }),
    ]
  }
}
```


注意：


- 只需要在打包的时候压缩，所以单独配置在 build.js 即可



## 七、分离js


webpack默认打包过程中，我们可以看到所有的js都会打包到一个文件中，这样会导致主文件特别大。像一些不常更新的第三方依赖，一般是不会变动的，如果每次都执行一次打包，肯定不是我们期望的。


webpack3时，我们的方案大多都是 `CommonsChunkPlugin` + 缓存策略 来实现；而webpack4给我们带来了更好用的工具： `splitChunks` [https://webpack.js.org/plugins/split-chunks-plugin/#root]。


`splitChunks`是一个默认集成的功能，不需要我们安装插件，直接配置即可。


比如我们可以把不常更新的react相关打包到一个baseChunks；antd、icons、echarts、emoji等打包到一个uiChunks；剩余的业务js等打包到默认chunk：


build.js


```javascript
module.exports = {
  optimization: {
    ...
    splitChunks: {
      chunks: 'all', // initial(初始化) | async(动态加载) | all(全部)
      minSize: 30000, // // 大于30k会被webpack进行拆包，默认0
      minChunks: 1, // 被引用次数大于等于这个次数进行拆分，默认1
      maxAsyncRequests: 5, // 最大异步请求数， 默认1
      maxInitialRequests: 5, // 最大初始化请求数，默认1
      name: true,
      automaticNameDelimiter: '.', // 打包分隔符
      cacheGroups: {
        // 基础库
        baseChunks: {
          name: 'base.chunks', // 要分隔出来的 chunk 名称
          test: (module) => (/react|react-dom|react-router-dom|react-redux|redux|axios|moment|lodash/.test(module.context)),
          priority: 20 // 打包优先级
        },
        // UI、icon、图表、表情等库
        uiChunks: {
          name: 'ui.chunks', // 要分隔出来的 chunk 名称
          test: (module) => (/antd|@ant-design\/icons|echarts|emoji-mart/.test(module.context)),
          priority: 10 // 打包优先级
        },
        // 打包其余的的公共代码
        default: {
          name: 'common.chunks', // 要分隔出来的 chunk 名称
          minChunks: 2, // 引入两次及以上被打包
          priority: 5,
          reuseExistingChunk: true // 可设置是否重用已用chunk 不再创建新的chunk
        }
      }
    },
  }
}
```


注意：


- 一定要配置js文件chunkhash命名，否则每次打包hash都会改变，还是会更新不期望变更的文件：



```javascript
module.exports = {
   // 出口配置
  output: {
    path: resolve('../dist'),
    filename: isDev ? 'js/[name].[hash:8].js' : 'js/[name].[chunkhash:8].js',
    publicPath: isDev ? '/' : './'
  },
}
```


打包后的结果如图：
![webpack4优化可视化.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/12735713/1615127355802-f09c7108-43cd-4095-b4a1-a69a48db496f.jpeg#align=left&display=inline&height=937&margin=%5Bobject%20Object%5D&name=webpack4%E4%BC%98%E5%8C%96%E5%8F%AF%E8%A7%86%E5%8C%96.jpg&originHeight=937&originWidth=1920&size=282215&status=done&style=shadow&width=1920)


## 八、压缩js


开启js压缩，减小js文件大小，这里可以使用 `uglifyjs-webpack-plugin` 插件：


### 1、安装依赖


```shell
# npm
$ npm install --save-dev uglifyjs-webpack-plugin

# or yarn
$ yarn add -D uglifyjs-webpack-plugin
```


### 2、配置插件


build.js


```javascript
const UglifyJsPlugin = require('uglifyjs-webpack-plugin') // js压缩

module.exports = {
  ...
  optimization: {
    // 自定义js优化配置，将会覆盖默认配置
    new UglifyJsPlugin({
      parallel: true, // 使用多进程并行运行来提高构建速度
      sourceMap: true, // 一定要打开才可以生成map文件
      cache: true,
      uglifyOptions: {
        output: {
          comments: false, // 去掉注释
          ascii_only: true
        },
        compress: {
          drop_console: false,
          drop_debugger: true,
          comparisons: false
        }
      }
    })
  }
}
```


注意：


- 只需要在打包的时候压缩，所以单独配置在 build.js 即可
- 自定义js优化配置，将会覆盖默认配置



## 九、多进程


由于webpack是基于node.js的单线程模式，那么当我们编译、打包文件时，就只能逐个地处理任务，构建速度肯定很慢。


`Happypack`就是为了解决这个问题，开启多进程，将任务拆分成多个子进程并发执行，子进程处理完任务后将结果发送给主进程，加快打包速度。


> 严格来说，应该是多进程：由于js是单线程模型，要想发挥多核CPU能力，只能通过多进程实现，而无法通过多线程实现。



示例：通过 `HappyPack` 配置2个多进程的loader，然后在rules中引用配置好的loader


### 1、安装依赖


```shell
# npm
$ npm install --save-dev happypack

# or yarn
$ yarn add -D happypack
```


### 2、配置插件


```javascript
const os = require('os')
const Happypack = require('happypack') // 多进程打包

// 开启进程池，使用系统CPU的最大核数
const happyThreadPool = Happypack.ThreadPool({
  size: os.cpus().length
})

module.exports = {
  plugins: [
    ...
    // 可以配置多个不同的进程
    // js进程
    new Happypack({
      id: 'js', // 进程名
      threadPool: happyThreadPool,
      // 和loader配置方式相同
      use: [
        'babel-loader'
      ]
    }),
    // less进程
    new Happypack({
      id: 'less', // 进程名
      threadPool: happyThreadPool,
      // 和loader配置方式相同
      use: [
        {
          loader: 'css-loader',
          options: {
            importLoaders: 1,
            modules: true // less modules支持，一定要开启
          }
        },
        {
          loader: 'less-loader'
        }
      ]
    })
  ]
}
```


### 3、应用进程


配置好happypack后，可以在对应的loader配置中使用：


```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: [resolve('../node_modules')], // 屏蔽不需要处理的文件（文件夹）（可选）
        use: 'happypack/loader?id=js'
      },
      ...
      {
        test: /\.less$/,
        exclude: [resolve('../node_modules')],
        // happypack中MiniCssExtractPlugin.loader会有报错，因此单独设置
        use: [isDev ? 'style-loader' : MiniCssExtractPlugin.loader, 'happypack/loader?id=less']
      },
    ]
  }
}
```


注意：


- `loader`的 `id` 一定要与配置 `happypack` 时的一致，这样才可以加载对应的进程



运行效果如图，可以看到正在运行的进程数：
![进程数.png](https://cdn.nlark.com/yuque/0/2021/png/12735713/1615127402147-967c7f93-7fdd-432e-af3b-538e493039cc.png#align=left&display=inline&height=146&margin=%5Bobject%20Object%5D&name=%E8%BF%9B%E7%A8%8B%E6%95%B0.png&originHeight=146&originWidth=637&size=8635&status=done&style=shadow&width=637)

---

OK，到这里，优化的配置基本就完成了！


可以看下最终的效果，相比开始的5-6分钟，优化后的打包不到1分钟就完成了！打包后的文件目录也很清晰！


> Tips：建议每配置完一项，都执行一次build命令，方便及时发现问题、并定位解决！



![webpack4优化.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/12735713/1615127436238-25ce7a9f-5fe3-45cd-aedd-18982d0e6403.jpeg#align=left&display=inline&height=819&margin=%5Bobject%20Object%5D&name=webpack4%E4%BC%98%E5%8C%96.jpg&originHeight=819&originWidth=972&size=61866&status=done&style=shadow&width=972)
![打包后的文件1.png](https://cdn.nlark.com/yuque/0/2021/png/12735713/1615127469973-47d57c90-0b7d-427a-a468-2dd486f259a9.png#align=left&display=inline&height=188&margin=%5Bobject%20Object%5D&name=%E6%89%93%E5%8C%85%E5%90%8E%E7%9A%84%E6%96%87%E4%BB%B61.png&originHeight=188&originWidth=632&size=11554&status=done&style=shadow&width=632)
![打包后的文件2.png](https://cdn.nlark.com/yuque/0/2021/png/12735713/1615127482836-d5fedb56-c1da-451c-86b8-670d87045511.png#align=left&display=inline&height=163&margin=%5Bobject%20Object%5D&name=%E6%89%93%E5%8C%85%E5%90%8E%E7%9A%84%E6%96%87%E4%BB%B62.png&originHeight=163&originWidth=645&size=12403&status=done&style=shadow&width=645)

---

最后，记录一下配置过程中可能遇到的问题：


## 十、可能遇到的问题


**1、"Failed to load resource: net::ERR_FILE_NOT_FOUND"错误**


原因：打包时的文件输出路径有问题


解决方案：修改build.js出口配置


```javascript
output: {
    publicPath: './'
}
```


**2、打包后，图片地址变成了 [object Module]**


原因：这个问题是file-loader的 `esModule` 在新版本中默认是`true`


解决方案：手动改为`false`


```javascript
{
  test: /\.(png|jpe?g)(\?.*)?$/,
  loader: 'url-loader',
  options: {
    esModule: false,
    limit: 10000
  }
}
```


**3、打包后，index.html引入文件没有引号**


原因：这个问题是压缩时删除了引号


解决方案：在build.js找到minify，设置`removeAttributeQuotes: false`，或者把整个minify删除


**4、警告：[mini-css-extract-plugin] Conflicting order between 和 Entrypoint mini-css-extract-plugin=**


原因：在不同文件中引用相同的两个css文件且顺序不同，就会报出警告


解决方案：


- 修改出现问题的js中css文件顺序
- 如果无法修改，可以考虑关闭警告。配置 `stats` 去掉警告，stats文档：[https://webpack.js.org/configuration/stats/#stats](https://webpack.js.org/configuration/stats/#stats)



```javascript
module.exports = {
  ...
  // 构建时的log设置
  stats: {
    children: false
  },
}
```


**5、警告：WARNING in asset size limit: The following asset(s) exceed the recommended size limit (244 KiB)**


原因：打包的文件体积超过了默认限制


解决方案：如果可以，则修改源文件；如果无法修改，可以考虑关闭警告或者修改限制。
build.js添加配置：


```javascript
module.exports = {
  // 配置如何展示性能提示
  performance: {
    // hints: false, // 直接关闭或者配置以下警告限制
    hints: 'warning',
    maxEntrypointSize: 50000000, // 入口起点的最大体积，默认250kb
    maxAssetSize: 30000000 // 根据单个资源体积，控制webpack何时生成性能提示，默认250kb
  }
}
```


