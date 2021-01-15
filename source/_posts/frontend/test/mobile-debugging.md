---
title: 移动端调试方法总结
date: 2021-1-15
tags: 
    - 测试
categories: 测试
keywords: [测试]
description: 移动端调试方法总结
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2021/01/15/s0DWaq.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

最近要分析web页面，在安卓和ios上的性能差异，除了操作系统本身不同之外，应该还多地方要探究的，第一步就是要在真机上分析。所以总结一下几个方法。

### Mac+iPhone+Lightning+Safari 浏览器

步骤：

1. 用：Lighting线将mac与iphone相连
2. iphone打开Web检查器（设置->Safari->高级->Web检查器）
3. iphone用safari打开要进行分析的页面
4. mac打开safari浏览器（菜单->开发->对应的手机名称->要调试的页面），点击即进入Safari Developer Tools，如图：

![safari](https://s3.ax1x.com/2021/01/15/s0DXIx.jpg)

5. 可以见到的调试界面是这样的

![Safari Developer Tools](https://s3.ax1x.com/2021/01/15/s0rFeA.png)

**缺点：不能调试webView里面的页面**

 

### 安卓手机+安卓数据线+电脑

步骤：

1. 用数据线将手机与电脑相连
2. 手机开启use调试（安卓不同机型开启的步骤不尽相同，不知道的百度一下）
3. 打开chrome，输入chrome://inspect/#devices，勾选Discover USB devices
4. 用手机chrome打开要调试的网页（如果是其他webView页面，需要在app配置启动代码，详键官方教程）
5. 选择你要调试的页面进入

![调试的页面](https://s3.ax1x.com/2021/01/15/s0rPLd.png)

6. 可以见到是这样的调试界面

![调试界面](https://s3.ax1x.com/2021/01/15/s0ruQg.png)

**缺点：亲测，mac中调试界面与小米6手机的界面经常不同步，操作非常不方便，还好控制台还是能正常看东西**

### weinre

步骤：

1. 可以直接npm install weinre，然后启动，打开管理界面即可
2. 直接安装whistle，自带了weinre。还可以代理不同环境，具体教程见：https://avwo.github.io/whistle/rules/weinre.html

![weinre](https://s3.ax1x.com/2021/01/15/s0rMLj.png)

**缺点：可以说是极简主义了，步骤简单、调试简单、能调的也简单（就是查查元素，看看控制台，不能像chrome那些分析工具一样)**

### vConsole+whistle

步骤：

1. 安装 whistle 后打开面板，在 value 中新建 vConsole.js，然后到 `https://github.com/Tencent/vConsole/blob/dev/dist/vconsole.min.js` 拷贝代码到 vConsole.js 中
2. 写代理规则，如
```js
https://baidu.com/ js://{vConsole.js}
```
这样在手机看，就会有个控制台出现，能看到 console.log 出来的东西

**缺点：功能有限，无法看到dom结构等，只能看一些输出**

<br>
<br>
<br>
