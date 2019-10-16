---
title: Taro兼容性汇总
date: 2019-10-15
tags: 
    - Taro
categories: Taro
keywords: [Taro]
description: taro兼容性
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/10/14/uzsFTH.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

<table><tr><td height=100px bgcolor=#FDEEE9>[实时更新] 目前Taro版本 1.3.19</td></tr></table> 
<br>

[各端开发前注意](https://nervjs.github.io/taro/docs/before-dev-remind.html)
[ReactNative 样式表](https://nervjs.github.io/taro/docs/before-dev-remind.html#properties-属性)
<br>

### 一、样式
#### 1.1 布局
<table>
    <tr><td height=50px bgcolor=#F5F5D5>ReactNative 必须采用 Flex 布局</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5></td></tr>

</table> 
<br>

#### 1.2 选择器
<table>
    <tr><td height=50px bgcolor=#F5F5D5>ReactNative 仅支持类选择器，且不支持组合器, 不支持伪类及伪元素</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5></td></tr>

</table>
<br>

### 1.3 CSS API
<table>
    <tr><td height=50px bgcolor=#F5F5D5>ReactNative 这方面支持得并不好（仅 ios 支持且支持程度有限）</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5>ReactNative 不允许 属性简写例如：border、margin、padding, 必须拆分成 border-width ...</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5>ReactNative 不支持 background-image</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5>ReactNative 不支持 position:fixed</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5>ReactNative 不支持 动画</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5></td></tr>
    
</table>
<br>

### 二、配置项 app.jsx
#### 2.1 [全局配置项列表](https://nervjs.github.io/taro/docs/tutorial.html#%E9%85%8D%E7%BD%AE%E9%A1%B9%E5%88%97%E8%A1%A8)

#### 2.2 [页面配置项列表](https://nervjs.github.io/taro/docs/tutorial.html#%E9%85%8D%E7%BD%AE%E9%A1%B9%E5%88%97%E8%A1%A8-1)

#### 2.3 [页面事件处理函数](https://nervjs.github.io/taro/docs/tutorial.html#%E9%A1%B5%E9%9D%A2%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0)
<br>

### 三、[项目配置](https://nervjs.github.io/taro/docs/project-config.html)
**通过 Taro 模板创建的项目都会默认拥有 project.config.json 这一项目配置文件，这个文件 只能用于微信小程序，若要兼容到其他小程序平台，请按如下对应规则来增加相应平台的配置文件，其配置与各自小程序平台要求的一致**

### 四、[多端开发](https://nervjs.github.io/taro/docs/envs.html)










<br>
<br>
<br>
<br>
<br>