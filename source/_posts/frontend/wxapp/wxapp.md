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


### 四、[自定义组件(*)](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)
* 注意：在组件wxss中不应使用ID选择器、属性选择器和标签名选择器。

json设置`"component": true` --> 定义`Component` ---> 使用自定义组件`usingComponents` ---> 使用
```
// 组件
{
  "component": true
}

<view class="inner">
  {{innerText}}
</view>
<slot></slot>

Component({
  properties: {
    innerText: {
      type: String,
      value: 'default value',
    }
  },
  data: {
  },
  methods: {
  }
})
```
```
{
  "usingComponents": {
    "component-tag-name": "path/to/the/custom/component"
  }
}

<component-tag-name inner-text="Some text"></component-tag-name>
```


### 五、基础能力
#### 5.1 网络
请求、上传下载最大并发10个；

sockt并发5个；

只要成功接收到服务器返回，无论 statusCode 是多少，都会进入 success 回调。请开发者根据业务逻辑对返回值进行判断。

#### 5.2 [分包加载](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages/basic.html)

#### 5.3 [多线程worker](https://developers.weixin.qq.com/miniprogram/dev/framework/workers.html)

#### 5.4 自定义tabBar


### 六、开发能力
#### 6.1 获取手机号针对认证的小程序



### 十、性能优化
#### 10.1 [WXS响应事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/interactive-animation.html)
减少通信的次数，让事件在视图层（Webview）响应。

#### 10.2 [动画](https://developers.weixin.qq.com/miniprogram/dev/framework/view/animation.html)
1. 过将页面的 setData 改为 自定义组件 中的 setData 来提升性能。
不用本页面的setData。
2. 使用this.animate去制作关键帧动画。

####10.3 [周期性更新](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/background-fetch.html)

用户七天内使用过的小程序

周期性更新能够在用户未打开小程序的情况下，也能从服务器提前拉取数据，当用户打开小程序时可以更快地渲染页面，减少用户等待时间，增强在弱网条件下的可用性。

#### 10.4 [数据预拉取](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/pre-fetch.html)

####10.5 setData
* 毫秒级的操作setData
* setData传递大量数据
* 后台页面进行setData

#### 10.6 避免太大的图片

#### 10.7 代码包大小的优化
代码包大小直接影响到下载速度，从而影响用户的首次打开体验


### 十一、性能评分
#### 11.1 首屏时间 < 5s
* 分批渲染首页内容
* 从服务端请求数据时间过长
* 一次性渲染数据太大

#### 11.2 渲染时间 < 500ms
* setData首次渲染的数据
* 首次渲染或因数据变化带来的页面结构变化的渲染花费的时间。

#### 11.3 脚本执行时间 < 1s
在事件或者是生命周期回调中同步执行的时间。

#### 11.4 setData
* 调用频率 < 20次/秒
* setData数据大小 < JSON.stringify 256kb
* setData 传入的数据都有相对应的依赖关系

#### 11.5 wxml页面节点数
* < 1000 个节点
* 节点深度 < 30 层
* 子节点 < 60个

#### 11.6 图片
* 缓存
* 大小
* 请求图片数 < 20个

#### 11.7 请求耗时 < 1s
请求的耗时太长会让用户一直等待甚至离开，应当优化好服务器处理时间、减小回包大小，让请求快速响应

#### 11.8 网络请求数 < 10个/秒
短时间内发起太多请求会触发小程序并行请求数量的限制，同时太多请求也可能导致加载慢等问题，应合理控制请求数量，甚至做请求的合并等

#### 11.9 网络请求缓存
3 分钟以内同一个url请求不出现两次回包大于 128KB 且一模一样的内容

#### 11.10 惯性滚动
```
-webkit-overflow-scrolling: touch
```

#### 11.11 避免使用:active伪类来实现点击态

##### 11.12 保持图片大小比例 < 15%

##### 11.13 可点击元素的响应区域

##### 11.14 iPhone X 兼容
```
padding-bottom: constant(safe-area-inset-bottom);
padding-bottom: env(safe-area-inset-bottom);
```

#### 11.15 合理的颜色搭配
1. 对于较大字体（font-size >= 24px，或同时满足font-size >= 19px与font-weight >= 700），文字颜色和背景颜色的对比度不小于3

2. 其他字体，文字颜色和背景颜色的对比度不小于4.5

#### 11.16 及时回收定时器

#### 11.17 wxss未使用的资源 < 2kb

#### 11.18 脚本异常、请求异常、404、基础版本库













<br>
<br>
<br>
<br>
<br>