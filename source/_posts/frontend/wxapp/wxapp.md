---
title: 再次深入微信小程序
date: 2019-11-10
tags: 
    - 微信小程序
categories: 微信小程序
keywords: [小程序]
description: 微信小程序
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/12/20/QX33sU.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 一、小程序有普通网页的区别
小程序的逻辑层和渲染层是分开的 --> 逻辑层运行在`JSCore` --> 没有完整的浏览器对象 --> 缺少DOM BOM api
同Nodejs环境也不尽相同 --> NPM包无法运行

运行环境 | 逻辑层 | 渲染层
------- | ------- | -------
iOS | JavaScriptCore | WKWebView
安卓 | V8 | chromium定制内核
小程序开发者工具 | NWJS | Chrome WebView













<br>
<br>
<br>
<br>
<br>