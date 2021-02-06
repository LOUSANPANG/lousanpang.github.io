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

### 1.3 关闭`productionSourceMap`


## 二、Babel方面的优化配置




<br>
<br>
<br>
