---
title: 你不知道的CSS
date: 2018-06-01
tags: 
    - CSS3
categories: CSS3
keywords: [CSS3]
description: 你不知道的CSS3
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


#### 1.2 边距合并
当两个垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

只有普通文档流中块框的垂直外边距才会发生外边距合并。 行内框、浮动框或绝对定位之间的外边距不会合并。




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


#### 3.4 页面自适应最佳实践
经过大型项目实践，下面这段CSS是最好的基于rem和vm和calc实践代码
```css
html {
    font-size: 16px;
}
@media screen and (min-width: 375px) {
    html {
        /* iPhone6的375px尺寸作为16px基准，414px正好18px大小, 600 20px */
        font-size: calc(100% + 2 * (100vw - 375px) / 39);
        font-size: calc(16px + 2 * (100vw - 375px) / 39);
    }
}
@media screen and (min-width: 414px) {
    html {
        /* 414px-1000px每100像素宽字体增加1px(18px-22px) */
        font-size: calc(112.5% + 4 * (100vw - 414px) / 586);
        font-size: calc(18px + 4 * (100vw - 414px) / 586);
    }
}
@media screen and (min-width: 600px) {
    html {
        /* 600px-1000px每100像素宽字体增加1px(20px-24px) */
        font-size: calc(125% + 4 * (100vw - 600px) / 400);
        font-size: calc(20px + 4 * (100vw - 600px) / 400);
    }
}
@media screen and (min-width: 1000px) {
    html {
        /* 1000px往后是每100像素0.5px增加 */
        font-size: calc(137.5% + 6 * (100vw - 1000px) / 1000);
        font-size: calc(22px + 6 * (100vw - 1000px) / 1000);
    }
}
```

#### 3.5 七种方式实现三栏布局
```css
.left {
  width: 200px;
  height: 300px;
  background: olive;
}
.right {
  width: 200px;
  height: 300px;
  background: olivedrab;
}
.center {
  height: 300px;
  background: red;
}
```

flex、圣杯布局
```html
<div class="container">
    <div class="center"></div>
    <div class="left"></div>
    <div class="right"></div>
</div>
```
```css
/* flex */
.container {
  display: flex;
}
.center {
  flex-grow: 1;
}
.left {
  order: -1;
}

/* 圣杯布局 */
.container {
  padding: 0 200px;
}
.container div {
  float: left;
}
.center {
  width: 100%;
}
.left {
  position: relative;
  left: -200px;
  margin-left: -100%;
}
.right {
  position: relative;
  left: 200px;
  margin-left: -200px;
}
```

双飞翼布局
```html
<div class="container">
    <div class="center"></div>
</div>-
<div class="left"></div>
<div class="right"></div>
```
```css
.container {
  float: left;
  width: 100%;
}
.container .center {
  margin: 0 200px;
}
.left {
  float: left;
  margin-left: -100%;
}
.right {
  float: right;
  margin-left: -200px;
}
```

flot流体布局、bfc布局、position布局
```html
  <div class="container">
    <div class="left"></div>
    <div class="right"></div>
    <div class="center"></div>
  </div>
```
```css
/* flot 流体布局 */
.left {
  float: left;
}
.right {
  float: right;
}
.center {
  margin: 0 200px;
}

/* bfc布局 */
.left {
  float: left;
}
.right {
  float: right;
}
.center {
  overflow: hidden;
}

/* position */
.left {
  position: absolute;
  left: 0;
  top: 0;
}
.center {
  position: absolute;
  top: 0;
  left: 200px;
  right: 200px;
}
.right {
  position: absolute;
  right: 0;
  top: 0;
}
```



### 四、css好用的属性

#### 4.1 background-attachment属性
背景图像位置
```
background-attachment: scroll; // 默认值。背景图像会随着页面其余部分的滚动而移动。
background-attachment: fixed; // 当页面的其余部分滚动时，背景图像不会移动。
```

#### 4.2 文本
**（1）空隙**
`word-spacing` 单词之间的空隙， `letter-spacing` 字符之间的空隙；

**（2）大小写**
`text-transform 处理文本大小写`
```
uppercase 转换全部大写

lowercase 转换全部小写

capitalize 首字母大写
```

**（3）换行**
`white-space` 处理空白符
```
white-space: pre; // 识别换行符和空格，按标签内容格式来展示

white-space: nowrap; // 文本不换行
```
值 | 空白符 | 换行符 | 自动换行
------- | ------- | ------- | -------
pre-line | 合并 | 保留 | 允许
normal | 合并 | 忽略 | 允许
nowrap | 合并 | 忽略 | 不允许
pre | 保留 | 保留 | 不允许
pre-wrap | 保留 | 保留 | 允许

#### 4.3 链接
```
a:link    /* 未被访问的链接 */
a:visited /* 已被访问的链接 */
a:hover   /* 鼠标指针移动到链接上 */
a:active  /* 正在被点击的链接 */

a:hover 必须位于 a:link 和 a:visited 之后！！
a:active 必须位于 a:hover 之后！！
```

#### 4.4 设置可拖动的div
```css
div {
  resize:both;
  overflow:auto;
}
```

#### 4.5 使用currentColor来简化css
设置border-color、background-color等颜色的时候，可以使用currentColor[与当前元素的字体颜色相同]来简化css。
```css
.div{
    color: rgba(0,0,0,.85);
    font-weight: 500;
    text-align: left;
    padding: 20px;
    border: solid 1px currentColor;
}
```

#### 4.6 利用灰色滤镜做样式的disable效果
灰色图可以直接加滤镜，不用切多一张图。
```css
.coupon_style .disable {
  -webkit-filter: grayscale(1);
}
```

#### 4.7 图片自适应占位方式
给容器添加一个伪元素的子元素用于撑起内容，该子元素拥有一个padding-top:100%，同时给容器一个max-height尝试限制容器的高度，最后内容用绝对定位的方式添加即可
```css
#container{
    width: 50%;
    max-height:300px;
    background-color:#ddd;
    /*由于margin存在塌陷的问题，需要通过构建BFC来保证容器不会受到影响，因此这里可以给容器一个overflow:hidden来保证伪元素的margin不会塌陷。*/
    overflow:hidden;
    position: relative; /* 父容器相对定位 */
}
.placeholder::after{
    content:"";
    display:block;
    margin-top:100%;
}
img{
    position:absolute;  /* 内容绝对定位 */
    left: 50%;
    top: 50%;      
    transform: translateX(-50%) translateY(-50%); /* 控制内容绝对定位位置 */
    width:80%;   /* 控制图片不溢出，因此这里使用的图片实际宽度受父容器影响 */
}
```

#### 4.8 css动态变量
```css
:root {
  --main-text-color: #ff4400;
}
.u-theme-text {
  color: var(--main-text-color);
}
```













<br>
<br>
<br>
<br>
<br>