---
title: 前端性能优化Webpack\Babel\编码篇
date: 2021-02-20
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

### 1.2 IgnorePlugin
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

### 1.3 source map
`source map`资源地图。定位浏览器控制台输出语句在项目文件的位置。
```js
module.exports = {
  productionSourceMap: false
}
```


### 1.4 webpack-bundle-analyzer
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

### 1.5 SplitChunksPlugin
你可以把应用中的特定部分移至不同文件。如果一个模块在不止一个chunk中被使用，那么利用代码分离，该模块就可以在它们之间很好地被共享。这是Webpack的默认行为。

```js
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'async', // 有三个可选值：initial(初始块)、async(按需加载块)、all(全部块)，默认为async
      minSize: 30000, // 表示在压缩前的最小模块大小，默认为30000
      minChunks: 1, // 表示被引用次数，默认为1
      maxAsyncRequests: 5, // 按需加载时候最大的并行请求数，默认为5
      maxInitialRequests: 3, // 一个入口最大的并行请求数，默认为3
      automaticNameDelimiter: '~', // 命名连接符
      name: true, // 拆分出来块的名字，默认由块名和hash值自动生成
      cacheGroups: { // 缓存组。缓存组的属性除上面所有属性外，还有test, priority, reuseExistingChunk
        test: '', // 用于控制哪些模块被这个缓存组匹配到
        priority: '', // 缓存组打包的先后优先级
        reuseExistingChunk: '' // 如果当前代码块包含的模块已经有了，就不在产生一个新的代码块
      }
    }
  }
}
```

**下面通过示例看看chunks三种模式在对正常与动态加载的代码效果上有什么区别**
准备了两个入口文件，都引入了三个 npm 库，jquery,react,lodash。
* jquery 均同步引入
* 其中 react 在 a 文件同步引入而 b 中动态加载，
* lodash 在两者中均动态引入

```js
// a.js
import "react";
import("lodash");
import "jquery";
console.log("a");

// b.js
import("react");
import("lodash");
import "jquery";
console.log("b");

// webpack.config.js
module.exports = {
  entry: {
    a: "./a.js",
    b: "./b.js"
  },
  output: {
    filename: "[name].bundle.js"
  },
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          chunks: "async",
          priority: 1
        }
      }
    }
  },
  plugins: [new BundleAnalyzerPlugin()]
};
```

1. chunks模式为`async`
* 在 a,b 中均同步引入的 jquery 被打包进了各自的 bundle 中没有拆分出来共用，因为这种模式下只会优化动态加载的代码。
* react 打了两份:
  * 一份在 a 自己的 bundle 中，因为它同步引入了 react，而我们只优化动态加载的代码，所以这里的 react 不会被优化拆分出去。
  * 一份在单独的文件中，它是从 b 里面拆出来的，因为 b 中动态加载了 react。
* lodash 因为在 a,b 中都是动态加载，形成了单独的 chunk 被 a, b 共用。

2. chunks模式为`initial`
initial 即原始的拆分，原则就是有共用的情况即发生拆分。动态引入的代码不受影响，它是无论如何都会走拆分逻辑的（毕竟你要动态引入嘛，不拆分成单独的文件怎么动态引入？！）。而对于同步引入的代码，如果有多处都在使用，则拆分出来共用。
* jquery 在这种模式下发生了变化。形成了单独的 chunk 供 a,b 共用。
* react 没有变，因为它在 a,b 中引用的方式不同，所以不会被当成同一个模块拆分出来共用，而是走各自的打包逻辑。在 a 中同步引用，被打入了 a 的 bundle。在 b 中动态引入所以拆分成了单独的文件供 b 使用。
* lodash 没变，形成单独一份两者共用。

3. chunks模式为`all`
* 从上面 initial 模式下我们似乎看出了问题，即 在 a 中同步引入在 b 中动态引入的 react，它其实可以被抽成文件供两者共用的，只是因为引入方式不同而没有这样做。
* 所以 all 这种模式下，就会智能地进行判断以解决这个问题。此时不关心引入的模块是动态方式还是同步方式，只要能正确判断这段代码确实可以安全地进行拆分共用，那就干吧。
* 需要注意的是这里需要设置 minSize 以使 react 能够正确被拆分，因为它小于 30k，在同步方式下，默认不会被拆分出来。

**结论**
* 看起来似乎 all 是最好的模式，因为它最大限度地生成了复用的代码，Webpack 默认就走这个模式打包不就得了。
* 在开头的时候提到过一个原因为何默认情况下只优化 async 代码。所以，除了 all 之外的另外两个选项是有存在意义的。并且，具体的优化场景需要根据具体的需求而定，all 所产生的效果并非所有情况下都需要。


### 1.6 [TreeShaking sideEffects ](https://webpack.docschina.org/guides/tree-shaking/)
移除 JavaScript 上下文中的未引用代码(也就是移除文件中的未使用的代码)。

1. sideEffects
```json
// package.json
// 如果所有代码都不包含 side effect，我们就可以简单地将该属性标记为 false，来告知 webpack，它可以安全地删除未用到的 export。
{
  "name": "",
  "sideEffects": false
}

// 如果你的代码确实有一些副作用，可以改为提供一个数组：
"sideEffects": [
  "**/*.css",
  "**/*.scss",
  "./src/index.js",
  "./src/a.js"
],
```

### 1.7 [MiniCssExtractPlugin、OptimizeCssAssetsPlugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin)
用于提取、优化压缩CSS资源
```js
yarn add -D mini-css-extract-plugin optimize-css-assets-webpack-plugin cssnano

const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader"
        ]
      }
    ]
  },
  plugins: [
    new OptimizeCSSAssetsPlugin({
        assetNameRegExp: /\.css\.*(?!.*map)/g,  //注意不要写成 /\.css$/g
        cssProcessor: require('cssnano'),
        cssProcessorOptions: {
            discardComments: { removeAll: true },
            // 避免 cssnano 重新计算 z-index
            safe: true,
            // cssnano 集成了autoprefixer的功能
            // 会使用到autoprefixer进行无关前缀的清理
            // 关闭autoprefixer功能
            // 使用postcss的autoprefixer功能
            autoprefixer: false
        },
        canPrint: true
    }),
  ]
};
```

### 1.8 uglifyjs-webpack-plugin
此插件使用 uglify-js 压缩你的 JavaScript。
```js
yarn add uglifyjs-webpack-plugin -D

const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
module.exports = {
  optimization: {
    minimizer: [new UglifyJsPlugin()],
  },
};
```

### 1.9 contenthash
根据文件内容计算而来,打包后修改一个文件只改变文件的hash不会改变关联的文件hash。
```js
  module.exports = {
    entry: './src/index.js',
    plugins: [
    output: {
      filename: '[name].[contenthash].js',
      path: path.resolve(__dirname, 'dist'),
    },
  };
```

### 2.0 Shimming预置全局变量
```js
const webpack = require('webpack');

 module.exports = {
  plugins: [
    new webpack.ProvidePlugin({
      _: 'lodash',
    }),
  ],
 };

function component() {
  const element = document.createElement('div');
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
   return element;
}
document.body.appendChild(component());
```

### 2.1 noParse
过滤不需要解析的文件
```js
module.exports = {
  module: {
    noParse: '/jquery|lodash/', // 不去解析三方库
    rules: []
  }
}
```

### 2.2 cache-loader 提高打包效率
在一些性能开销较大的 loader 之前添加此 loader，以将结果缓存到磁盘里。
```js
yarn add cache-loader -D

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: [
          'cache-loader',
          'babel-loader'
        ],
        include: path.resolve('src')
      }
    ]
  }
}
```

### 2.3 speed-measure-webpack-plugin
查看loader、plugin打包花费时间
```js
yarn add -D speed-measure-webpack-plugin

const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();
const webpackConfig = smp.wrap({
  plugins: [new MyPlugin(), new MyOtherPlugin()],
});
```


### 2.4 [HardSourceWebpackPlugin](https://github.com/mzgoddard/hard-source-webpack-plugin)
用于为模块提供中间缓存步骤.第一次构建将花费正常时间。第二个版本将明显更快。
```js
yarn add --dev hard-source-webpack-plugin

const HardSourceWebpackPlugin = require('hard-source-webpack-plugin');
module.exports = {
  plugins: [
    new HardSourceWebpackPlugin()
  ]
}
```

### 2.5 image-webpack-loader
图片压缩优化操作
```js
yarn add --dev image-webpack-loader

module.exports = {
  module: {
    rules: [{
      test: /\.(gif|png|jpe?g|svg)$/i,
      use: [
        'file-loader',
        {
          loader: 'image-webpack-loader',
          options: {},
        },
      ],
    }]
  }
}
```


## 三、Babel方面的优化配置

## 四、代码方面的优化




<br>
<br>
<br>
