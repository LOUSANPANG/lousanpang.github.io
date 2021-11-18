---
title: uniapp跨端兼容性总结
date: 2021-11-17
tags: 
    - uniapp
categories: 跨端
keywords: [多端开发]
description: uniapp跨端兼容性总结
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2021/11/a2647d06a357f652.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

## 一、资源路径

### 1.1 模板内引入静态资源 `HBuilderX 2.6.6`
> template内引入静态资源，如image、video等标签的src属性时，可以使用相对路径或者绝对路径，形式如下

```html
<!-- ✅绝对路径，/static指根目录下的static目录，在cli项目中/static指src目录下的static目录 -->
<image class="logo" src="/static/logo.png"></image>
<image class="logo" src="@/static/logo.png"></image>
<!-- ✅相对路径 -->
<image class="logo" src="../../static/logo.png"></image>
```

**注意**

* @开头的绝对路径以及相对路径会经过base64转换规则校验
* 引入的静态资源在非h5平台，均不转为base64。
* H5平台，小于4kb的资源会被转换成base64，其余不转。
* 自HBuilderX 2.6.6起template内支持@开头路径引入静态资源，旧版本不支持此方式
* App平台自HBuilderX 2.6.9起template节点中引用静态资源文件时（如：图片），调整查找策略为【基于当前文件的路径搜索】，与其他平台保持一致
* 支付宝小程序组件内 image 标签不可使用相对路径

**建议**

* template内引入静态资源，尽可能使用绝对路径。


### 1.2 js文件引入 `HBuilderX 2.6.6`
```js
// ✅绝对路径，@指向项目根目录，在cli项目中@指向src目录
import add from '@/common/add.js'
// ✅相对路径
import add from '../../common/add.js'
// ❌不支持绝对路径
import add from '/common/add.js'
```


### 1.3 css引入静态资源 `HBuilderX 2.6.6`
```css
/* ✅绝对路径 */
@import url('/common/uni.css');
@import url('@/common/uni.css');
/* ✅相对路径 */
@import url('../../common/uni.css');

/* ✅绝对路径 */
background-image: url(/static/logo.png);
background-image: url(@/static/logo.png);
/* ✅相对路径 */
background-image: url(../../static/logo.png);
```

**注意**

* 引入字体图标请参考，字体图标
* @开头的绝对路径以及相对路径会经过base64转换规则校验
* 不支持本地图片的平台，小于40kb，一定会转base64。（共四个平台mp-weixin, mp-qq, mp-toutiao, app v2）
* h5平台，小于4kb会转base64，超出4kb时不转。
* 其余平台不会转base64

**建议**

* 导入的外联样式表使用相对路径


### 1.4 尺寸单位
> 在App端和H5端屏幕宽度达到 960px 时，默认将按照 375px 的屏幕宽度进行计算。


### 1.5 选择器
* `::after` `::before` 仅 vue 页面生效
* 在 uni-app 中不能使用 * 选择器
* 微信小程序自定义组件中仅支持 class 选择器


### 1.6 背景图片
* 支持 base64 格式图片
* 支持网络路径图片
* 不支持在css中使用本地文件


### 1.7 ES6支持情况
* iOS8 不支持 Array `values` `includes`
* Android 不支持 Array `values`
* `Proxy` 不支持在 iOS8	iOS9 Android 使用


## 二、nvue注意项

### 2.1 样式

* nvue页面暂不支持全局样式
* 不支持百分比单位、rem单位
* App端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px。
* nvue 在App端，还不支持 --status-bar-height变量，替代方案是在页面onLoad时通过uni.getSystemInfoSync().statusBarHeight获取状态栏高度，然后通过style绑定方式给占位view设定高度。



<br>
<br>
<br>
