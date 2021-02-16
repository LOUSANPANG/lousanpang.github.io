---
title: 前端性能优化Webpack\Babel篇
date: 2021-01-01
tags: 
    - 前端性能优化
categories: 前端性能优化
keywords: [前端性能优化]
description: 前端性能优化篇
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s1.ax1x.com/2020/10/28/B1ZIv4.gif # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## 一、Webpack方面的优化配置
### 1.1 GZIP
1. 借助`compression webpack plugin`插件
```bash
yarn add -D compression-webpack-plugin
```

2. [webpack config 配置](https://www.webpackjs.com/plugins/compression-webpack-plugin/)
```js
const CompressionPlugin = require("compression-webpack-plugin")

module.exports = {
  plugins: [
    new CompressionPlugin({
        threshold: 10240 // 10K以上的进行压缩
    })
  ]
}
```

### 1.2 忽略赘余的代码包`IgnorePlugin`
使用`webpack`内置的`IgnorePlugin`插件来忽略项目中用不到的文件。

1. 以`moment`为例，只用到了中文语言包，打包的时候把非中文语言包排除掉。
```js
const webpack = require('webpack')
module.exports = {
  plugins: [
    new webpack.IgnorePlugin(/\.\/locale/, /moment/)
  ]
}
```

2. 忽略了整个`locale`文件下的语言包，需要自己手动引入中文语言包。
```js
import moment from 'moment'
import 'moment/locale/zh-cn'

moment.locale('zh-cn')
```

### 1.3 关闭生产环境的 `source map`
`source map`资源地图。定位浏览器控制台输出语句在项目文件的位置。
```js
module.exports = {
  productionSourceMap: false
}
```


### 1.4 打包树形图 webpack-bundle-analyzer
将捆绑包内容表示为方便的交互式可缩放树形图
```js
yarn add -D webpack-bundle-analyzer

// webpack.config.js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin()
  ]
}

// package.json
{
  "scripts": {
    "analyz": "NODE_ENV=production npm_config_report=true npm run build" // 也可以打包时打印
  }
}
```

### 1.6 image-webpack-loader
### 1.7 CommonsChunkPlugin
### 1.8 Tree-Shaking
### 1.9 SplitChunksPlugin
### 2.0 mini-css-extract-plugin
### 2.1 optimize-css-assets-webpack-plugin
### 2.2 uglifyjs-webpack-plugin
### 2.3 contenthash
### 2.4 shimming ProvidePlugin
### 2.5 代码分割code split
### 2.6 tree shaking
### 2.7 懒加载
### 2.8 webpack-spritesmith
### 2.9 noParse
### 3.0 exclude
### 3.1 cache-loader 提高打包效率
### 3.2 speed-measure-webpack-plugin 打包时候每一个loader或者plugin花费了多少时间 1
### 3.3 提取第三方库
### 3.4 babel-loader 的 cacheDirectory
### 3.5 HardSourceWebpackPlugin
### 3.6 image-webpack-loader
### 3.7 useless-files-webpack-plugin



## 二、Babel方面的优化配置




<br>
<br>
<br>
