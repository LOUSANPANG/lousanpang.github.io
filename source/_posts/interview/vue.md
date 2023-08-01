---
title: 前端面试复习计划之Vue
date: 2021-10-1
tags: 
    - Vue
categories: 面试
keywords: [面试]
description: 前端面试复习计划
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2022/11/02/0f410e4fe77a988d.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## Vue 组件通信
1、组件通信方式有以下8种：
- props
- $emit/~~$on~~
- ~~$children~~/$parent
- $attrs/~~$listeners~~
- ref
- $root
- eventbus
- vuex

2、根据组件之间的关系讨论通信：
- 父子组件
    - `ref`/`props`/`$attrs`/`$emit`/`$parent`
- 兄弟组件
    - `$parent`/`$root`/`enentbus`/`vuex`
- 跨层级关系
    - `eventbus`/`vuex`/`provide+inject`


## v-if 和 v-for 哪个优先级更高？
1、实践中不应该把 v-for 和 v-if 放一起
2、**在 Vue2 中，v-for 的优先级是高于 v-if**，把它们放在一起，输出的渲染函数中可以看出会先执行循环再判断条件，哪怕我们只渲染列表的一小部分元素，也得在每次重渲染的时候遍历整个列表，这会比较浪费；另外需要注意的是**在 Vue3 中则完全相反，v-if 的优先级高于 v-for**，所以 v-if 执行时，它调用的变量还不存在，就会导致异常。
3、在过滤列表中的目标时可以定义一个计算属性，先过滤好需要的列表再渲染。


## 简述生命周期及各阶段做的事
1、




<br />
<br />
<br />
<br />