---
title: 构建一个npm包管理工具
date: 2020-12-31
tags: 
    - npm
categories: cli
keywords: [npm]
description: 如何整合一个共享的代码库
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s1.ax1x.com/2020/10/21/BCVpFg.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

[![npm webpack](https://s1.ax1x.com/2020/10/23/BE0IOg.jpg)](https://imgchr.com/i/BE0IOg)


## 一、⚒ 创建一个基础npm架子

### 1.1 项目初始化，创建一个`package.json`

```bash
$ npm init
```

- 关于版本制定 [语义化版本 2.0.0](https://semver.org/lang/zh-CN/)。
- 关于`main`字段（指定打包的出口文件）`eg: lib/index.min.js`。
- 关于`scripts`字段 （项目运行、打包、测试、链接等操作） `eg: "build": "webpack --config build/webpack.config.js --mode production"`。

<br>

### 1.2 创建 `.gitignore`

- 关于 [.gitignore](https://github.com/toptal/gitignore.io)

```bash
dist/
node_modules/
yarn.lock
yarn-error.log
package-lock.json
```

<br>

### 1.3 创建 `.browserslistrc`

- 关于浏览器兼容问题 [browserslistrc](https://github.com/browserslist/browserslist)

```bash
# Browsers that we support
> 1%
```

<br>

### 1.4 文件目录结构

```bash
- build
    - webpack.config.js
- docs
    - XX.md
- lib
    - index.min.js
- src
    - helpers
    - instances
    - services
    - index.js
- package.json
- .....
```

<br>

### 1.5 配置 [webpack](https://webpack.js.org/configuration/)

1. 安装依赖

```bash
$ yarn add webpack webpack-cli -D
```

2. 基础的出入口配置

```js
const path = require('path')

module.exports = {
  entry: path.resolve(__dirname,'../src/index.js'),
  output: {
    filename: 'index.min.js',
    path: path.resolve(__dirname,'../lib'),
    libraryTarget: 'commonjs2'
  },
  module: {
    rules: []
  },
  plugins: [ ]
}
```

<br>

### 1.6 配置`babel`

- 需要注意转化 `ESNext` 新语法（@babel/preset-env）
- 需要注意转化 `Promise Map Set` 等语法（@babel/plugin-transform-runtime）
- 注意使用 `Class static` 语法（@babel/plugin-proposal-class-properties）
- [presets](https://babeljs.io/docs/en/presets) [plugins](https://babeljs.io/docs/en/plugins)

1. 安装`babel`相关依赖。

```bash
$ yarn add @babel/runtime @babel/runtime-corejs3 -S

$ yarn add @babel/core @babel/plugin-proposal-class-properties @babel/plugin-transform-runtime @babel/preset-env -D
```

2. 配置 `.babelrc` 文件。

```
{
  "presets": [
    ["@babel/preset-env", {
      "targets": {
        "browsers": "last 2 versions, not ie <= 9"
      }
    }]
  ],
  "plugins": [
    "@babel/plugin-proposal-class-properties",
    [
      "@babel/plugin-transform-runtime",
      { "corejs": 3 }
    ]
  ]
}
```

3. [webpack添加babel-loader规则](https://webpack.docschina.org/loaders/babel-loader/)。

- 安装依赖

```bash
$ yarn add babel-loader -D
```

```js
  module: {
    rules: [{
      test: /\.js$/,
      exclude: /node_modules/,
      loader: 'babel-loader'
    }]
  },
```

<br>

### 1.7 配置`eslint`

1. 安装依赖

```bash
yarn add eslint eslint-loader -D
```

2. [`.eslintrc` 配置文件](https://lousanpang.github.io/2018/07/05/other/use-eslint/)

```bash
{
  "env": {
    "browser": true,
    "es6": true,
    "commonjs": true,
    "node": true
  },
  "extends": "eslint:recommended",
  "parser": "babel-eslint",
  "parserOptions": {
    "sourceType": "module"
  },
  "globals": {},
  "rules": {
    "no-console": 1,
    "consistent-this": 1
  }
}
```

3. [webpack配置eslint](https://webpack.js.org/plugins/eslint-webpack-plugin/)

```js
module: {
    rules: [
        {
            test: /\.js[x]?$/,
            enforce: 'pre',
            use: [{
                loader: 'eslint-loader', 
                options: { fix: true }
            }],
            include: path.resolve(__dirname, './src/**/*.js'),
            exclude: /node_modules/
        },
    ]
}
```

4. `package.json` 配置

```bash
"scripts": {
    "lint": "eslint --ext .js src"
}

npm run lint
```

5. 想要`babel-eslint` 检测`ES6`代码

```bash
$ yarn add babel-eslint -D
```

```bash
{
    "parser": "babel-eslint",
    "rules": {}
}
```

## 二、webpack打包

```bash
// package.json
"scripts": {
"dev": "webpack --config build/webpack.config.js --mode development",
"build": "webpack --config build/webpack.config.js --mode production"
}

$ npm run build
```


## 三、测试

### 3.1 在包项目内

```bash
$ npm run build

$ npm link
```

### 3.2 在测试项目中

```bash
// xx 指的是 package.json内的 name 字段 （也就是发布包的的名称）
$ npm link xx
```


## 发布包

```bash
$ npm login

$ npm publish
```


<br>
<br>
<br>
<br>