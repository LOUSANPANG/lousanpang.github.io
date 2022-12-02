---
title: 前端面试复习计划之Css
date: 2019-1-1
tags: 
    - Css
categories: 面试
keywords: [面试]
description: 前端面试复习计划
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2022/11/02/49274dae4082d11b.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## HTML5 语义化标签

- `header` `nav` `main` `article` `section` `aside` `footer`
- 代码结构清晰，易于阅读
- 利于开发和维护 方便其他设备解析（如屏幕阅读器）根据语义渲染网页
- 有利于搜索引擎优化（SEO），搜索引擎爬虫会根据不同的标签来赋予不同的权重


## HTML5 新特性

- 语义化标签
- 音视频处理API(audio,video)
- canvas / webGL
- 拖拽释放(Drag and drop) API
- history API
- requestAnimationFrame
- 地理位置(Geolocation)API
- webSocket
- web存储 localStorage、SessionStorage
- 表单控件，calendar、date、time、email、url、search


## CSS 选择器及优先级

**选择器**

- id选择器(#myid)
- 类选择器(.myclass)
- 属性选择器(a[rel="external"])
- 伪类选择器(a:hover, li:nth-child)
- 标签选择器(div, h1,p)
- 相邻选择器（h1 + p）
- 子选择器(ul > li)
- 后代选择器(li a)
- 通配符选择器(*)

**优先级** `!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性`

- !important
- 内联样式（1000）
- ID选择器（0100）
- 类选择器/属性选择器/伪类选择器（0010）
- 元素选择器/伪元素选择器（0001）
- 关系选择器/通配符选择器（0000）


## 渐进增强与优雅降级的理解及区别

**渐进增强（Progressive Enhancement）：** 一开始就针对低版本浏览器进行构建页面，完成基本的功能，然后再针对高级浏览器进行效果、交互、追加功能达到更好的体验。

**优雅降级（Graceful Degradation）：** 一开始就构建站点的完整功能，然后针对浏览器测试和修复。比如一开始使用 CSS3 的特性构建了一个应用，然后逐步针对各大浏览器进行 hack 使其可以在低版本浏览器上正常浏览。 两者区别 1、广义： 其实要定义一个基准线，在此之上的增强叫做渐进增强，在此之下的兼容叫优雅降级 2、狭义： 渐进增强一般说的是使用CSS3技术，在不影响老浏览器的正常显示与使用情形下来增强体验，而优雅降级则是体现html标签的语义，以便在js/css的加载失败/被禁用时，也不影响用户的相应功能。

```css
/* 例子 */
.transition { /*渐进增强写法*/
  -webkit-transition: all .5s;
     -moz-transition: all .5s;
       -o-transition: all .5s;
          transition: all .5s;
}
.transition { /*优雅降级写法*/
          transition: all .5s;
       -o-transition: all .5s;
     -moz-transition: all .5s;
  -webkit-transition: all .5s;
}
```


## CSS3新特性

过渡`transition`、动画`animation`、形状转换`transform`、选择器`nth-of-type()`、阴影、边框、背景、文字、渐变、Filter（滤镜）、弹性布局、栅格布局、多列布局、媒体查询


## position 属性的值有哪些及其区别

- 固定定位 fixed： 元素的位置相对于浏览器窗口是固定位置，即使窗口是滚动的它也不会移动。Fixed 定 位使元素的位置与文档流无关，因此不占据空间。 Fixed 定位的元素和其他元素重叠。
- 相对定位 relative： 如果对一个元素进行相对定位，它将出现在它所在的位置上。然后，可以通过设置垂直 或水平位置，让这个元素“相对于”它的起点进行移动。 在使用相对定位时，无论是 否进行移动，元素仍然占据原来的空间。因此，移动元素会导致它覆盖其它框。
- 绝对定位 absolute： 绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那 么它的位置相对于。absolute 定位使元素的位置与文档流无关，因此不占据空间。 absolute 定位的元素和其他元素重叠。
- 粘性定位 sticky： 元素先按照普通文档流定位，然后相对于该元素在流中的 flow root（BFC）和 containing block（最近的块级祖先元素）定位。而后，元素定位表现为在跨越特定阈值前为相对定 位，之后为固定定位。
- 默认定位 Static： 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声 明）。 inherit: 规定应该从父元素继承 position 属性的值。


## box-sizing属性

- box-sizing 规定两个并排的带边框的框，语法为 box-sizing：content-box/border-box/inherit
- content-box：宽度和高度分别应用到元素的内容框，在宽度和高度之外绘制元素的内边距和边框。【标准盒子模型】
- border-box：为元素设定的宽度和高度决定了元素的边框盒。【IE 盒子模型】
- inherit：继承父元素的 box-sizing 值


## CSS 盒子模型

CSS 盒模型本质上是一个盒子，它包括：边距，边框，填充和实际内容。CSS 中的盒子模型包括 IE 盒子模型和标准的 W3C 盒子模型。

- 在标准的盒子模型中，width 指 content 部分的宽度。
- 在 IE 盒子模型中，width 表示 content+padding+border 这三个部分的宽度。
- 标准盒模型： 一个块的总宽度 = width+margin(左右)+padding(左右)+border(左右)
- 怪异盒模型： 一个块的总宽度 = width+margin（左右）（既 width 已经包含了 padding 和 border 值）


## BFC（块级格式上下文）

BFC 是 Block Formatting Context 的缩写，即块级格式化上下文。BFC是CSS布局的一个概念，是一个独立的渲染区域，规定了内部box如何布局， 并且这个区域的子元素不会影响到外面的元素，其中比较重要的布局规则有内部 box 垂直放置，计算 BFC 的高度的时候，浮动元素也参与计算。

**BFC的原理布局规则**

- 内部的Box会在垂直方向，一个接一个地放置
- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
- 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反
- BFC的区域不会与float box重叠
- BFC是一个独立容器，容器里面的子元素不会影响到外面的元素
- 计算BFC的高度时，浮动元素也参与计算高度
- 元素的类型和display属性，决定了这个Box的类型。不同类型的Box会参与不同的Formatting Context

**如何创建BFC**

- 根元素，即HTML元素
- float的值不为none
- position为absolute或fixed
- display的值为inline-block、table-cell、table-caption
- overflow的值不为visible

**BFC的使用场景**

- 去除边距重叠现象
- 清除浮动（让父元素的高度包含子浮动元素）
- 避免某元素被浮动元素覆盖
- 避免多列布局由于宽度计算四舍五入而自动换行


## 让一个元素水平垂直居中

**水平居中**

- 对于 行内元素 : text-align: center;
- 对于确定宽度的块级元素：
    - width和margin实现。margin: 0 auto;
    - 绝对定位和margin-left: margin-left: (父width - 子width）/2, 前提是父元素position: relative

- 对于宽度未知的块级元素
    - table标签配合margin左右auto实现水平居中。使用table标签（或直接将块级元素设值为 display:table），再通过给该标签添加左右margin为auto。
    - inline-block实现水平居中方法。display：inline-block和text-align:center实现水平居中。
    - 绝对定位+transform，translateX可以移动本身元素的50%。
    - flex布局使用justify-content:center

**垂直居中**

- 利用 line-height 实现居中，这种方法适合纯文字类
- 通过设置父容器 相对定位 ，子级设置 绝对定位，标签通过margin实现自适应居中
- 弹性布局 flex :父级设置display: flex; 子级设置margin为auto实现自适应居中
- 父级设置相对定位，子级设置绝对定位，并且通过位移 transform 实现
- table 布局，父级通过转换成表格形式，然后子级设置 vertical-align 实现。（需要注意的是：vertical-align: middle使用的前提条件是内联元素以及display值为table-cell的元素）。


## 用CSS实现三角符号

```css
/*记忆口诀：盒子宽高均为零，三面边框皆透明。 */
div:after{
    position: absolute;
    width: 0px;
    height: 0px;
    content: " ";
    border-right: 100px solid transparent;
    border-top: 100px solid #ff0;
    border-left: 100px solid transparent;
    border-bottom: 100px solid transparent;
}
```


## 页面布局

- Flex 布局
- Rem 布局
- 百分比布局
- 浮动布局


## 如何使用rem或viewport进行移动端适配

**rem适配原理：**改变了一个元素在不同设备上占据的css像素的个数。

- 优点：没有破坏完美视口
- 缺点：px值转换rem太过于复杂(下面我们使用less来解决这个问题)


**viewport适配的原理：**每一个元素在不同设备上占据的css像素的个数是一样的。但是css像素和物理像素的比例是不一样的，等比的。

- 在我们设计图上所量取的大小即为我们可以设置的像素大小，即所量即所设
- 缺点破坏完美视口


## 清除浮动的方式

- 父级添加overflow属性，或者设置高度
- 添加额外标签
```html
<div class="parent">
    //添加额外标签并且添加clear属性
    <div style="clear:both"></div>
    //也可以加一个br标签
</div>
```
- 建立伪类选择器清除浮动
```css
//在css中添加:after伪元素
.parent:after{
    /* 设置添加子元素的内容是空 */
    content: '';
    /* 设置添加子元素为块级元素 */
    display: block;
    /* 设置添加的子元素的高度0 */
    height: 0;
    /* 设置添加子元素看不见 */
    visibility: hidden;
    /* 设置clear：both */
    clear: both;
}
```



<br />
<br />
<br />
<br />