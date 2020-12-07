---
title: 最后一次回顾下Webpack4.x
date: 2020-12-07
tags: 
    - webpack
categories: CLI
keywords: [webpack]
description: 由浅入深配置webpack4
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2020/12/07/DxpRxO.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

[![「模块打包工具」，将多个模块打包到生成一个最终的bundle.js问题。](https://s3.ax1x.com/2020/12/07/DxpRxO.png)](https://imgchr.com/i/DxpRxO)


## 概述
webpack需要掌握的核心概念👇
- Entry:webpack开始构建的入口模块

- Output: 如何命名输出文件，以及输出目录，比如常见的dist目录。

- Loaders:作用在于解析文件，将无法处理的非js文件，处理成webpack能够处理的模块。

- Plugins:更多的是优化，提取精华(公共模块去重)，压缩处理(css/js/html)等，对webpack功能的扩展。

- Chunk: 个人觉得这个是webpack 4 的Code Splitting 产物，抛弃了webpack3的CommonsChunkPlugin,它最大的特点就是配置简单，当你设置 mode 是 production，那么 webpack 4 就会自动开启 Code Splitting，可以完成将某些公共模块去重，打包成一个单独的chunk。


## 一、基础知识
### 1.1 如何安装Webpack
```bash
node -v
npm -v
```

### 1.2 初始化项目
```bash
npm init
```
接下来就发现，在该根目录下，会「生成一个package.json文件」，这个文件描述了node项目，node包的一些信息。也就是说，「npm init 生成的就是一个package.json文件。」
```
name - 包名.
version - 包的版本号。
description - 包的描述。
homepage - 包的官网URL。
author - 包的作者，它的值是你在https://npmjs.org网站的有效账户名，遵循“账户名<邮件>”的规则，例如：zhangsan <zhangsan@163.com>。
contributors - 包的其他贡献者。
dependencies / devDependencies - 生产/开发环境依赖包列表。它们将会被安装在 node_module 目录下。
main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
keywords - 关键字
```

### 1.3 安装webpack
不建议全局安装，建议局部安装。
```bash
npm install webpack webpack-cli -D
```
查看版本，不要`webpack -v`, 这样node会去全局去找，找不到。
```bash
npx webpack -v
```
查看webpack包的版本
```bash
npm info webpack
```

### 1.4 webpack配置文件
webpack.config.js就是webpack的默认配置文件，我们可以自定义配置文件，比如文件的入口，出口
```js
const path = require('path')
module.exports = {
    entry : './index.js',
    output : {
        filename : 'bundle.js',
        path : path.join(__dirname, 'dist')
    }
}
```

### 1.5 基础运行
命令行中运行npx webpack，就会去找webpack.config.js文件中的配置信息。
```bash
npx webpack
```
「默认的配置文件必须是webpack.config.js这个名称」，但是你自己写了一个webpack配置文件信息，得运行以下命令👇
```bash
// --config 后面就是你自己配置的webpack文件信息
npx webpack --config xxxx
```

### 1.6 npm script
「npm scripts」 有时候，你用过vue，React的话， 经常使用的都是npm run dev的形式，那么我们是不是也能配置这样子的信息呢？我们只需要在package.json文件中配置scripts命令就行👇
```json
"scripts": {
  "dev": "webpack --config webpack.config.js"
}
```
「webpack打包三种命令」
- webpack index.js (全局)
- npx webpack index.js
- npm run dev

### 1.7 webpack-cli
这个时候，你也许就会发现这个webpack-cli作用了吧，不下载这个包的话，你在命令行运行webpack指令是不生效的，也就是说，「webpack-cli作用就是可以在命令行运行webpack命令并且生效。」

不下载的话，在命令行中使用webpack命令是不允许的。

### 1.8 webpack配置环境
主要分为「development」和「production」两种环境，默认情况下是production环境，**两者的区别就是，后者会对打包后的文件压缩。**
```js
const path = require('path')
module.exports = {
    mode : 'development',
    entry : './index.js',
    output : {
        filename : 'bundle.js',
        path : path.join(__dirname, 'bundle')
    }
}
```
这个时候，再去看的话，就会发现，「bundle.js文件没有压缩代码」。


## 二、webpack核心概念loader
### 2.1 什么是loader
「loader就是一个打包的方案，它知道对于某个特定的文件该如何去打包。」 本身webpack不清楚对于一些文件如何处理，loader知道怎么处理，所以webpack就会去求助于loader。

webpack是默认知道如何打包js文件的，但是对于一些，比如图片，字体图标的模块，webpack就不知道如何打包了，那么我们如何让webpack识别图片等其他格式的模块呢？

那么就去配置文件webpack.config.js配置相应的信息，配置module👇
```js
const path = require('path')
module.exports = {
    mode: 'production',
    entry: './src/index.js',
    module: {
        rules: [{
            test: /\.(png|jpg|gif)$/,
            use: {
                loader: 'file-loader'
            }
        }]
    },
    output: {
        filename: 'bundle.js',
        path: path.join(__dirname, 'dist')
    }
}
```
我们需要file-loader的话，也就是依赖于它，所以先下载它
```
npm install file-loader -D
```
然后我们看看index.js是如何写的👇
```js
import acator from './头像.jpg'
console.log(acator)
```
通过这个我们发现，在控制台，打印的结果是 
```
3f16daf5233d30f46509b1bf2c4e08a5.jpg
```
说明file-loader帮我们图片模块打包到了dist目录下，并且index.js中，这个acator变量，结果是一个名称，这样子的话，就可以完成打包，后续需要该图片也轻松搞定。

「总结」

webpack无法识别非js结尾的模块，所以需要loader让webpack识别出来，这样子就可以完成打包。

- 遇到非js结尾的模块，webpack会去module中找相应的规则，匹配到了对于的规则，然后去求助于对应的loader
- 对应的loader就会将该模块打包到相应的目录下，上面的例子就是dist目录，并且呢，「返回的是该模块的路径」,拿上面的例子来说，就是acator 变量的值就是路径

### 2.2 配置file-loader
举个例子，比如，你想将文件打包名称不改变，并且加个后缀的话，可以这么来配置options👇
```js
{
  loader: 'file-loader',
  options: {
      // name就是原始名称,hash使用的是MD5算法,ext就是后缀
      name: '[name]_[hash].[ext]'
  }
}
```
我们引入照片的是下面👇
```js
import acator from './头像.jpg'
```
那么最后打包完的名称是说明呢👇
```js
头像_3f16daf5233d30f46509b1bf2c4e08a5.jpg
```
在举个例子，比如你想将图片这些模块都打包到dist目录下的images下，是不是也是可以配置下
```js
{
  loader: 'file-loader',
  options: {
    name: '[name]_[hash].[ext]',
    outputPath: 'images/'
  }
}
```
比如不同的环境下，打包的图片位置也可以不一样，👇
```js
if (env === 'development') {
  return '[path][name].[ext]'
}
```
比如字体图标怎么配置信息呢？对于字体图标大打包，可以使用file-loader完成👇
```js
{
    test: /\.(woff|woff2|eot|ttf|otf)$/,
    use: [
        'file-loader'
    ]
}
```

### 2.3 配置url-loader
上面对于图片的模块打包，我们同样可以去使用url-loader，那么它与file-loader区别是什么呢？
```js
{
  loader: 'url-loader',
  options: {
    name: '[name]_[hash].[ext]',
    outputPath: 'images/',
    limit : 102400  //100KB
  }
}
```
唯一的区别就在于，要打包的图片是否会打包到images目录下，还是以Base64格式打包到bundle.js文件中，这个就看limit配置项了。
- 当你打包的图片大小比limit配置的参数大，那么跟file-loader一样。
- 当图片较小时，那么就会以Base64打包到bundle.js文件中。

### 2.4 配置css-loader
比如你引入了一个css模块，这个时候，就需要去下载相应的模块loader。
```js
cnpm install css-loader style-loader -D   // 下载对应的模块
```
然后就是配置module👇
```js
{
  test: /\.css$/,
  use: ['style-loader','css-loader']
}
```
这样子的话，你在index.js 导入样式就可以生效啦，我们看看是如何导入的👇
```js
import acator from './头像.jpg'
import './index.css'
const img = new Image()
img.src = acator
img.classList.add('imgtitle')
document.body.appendChild(img)
```
这个imgtitle就是样式，如下👇
```css
.imgtitle{
  width: 100px;
  height: 100px;
}
```
通过两个loader，就实现了webpack打包css文件，那我们分析以下两个loader功能。
- css-loader主要作用就是将多个css文件整合到一起，形成一个css文件。
- style-loader会把整合的css部分挂载到head标签中。

### 2.5 配置sass-loader
安装sass-loader，需要同时安装node-sass，然后就去配置对应的module
```
npm install sass-loader node-sass --save-dev
```
```js
{
  test: /\.scss$/,
  use: ['style-loader','css-loader','sass-loader']
}
```
这样子的话，你像下面去导入scss样式文件，是可以打包完成的👇
```js
// index.js 
import acator from './头像.jpg'
// console.log(acator)
import './index.scss'   // 导入scss文件

const img = new Image()
img.src = acator
img.classList.add('imgtitle')
document.body.appendChild(img)
```
模块的加载就是从右像左来的，所以先加载sass-loader翻译成css文件，然后使用css-loader打包成一个css文件，在通过style-loader挂载到页面上去。

### 2.6 配置postcss-loader
这个loader解决的就是加上厂商前缀
```bash
npm i -D postcss-loader autoprefixer
```
然后呢，还需要建一个「postcss.config.js」，这个配置文件(「位置跟webpack.config.js一个位置」)配置如下信息👇
```js
// postcss.config.js
// 需要配置这个插件信息
module.exports = {
    plugins: [
        require('autoprefixer')({
            overrideBrowserslist: [
                "Android 4.1",
                "iOS 7.1",
                "Chrome > 31",
                "ff > 31",
                "ie >= 8"
            ]
        })
    ]
};
```
一开始我设置的话，是不生效的，原因就是「没有设置支持的浏览器」，然后看看下面👇
```js
{
  test: /\.scss$/,
  use: ['style-loader','css-loader','sass-loader','postcss-loader']
}
```
最后就可以看见比如css3会加上厂商前缀了👇
```css
-webkit-transform: translate(100px, 100px);
-ms-transform: translate(100px, 100px);
transform: translate(100px, 100px);
```
一些其他问题，有时候，你会遇到这样子的一个问题，在某个scss文件中又导入新的scss文件，这个时候，打包的话，它就不会帮你重新安装postcss-loader开始打包，这个时候，我们应该如何去设置呢，我们先来看例子👇
```js
// index.scss
@import './creare.scss';
body {
    .imgtitle {
        width: 100px;
        height: 100px;
        transform: translate(100px, 100px);
    }
}
```
- 我们知道，我们配置的loader规则中，是符合这样子的预期
- 当js代码中引入scss模块的话，会按照这样子的规则去做
- 那么如何在scss文件中引入scss文件，那么规则肯定不会从postcss-loader开始打包，所以我们需要配置一些信息。
```js
{
  test: /\.scss$/,
  use: ['style-loader',
      {
          loader: 'css-loader',
          options:{
              importLoaders:2,
              modules : true
          }
      },
      'sass-loader',
      'postcss-loader'
  ]
}
```
我们需要在css-loader中配置options，加入「importLoaders :2」， 这样子就会走postcss-loader，和sass-loader，这样子的语法，「无论你是在js中引入scss文件，还是在scss中引入scss文件，都会重新依次从下往上执行所以loader。」

那么modules:true这个配置是什么作用呢？有时候，你希望你的css样式作用的是当前的模块中，而不是全局的话，就需要加上这个配置了，看下案例👇
```js
// index.js
import acator from './头像.jpg'
import create from './create'

import style from './index.scss'  // 通过modules:true 避免了css作用域create中的模块
const img = new Image()
img.src = acator
img.classList.add(style.imgtitle)
document.body.appendChild(img)
create()
```
那么create模块是什么呢👇
```js
import acator from './头像.jpg'
import style from './index.scss'
function create() {
    const img = new Image()
    img.src = acator
    img.classList.add(style.imgtitle)
    document.body.appendChild(img)
}

export default create;
```
可以看出来，这个create模块，就是创建一个img标签，并且设置单独的样式。给modules : true后，我们需要接着往下做的就是import语法上有些改变。
```js
import style from './index.scss'
```
然后通过style这个对象变量中去找，找到scss中设置的名称即可。

「总结」

- importLoaders:2该配置信息解决的就是在scss文件中又引入scss文件，会重新从postcss-loader开始打包
- modules:true会作用域当前的css环境中，样式不会全局引入，语法上也需要使用如下引入
- import style from './index.scss'


## 核心概念plugins
plugins: 「可以在webpack运行到某个时刻的时候,帮你做一些事情.」

### 3.1 使用HtmlWebpackPlugin
「这个插件的作用，就是为你生成一个HTML文件，然后将打包好的js文件自动引入到这个html文件中。」
```bash
cnpm install --save-dev html-webpack-plugin
```
然后在webpack.config.js中配置如下信息👇
```js
var HtmlWebpackPlugin = require('html-webpack-plugin');
var path = require('path');
var webpackConfig = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js'
  },
  plugins: [new HtmlWebpackPlugin({
            template: 'src/index.html'  // 以src/目录下的index.html为模板打包
        }
    )],
};
```
然后运行npm run dev，就会发现在dist目录下，自动帮你生成一个HTML模块，并且引入bundle.js文件。

template: 'src/index.html' 这个配置信息的作用就是告诉你，以具体哪个index.html为模板去打包

### 3.2 使用cleanWebpackPlugin
这个插件的作用就是帮你删除某个目录的文件，是在打包前删除所有上一次打包好的文件。
```bash
cnpm i clean-webpack-plugin -D
//"clean-webpack-plugin": "^3.0.0",我的是这个版本
```
配置信息如下👇,这个是最新的clean-webpack-plugin配置
```js
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
// plugins新增加这一项，webpack4版本不需要配置路径
plugins: [ new CleanWebpackPlugin()]
```
最新的webpack4版本是不需要去配置路径的，自动帮我们清除打包好的dist目录下文件.


## 四、核心概念
### 4.1 entry和output基本配置
有时候,你需要多个入口文件,就需要来看一看entry和output配置项。
```js
entry: {
    index :'./src/index.js',
    bundle : './src/create.js',
},
output: {
    filename: '[name].js',
    publicPath: "https://cdn.example.com/assets/",
    path: path.join(__dirname, 'dist')
}
```
「总结」
- entry这样子配置就可以接受多个打包的文件入口,同时的话,output输出文件的filename需要使用占位符name
- 这样子就会生成两个文件,不会报错,对于的名字就是entry名称对应
- 如果后台已经将资源挂载到了cdn上,那么你的publicPath就会把路径前做修改加上publicPath值

## 4.2、使用devtool配置source-map
devtool配置source-map,解决的问题就是,当你代码出现问题时,会映射到你的原文件目录下的错误,并非是打包好的错误,这点也很显然,如果不设置的话,只会显示打包后bundle.js文件中报错,对应查找错误而言,是很不利的。
```
devtool:'inline-cheap-source-map'
```
对应不同的环境,设置devtool是很有必要的,开发环境中,我们需要看我们代码是哪里报错误,所以需要配置。
- development环境下,配置 devtool:'cheap-module-eval-source-map'
- production环境下,配置 devtool:'cheap-module-source-map'
```js
// development devtool:'cheap-module-eval-source-map'
// production  devtool:'cheap-module-source-map'
```

### 4.3 如何使用webpack-dev-server
```bash
cnpm i clean-webpack-plugin -D
```
它的作用很多,可以开启一个服务器,而且可以实时去监听打包文件是否改变,改变的话,就会出现去更新.
```js
devServer: {
    contentBase: path.join(__dirname, "dist"),   // dist目录开启服务器
    compress: true,    // 是否使用gzip压缩
    port: 9000,    // 端口号
    open : true   // 自动打开网页
}
```
很多的配置项,可以去官方文档查看,比如proxy代理等配置项

然后在package.json中scripts配置项如下
```
"start": "webpack-dev-server"
```
「这个devServer可以实时检测文件是否发生变化」

同时你需要注意的内容就是使用webpack-dev-server打包的话,不会生成dist目录,而是将「你的文件打包到内存中」

「总结」
- devServer可以开启一个本地服务器,同时帮我们更新加载最新资源
- 打包的文件会放在内存中,不会生成dist目录

### 4.4 模块热替换(hot module replacement)
模块热替换(HMR - Hot Module Replacement)功能会在应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面。

顾名思义它说的就是,多个模块之前,当你修改一个模块,而不想重新加载整个页面时,就可以使用hot module replacement
```js
devServer: {
    contentBase: path.join(__dirname, "dist"),
    compress: true,
    port: 9000,
    open: true,
    hot: true,   // 开启热更新
    // hotOnly: true,
},
```
这个hotOnly可以设置,最主要的是设置hot:true

然后加入两个插件,这个插件时webpack自带的,所以不需要下载👇
```js
const webpack = require('webpack')
plugins: [
    new webpack.NamedModulesPlugin(),  // 可配置也可不配置
    new webpack.HotModuleReplacementPlugin() // 这个是必须配置的插件
],
```
添加了 NamedModulesPlugin，以便更容易查看要修补(patch)的依赖。

配置完上述的信息之后,重新去运行命令的话,就会发现启动了模块热替换,不同模块的文件更新,只会下载当前模块文件

唯一需要注意的内容是,对于css的内容修改,css-loader底层会帮我们做好实时热更新,对于JS模块的话,我们需要手动的去配置👇
```js
if(module.hot){
    module.hot.accept('./print',()=>{
        print()
    })
}
```
这个官方也给出了语法,module.hot.accept(module1,callback) 表示的就是接受一个需要实时热更新的模块,当内容发生变化时,会帮你检测到,然后执行回调函数。

「总结」
- HMR模块热替换解决的问题就是,它允许在运行时更新各种模块，而无需进行完全刷新。
- 意思就是不需要重新去本地服务器重新去加载其他为修改的资源
- 需要注意的就是,对于js文件的热更新的话,需要手动的去检测更新内容,也就是module.hot.accept语法

### 4.5 Babel处理ES6语法
1. 安装
```bash
npm install --save-dev babel-loader @babel/core
// @babel/core 是babel中的一个核心库

npm install --save-dev @babel/preset-env
// preset-env 这个模块就是将语法翻译成es5语法,这个模块包括了所有翻译成es5语法规则

npm install --save @babel/polyfill
// 将Promise,map等低版本中没有实现的语法,用polyfill来实现.
```
2. 配置module👇
```js
module: {
  rules: [
    {
            test: /\.js$/,
            exclude: /node_modules/,
            loader: "babel-loader",
            options: {
                "presets": [
                    [
                        "@babel/preset-env",
                        {
                            "useBuiltIns": "usage"
                        }
                    ]
                ]
            }
        }
  ]
}
// exclude参数: node_modules目录下的js文件不需要做转es5语法,也就是排除一些目录
// "useBuiltIns"参数
```
- 有了preset-env这个模块后,我们就会发现我们写的「const语法被翻译成成var」
- 但是细心的会发现,对于Promise以及map这些语法,低版本浏览器是不支持的,
- 所以我们需要@babel/polyfill模块,对Promise,map进行补充,完成该功能,也就是前面说的polyfill
3. 使用
```bash
import "@babel/polyfill";
```
会发现问题,用完这个以后,打包的文件体积瞬间增加了10多倍之多。

这是因为,@babel/polyfill为了弥补Promise,map等语法的功能,该模块就需要「自己去实现Promise,map等语法」的功能,这也就是为什么打包后的文件很大的原因.

那我们需要对@babel/polyfill参数做一些配置即可,如下👇
```
"useBuiltIns": "usage"
```
这个语法作用就是: 只会对我们index.js当前要打包的文件中使用过的语法,比如Promise,map做polyfill,其他es6未出现的语法,我们暂时不去做polyfill,这样子,打包后的文件就减少体积了。

「总结」
- 需要按照babel-loader @babel/core这些库,@babel/core是它的核心库
- @babel/preset-env 它包含了es6翻译成es5的语法规则
- @babel/polyfill 解决了低版本浏览器无法实现的一些es6语法,使用polyfill自己来实现
- 通过import "@babel/polyfill"; 在js文件开头引入,完成对es6语法的polyfill
- 以上的场景都是解决的问题是业务中遇到babel的问题

当你生成第三方模块时,或者是生成UI组件库时,使用polyfill解决问题,就会出现问题了,上面的场景使用babel会污染环境,这个时候,我们需要换一种方案来解决👇

「@babel/plugin-transform-runtime」这个库就能解决我们的问题,那我们先安装需要的库
```bash
npm install --save-dev @babel/plugin-transform-runtime

npm install --save @babel/runtime
```
我们这个时候可以在根目录下建一个.babelrc文件,将原本要在options中的配置信息写在.babelrc文件👇
```js
{
    
    "plugins": [
      [
        "@babel/plugin-transform-runtime",
        {
          "corejs": 2,
          "helpers": true,
          "regenerator": true,
          "useESModules": false
        }
      ]
    ]
  }
```
```bash
// 当你的 "corejs": 2,需要安装下面这个
npm install --save @babel/runtime-corejs2
```
这样子的话,在使用语法是,就不需要去通过import "@babel/polyfill";这样子的语法去完成了,直接正常写就行了,而且从打包的体积来看,其实可以接受的。

「总结」
- 从业务场景来看,可以使用@babel/preset-env
- 从自己生成第三方库或者时UI时,使用@babel/plugin-transform-runtime,它作用是将 helper 和 polyfill 都改为从一个统一的地方引- 入，并且引入的对象和全局变量是完全隔离的,避免了全局的污染















<br>
<br>
<br>
<br>