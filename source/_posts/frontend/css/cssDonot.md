---
title: 你不知道的Css
date: 2018-12-6
tags: 
    - Css
categories: Css
keywords: [Css]
description: Css
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/12/06/QJAUW6.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

**css常用小技巧**

### 一、定位

#### **1.1 `position: sticky` 动态固定"效果**

**(1) 使用**

例如，网页的搜索工具栏，初始加载时在自己的默认位置（relative定位）, 页面向下滚动时，工具栏变成固定位置，始终停留在页面头部（fixed定位）.

`sticky`生效的前提是，必须搭配`top、bottom、left、right`这四个属性一起使用，不能省略，否则等同于`relative`定位，不产生"动态固定"的效果。原因是这四个属性用来定义"偏移距离"，浏览器把它当作`sticky`的生效门槛。

它的具体规则是，当页面滚动，父元素开始脱离视口时（即部分不可见），只要与`sticky`元素的距离达到生效门槛，`relative`定位自动切换为`fixed`定位；等到父元素完全脱离视口时（即完全不可见），`fixed`定位自动切换回`relative`定位。

```
#toolbar {
  position: -webkit-sticky; /* safari 浏览器 */
  position: sticky; /* 其他浏览器 */
  top: 0;
  margin-top: 100px;
}
```

**(2) 应用**

顶部导航栏

滑动页面,堆叠效果



<br>
<br>
<br>
<br>
<br>