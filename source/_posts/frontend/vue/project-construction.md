---
title: 项目总结-vue@2.x搭建篇
date: 2019-10-11
tags: 
    - Vue
categories: 项目总结
keywords: [Vue]
description: cli初始化配置、vuex、ui、lodash、eslint、封装axios、默认打包性能优化、等组织规范
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover:  # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 描述
#### 关于vue，梳理了两篇文章，[一篇关于开发环境的搭建准备](https://lousanpang.github.io/2019/10/11/frontend/vue/project-construction/)，[另一篇关于生产环境的部署优化](https://lousanpang.github.io/2019/12/01/frontend/vue/project-optimization/)。

#### 这一篇是关于开发环境的搭建准备，主要是通过大纲和[代码库](https://github.com/LOUSANPANG/VueBuildTool)的方式进行列举参考。


### 一、构建工具配置方面
#### 1.1. [部署Angular 团队Git的规范，用 `git cz` 代替 `git commit`](https://lousanpang.github.io/2019/11/01/frontend/git/git/)

#### 1.2. [eslintc 全局变量、忽略规则配置](https://lousanpang.github.io/2018/07/05/other/use-eslint/)
```
.eslintrc.js

module.exports = {
  rules: {
    'generator-star-spacing': 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    "no-new": 0,
    "no-unused-vars":0
  },
  globals: {
    $CONFIG: false,
    $API: false,
  }
}
```

#### 1.3. webpack（build、config文件）
<!-- - 构建工具打包图片、css路径问题 -->
- 修改host (便于访问、演示)
```
config/index.js

module.exports = {
  dev: {
    host: '0.0.0.0'
  }
}
```

- [webpack配置proxy (本地解决跨域，域名前缀问题)](https://webpack.docschina.org/configuration/dev-server/#devserverproxy)
- [如果你的前端应用和后端 API 服务器没有运行在同一个主机上，你需要在开发环境下将 API 请求代理到 API 服务器。这个问题可以通过 vue.config.js 中的 devServer.proxy 选项来配置。](https://cli.vuejs.org/zh/config/#devserver)
```
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: '<url>', // 代理地址，这里设置的地址会代替axios中设置的baseURL
        ws: true, // proxy websockets
        changeOrigin: true, // 如果接口跨域，需要进行这个参数配置
        pathRewrite: { // 重写url
          '^/api': '/api/v1' // target + /api/v1
          //pathRewrite: {'^/api': '/'} 重写之后url为 <url>/
          //pathRewrite: {'^/api': '/api'} 重写之后url为 <url>/api
        }
      },
      '/foo': {
        target: '<other_url>'
      }
    }
  }
}
```

- 修改alias （便捷的路径访问）
```
build/webpack.base.config.js

module.exports = {
  resolve: {
    alias: {
      '@': resolve('src')
    }
  }
}
```


### 二、 添加常用文件方面
#### 2.1. [`static config.js` 添加静态全局服务route（对应微服务、不同环境地址切换服务前缀）](https://github.com/LOUSANPANG/VueBuildTool/tree/master/dev/static/config)

#### 2.2. `static lib` 添加静态资源（mapbox min.css min.js min.json）

#### 2.3. `src style` 样式文件
- normalize.css 样式初始化重置 (解决各浏览器初始化兼容问题)
- base.css 基础样式(滚动条等)
- mixin.less/scss/stylus 功能样式 （常用颜色、字体、布局等）

#### 2.4. `src utils` 工具函数
- 常用方法工具函数

#### 2.5. 组件方面
- `src components` loading组件
- `src pages 404` 404页面


### 三 依赖、构建方面
#### 3.1. `src services`axios二次封装

#### 3.2. `src store` vuex二次封装

#### 3.3. `less scss stylus`css模块包

#### 3.4. ui库 + 二次封装

#### 3.5. babel低版本兼容

#### 3.6. `px2rem/vw` 适配方案

### 四、规范方面
#### 4.1. rule.vue vue规范文件示例

#### 4.2. vue代码风格指南





<br>
<br>
<br>
