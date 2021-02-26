---
title: rollup
date: 2021-1-15
tags: 
    - rollup
categories: cli
keywords: [rollup]
description: rollup打包工具
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://ftp.bmp.ovh/imgs/2021/02/c2e5aebd61f6b8ea.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


### [rollup](https://www.rollupjs.com/)
- 使用ES模块比CommonJS更好
- tree-shaking 自动编译移除无用代码片段


### 一、安装
```
yarn global add rollup
```

### 二、使用
创建 `rollup.config.js`
```js
export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  }
}
```

编译
```
// 使用配置文件
rollup -c
// 或者使用命令
rollup src/main.js -o bundle.js -f cjs
```

### 三、结合插件
安装
```
yarn add -D rollup-plugin-json
```

使用
```js
// rollup.config.js
import json from 'rollup-plugin-json';
export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [ json() ]
}
```


<br>
<br>
<br>
<br>