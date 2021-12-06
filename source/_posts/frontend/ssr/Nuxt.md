---
title: 服务器端渲染-Nuxtjs
date: 2021-12-2
tags: 
    - Nuxt
categories: SSR
keywords: [Nuxt]
description: 服务器端渲染
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2021/12/194fc0c8205f3acf.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

## 浏览器端渲染 (Client Side Render)
> 页面上的内容是我们加载的js文件渲染出来的，js文件运行在浏览器上面，服务端只返回一个html模板


## 服务器端渲染 (Server Side Render)
> 页面上的内容是通过服务端渲染生成的，浏览器直接显示服务端返回的html


## CSR SSR
> CSR配合预渲染方式（loading、骨架图）可以提前FP、FCP从而减少白屏问题，但无法提前FMP；SSR将FMP提前至js加载前触发，提前显示网页中的"主角元素"。SSR不仅可以减少白屏时间还可以大幅减少首屏加载时间。
* FP：首次绘制。浏览器开始请求网页到网页首帧绘制的时间点。
* FCP：首次内容绘制。FCP 标记的是浏览器渲染来自 DOM 第一位内容的时间点，该内容可能是文本、图像、SVG 甚至` <canvas> `元素。
* FMP：首次有效绘制。这是一个很主观的指标。根据业务的不同，每一个网站的有效内容都是不相同的，有效内容就是网页中"主角元素"。对于视频网站而言，主角元素就是视频。对于搜索引擎而言，主角元素就是搜索框。
* TTI：可交互时间。用于标记应用已进行视觉渲染并能可靠响应用户输入的时间点。应用可能会因为多种原因而无法响应用户输入：①页面组件运行所需的JavaScript尚未加载完成。②耗时较长的任务阻塞主线程。


## [Vue SSR 指南](https://ssr.vuejs.org/zh/guide/)
>[Nuxt.js提供了平滑开箱即用体验的更高层次解决方案](https://www.nuxtjs.cn/)

### Nuxtjs特点
* 服务端渲染
* 强大的路由功能、支持异步数据
* 静态文件服务
* ES6/7
* 打包压缩js、css
* HTML头部标签管理
* 本地开发热加载
* 集成各种预处理、语法规范等


<br>
<br>
<br>
