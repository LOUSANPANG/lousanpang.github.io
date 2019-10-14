---
title: 重新学习Taro
date: 2019-10-14
tags: 
    - Taro
categories: Taro
keywords: [Taro]
description: taro-document
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/10/14/uzGiPe.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


### 用Taro做了几个项目之后，产生了很多疑问，这篇文章整理了再次学习Taro的新收获。
<br>


### 一、文档-快速开始
#### 1.1 关于配置打包和命令
配置- `config文件下的index.js`
```
    cssModules: {
      enable: true, // 默认为 false，如需使用 css modules 功能，则设为 true
    }
```
配置- `project.config.json`
```
    "appid": " ", // 小程序的appid
    "setting": {
        "urlCheck": true,
        "es6": false,
        "postcss": false,
        "minified": true // 压缩
    }
```
打包（去掉 --watch 将不会监听文件修改，并会对代码进行压缩打包）
```
# yarn
    $ yarn build:weapp

# npm script
    $ npm run build:weapp
```
命令
```
# Taro 所有命令及帮助
    $ taro --help

# 更新 Taro CLI 工具
    # taro
    $ taro update self
    # npm
    npm i -g @tarojs/cli@latest
    # yarn
    yarn global add @tarojs/cli@latest

# 更新项目中 Taro 相关的依赖
    $ taro update project

# 诊断
    $ taro doctor
```


#### 1.2 开发前注意
微信开发者工具的项目设置
  * 关闭 ES6 转 ES5 功能
  * 关闭上传代码时样式自动补全
  * 关闭代码压缩上传

ReactNative
[ReactNative 样式表](https://nervjs.github.io/taro/docs/before-dev-remind.html#properties-属性)