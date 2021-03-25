---
title: Hexo腻了，试试VuePress
date: 2021-01-24
tags: 
    - 关于写作
categories: 关于写作
keywords: [写作]
description: VuePress搭建静态文档
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2021/01/24/sbRtgg.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


![](https://ftp.bmp.ovh/imgs/2021/02/98ae095cfb040cb5.png)


## [vuepress](https://vuepress.vuejs.org/zh/guide/)
按官方文档来，基本能搭个大概。但还是有一些不好理解的地方，下边就简单梳理下。

## 快速上手
### 初始化、安装
```bash
yarn init

yarn add -D vuepress
```

```json
// package.json

{
  "scripts": {
    "dev": "vuepress dev docs",
    "build": "vuepress build docs"
  },
  "devDependencies": {
    "vuepress": "^1.8.0"
  }
}

```

### 常见目录
```
- docs
    - .vuepress
        - public // 静态文件
        - config.js // 项目配置文件
    - guide // 文章
    - README.md // 首页
- .gitignore
- .travis.yml
- deploy.sh
- package.json
- 
```
关于每个文件的内容都在文档中，已经很详细了。

## 不理解的配置
### 关于侧边栏
项目的顶栏、侧边栏都在`config.js`配置的.
```js
// 侧边栏
sidebar: {
    '/guide/xxx/': [{
        title: 'A',
        path:'/guide/xxx/',
        children: [
            { title: 'Aa', path:'guide/xxx/aa' },
            { title: 'Ab', path:'guide/xxx/ab' }
        ]
    }]
}
```

### [关于GitHub自动部署](https://neveryu.github.io/2019/02/05/travis-ci/)

#### [使用GitHub Action自动部署](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)

#### 使用`travis-ci`部署
流程按照文档构建就可以了。关于token的问题：
1. 在[`travis`](https://travis-ci.org/)文档选择自己仓库。
2. 设置token
3. 在`deploy.sh`设置`git push -f https://${xxx}@github.com/xxx.git main:gh-pages`
xxx就是在`travis`中设置token的变量。





<br>
<br>
<br>
<br>
<br>