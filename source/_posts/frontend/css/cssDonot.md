---
title: 你不知道的Css
date: 2019-12-6
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


### 二、加载性能优化css

#### 2.1 异步加载css

现在，rel="preload"5这一Web标准指出了如何异步加载资源，包括CSS类资源。

`<link rel="preload" href="mystyles.css" as="style" onload="this.rel='stylesheet'">`

注意，as是必须的。忽略as属性，或者错误的as属性会使preload等同于XHR请求，浏览器不知道加载的是什么内容，因此此类资源加载优先级会非常低。

#### 2.2 有选择的使用选择器（CSS选择器是从右向左匹配的）

1. 保持简单，不要使用嵌套过多过于复杂的选择器。

2. 通配符和属性选择器效率最低，需要匹配的元素最多，尽量避免使用。

3. 不要使用类选择器和ID选择器修饰元素标签，如h3#markdown-content，这样多此一举，还会降低效率。

4. 不要为了追求速度而放弃可读性与可维护性。

#### 2.3 减少使用昂贵的属性

在编写CSS时，我们应该尽量减少使用昂贵属性，如`box-shadow`/`border-radius`/`filter`/`透明度`/`:nth-child`等。

#### 2.4 优化重排与重绘

当**FPS为60**时,用户使用网站时才会感到流畅.需要在**16.67ms内**完成每次渲染相关的所有操作

**(1)、减少重排**

重排会导致浏览器重新计算整个文档，重新构建渲染树，这一过程会降低浏览器的渲染速度。如下所示，有很多操作会触发重排，我们应该避免频繁触发这些操作。

1. 改变`font-size`和`font-family`
2. 改变元素的内外边距
3. 通过JS改变CSS类
4. 通过JS获取DOM元素的位置相关属性（如width/height/left等）
5. CSS伪类激活
6. 滚动滚动条或者改变窗口大小

**(2)、避免不必要的重绘**

当元素的外观（如color，background，visibility等属性）发生改变时，会触发重绘。


### 三、css你不知道的布局技巧

#### 3.1 使用writing-mode排版竖文
```
writing-mode: horizontal-tb;  // 内容从左到右水平，从上到下垂直。下一条水平线位于上一条线的下方。
writing-mode: vertical-rl;  // 内容从上到下垂直流动，从右到左水平流动。下一条垂直线位于上一行的左侧。
writing-mode: vertical-lr;  // 内容从上到下垂直流动，从左到右水平流动。下一条垂直线位于前一行的右侧。
```

#### 3.2 使用text-align-last对齐两端文本
```
text-align-last: justify;
```

#### 3.3 使用object-fit规定图像尺寸
[object-fit](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)


<br>
<br>
<br>
<br>
<br>