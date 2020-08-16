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

#### 关于vue两篇文章，[一篇关于开发环境的搭建准备](https://lousanpang.github.io/2019/10/11/frontend/vue/project-construction/)，[另一篇关于生产环境的部署优化](https://lousanpang.github.io/2019/12/01/frontend/vue/project-optimization/)。

#### 这一篇关于开发环境的搭建准备，主要是通过大纲和[代码库](https://github.com/LOUSANPANG/Frame_vueTemplate)的方式进行列举参考。

### 一、关于开发环境的搭建准备。

#### 1.1 **配置方面**
1. 部署Angular 团队Git的规范，用 `git cz` 代替 `git commit`
2. px -> rem/vw 适配
3. webpack打包路径问题
4. webpack其他基础开发常用配置（别名等）
5. eslintc 全局变量等忽略规则配置

#### 1.2 **初始化文件方面**
1. 样式文件 - `reset.css`(各浏览器兼容问题的css)
2. 样式文件 - `base.css`（滚动条设置等）
3. 工具函数 - `xx.utils.js`(常用方法的工具函数)
4. 全局配置问题static - `tstatic > config.js`(存储服务api前缀)
5. pages - 404页面

#### 1.3 **依赖方面**
1. axios二次封装 - `src > services`(状态码封装、服务拦截前后做拦截后的提示错误信息等)
2. vuex二次封装 - `src > store`(模块化的状态管理器)
3. scss/less/stylus - `src > style`(mixin.less variables.less)
4. ui库方面 - 二次封装缺少的功能等
5. 低版本兼容为题 babel

#### 1.4 **规范方面**
1. rule.vue 规范行的vue文件
2. 参考vue代码风格指南





<br>
<br>
<br>
