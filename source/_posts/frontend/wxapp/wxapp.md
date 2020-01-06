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

### 二、小程序框架
#### 2.1 场景值
由于Android系统限制，目前还无法获取到按 Home 键退出到桌面，然后从桌面再次进小程序的场景值，对于这种情况，会保留上一次的场景值。

#### 2.2 视图层
（1）、template
```
<template name="staffName">
  <view>
    FirstName: {{firstName}}, LastName: {{lastName}}
  </view>
</template>

<template is="staffName" data="{{...staffA}}"></template>
<template is="staffName" data="{{...staffB}}"></template>
<template is="staffName" data="{{...staffC}}"></template>
```
```
Page({
  data: {
    staffA: {firstName: 'Hulk', lastName: 'Hu'},
    staffB: {firstName: 'Shang', lastName: 'You'},
    staffC: {firstName: 'Gideon', lastName: 'Lin'}
  }
})
```

(2)、wxs
由于运行环境的差异，在 iOS 设备上小程序内的 WXS 会比 JavaScript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。
```
<!--wxml-->

<wxs module="m1">
var msg = "hello world";
module.exports.message = msg;
</wxs>

<view> {{m1.message}} </view>
```

(3)、事件
1. target 和 currentTarget
target: 触发事件的组件的一些属性集合
currentTarget：当前组件的一些属性集合

2. dataset
必须驼峰命名 `data-xxYy`


### 三、小程序运行时
####3.1 [运行机制](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/operating-mechanism.html)

退出保留状态页
```
{
  "restartStrategy": "homePage" //（默认值）如果从这个页面退出小程序，下次将从首页冷启动
  "restartStrategy": "homePageAndLatestPage" // 如果从这个页面退出小程序，下次冷启动后立刻加载这个页面，页面的参数保持不变（不可用于 tab 页）
}
```

onSaveExitState 保留状态页数据
```
  onSaveExitState: function() {
    var exitState = { myDataField: 'myData' } // 需要保存的数据
    return {
      data: exitState,
      expireTimeStamp: Date.now() + 24 * 60 * 60 * 1000 // 超时时刻
    }
  }
```


### 四、自定义组件




### 十、性能优化
#### 10.1 [WXS响应事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/interactive-animation.html)
减少通信的次数，让事件在视图层（Webview）响应。

#### 10.2 [动画](https://developers.weixin.qq.com/miniprogram/dev/framework/view/animation.html)
1. 过将页面的 setData 改为 自定义组件 中的 setData 来提升性能。
不用本页面的setData。
2. 使用this.animate去制作关键帧动画。

####10.3 












<br>
<br>
<br>
<br>
<br>