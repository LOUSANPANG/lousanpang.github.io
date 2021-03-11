---
title: CSS记录篇
date: 2019-05-01
tags: 
    - CSS
categories: CSS
keywords: [CSS]
description: CSS记录总结
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s1.ax1x.com/2020/08/11/aLklo8.th.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 一、提高开发效率、提高用户体验
![css3](https://s1.ax1x.com/2020/08/11/aLZMcQ.png)


### 二、常用的css动画

#### 2.1 音乐跳动动画
![Loading Images](https://s1.ax1x.com/2020/08/11/aLR4h9.png)
```
<div className='m-preloading'>
  <div className='m-preloading-line m-preloading-lineone' />
  <div className='m-preloading-line m-preloading-linetwo' />
  <div className='m-preloading-line m-preloading-linethree' />
</div>

.m-preloading {
  display: flex;
  position: absolute;
  z-index: 999;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
@keyframes anim-scY{
  0%, 100%{ transform:scaleY(0); }
  50%{  transform:scaleY(1); }
}
.m-preloading-line {
  width: 20px;
  height: 80px;
  margin: 0 5px;
  transform: scaleY(0);
  transform-origin: bottom;
  animation: anim-scY 0.5s linear infinite;
}
.m-preloading-lineone {
  background: #2274e7;
}
.m-preloading-linetwo {
  animation-delay:.1s;
  background: #22e795;
}
.m-preloading-linethree {
  animation-delay: .3s;
  background: #e79522;
}
```

#### 2.2 横向旋转动画1
![横向旋转动画1](https://s1.ax1x.com/2020/08/11/aLZMcQ.png)
```
<div class="m-loader">
  <h1 class="m-loader-title">LOADING</h1>
  <span class="m-loader-sc m-loader-sc1"></span>
  <span class="m-loader-sc m-loader-sc2"></span>
  <span class="m-loader-sc m-loader-sc3"></span>
</div>

.m-loader-title{
	text-align:center;
	color:#333;
	font-size:16px;
	letter-spacing:1px;
	font-weight:500;
}

.m-loader .m-loader-sc {
	position:absolute;
	left:50%;
	display:inline-block;
	margin-left:-10px;
	width:16px;
	height:16px;
	border-radius:50%;
	animation:3s infinite linear;
}
.m-loader .m-loader-sc1 {
	background:#E84C3D;
	animation:kiri 1.2s infinite linear;
}
.m-loader .m-loader-sc2 {
	background:#F1C40F;
	z-index:100;
}
.m-loader .m-loader-sc3 {
	background:#2FCC71;
	animation:kanan 1.2s infinite linear;
}

@keyframes kanan {
  0% {
    transform:translateX(20px);
  }
	50% {
    transform:translateX(-20px);
	}
	100% {
    transform:translateX(20px);
	  z-index:200;
	}
}
@keyframes kiri {
  0% {
    transform:translateX(-20px);
	  z-index:200;
  }
	50% {
    transform:translateX(20px);
	}
	100% {
    transform:translateX(-20px);
	}
}
```

#### 2.3 四圆点旋转动画
![圆形旋转动画](https://s1.ax1x.com/2020/08/11/aL7Z6J.png)
```
<div class="m-loading">
  <div class="m-loading-box">
    <div class="m-loading-box-c m-loading-box-c1"></div>
    <div class="m-loading-box-c m-loading-box-c2"></div>
    <div class="m-loading-box-c m-loading-box-c3"></div>
    <div class="m-loading-box-c m-loading-box-c4"></div>
  </div>
  <span class="m-loading-title">loading</span>
</div>

.m-loading {
  position: fixed;
  width: 100%;
  height: 100%;
  background: #16171d;
  opacity: 0.9;
}
.m-loading .m-loading-title {
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -50px;
  margin-top: 30px;
  text-align: center;
  width: 100px;
  height: 30px;
  color: #ff8c00;
  font-size: 12px;
}
.m-loading .m-loading-box {
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -30px;
  margin-top: -30px;
  width: 60px;
  height: 60px;
}

.m-loading .m-loading-box .m-loading-box-c {
  position: absolute;
  top: 10px;
  left: 10px;
  transform-origin: 20px 20px;
  content: '';
  width: 16px;
  height: 16px;
  background: #ff8c00;
  border-radius: 8px;
  animation: spin-a 2s infinite cubic-bezier(0.5, 0, 0.5, 1);
}
.m-loading .m-loading-box .m-loading-box-c2 {
  top: 10px;
  left: auto;
  right: 10px;
  transform-origin: -4px 20px;
  animation: spin-b 2s infinite cubic-bezier(0.5, 0, 0.5, 1);
}
.m-loading .m-loading-box .m-loading-box-c3 {
  top: auto;
  left: auto;
  right: 10px;
  bottom: 10px;
  transform-origin: -4px -4px;
  animation: spin-c 2s infinite cubic-bezier(0.5, 0, 0.5, 1);
}
.m-loading .m-loading-box .m-loading-box-c4 {
  top: auto;
  bottom: 10px;
  transform-origin: 20px -4px;
  animation: spin-d 2s infinite cubic-bezier(0.5, 0, 0.5, 1);
}

@keyframes spin-a {
  0% { transform: rotate(90deg); }
  50% { transform: rotate(180deg); }
  75% { transform: rotate(270deg); }
  100% { transform: rotate(360deg); }
}
@keyframes spin-b {
  0% { transform: rotate(90deg); }
  25% { transform: rotate(90deg); }
  25% { transform: rotate(180deg); }
  75% { transform: rotate(270deg); }
  100% { transform: rotate(360deg); }
}
@keyframes spin-c {
  0% { transform: rotate(90deg); }
  25% { transform: rotate(90deg); }
  50% { transform: rotate(180deg); }
  50% { transform: rotate(270deg); }
  100% { transform: rotate(360deg); }
}
@keyframes spin-d {
  0%   { transform: rotate(90deg); }
  25%  { transform: rotate(90deg); }
  50%  { transform: rotate(180deg); }
  75%  { transform: rotate(270deg); }
  75% { transform: rotate(360deg); }
  100% { transform: rotate(360deg); }
}
```

#### 2.4 条形旋转动画变颜色
![条形旋转动画变颜色](https://s1.ax1x.com/2020/08/11/aLqr9K.png)
```
<svg class="m-loading" width="65px" height="65px" viewBox="0 0 66 66">
   <circle class="m-loading-path" fill="none" stroke-width="6" stroke-linecap="round" cx="33" cy="33" r="30"></circle>
</svg>

.m-loading {
  animation: rotator 1.4s linear infinite;
}
.m-loading .m-loading-path {
  stroke-dasharray: 187;
  stroke-dashoffset: 0;
  transform-origin: center;
  animation:
    dash 1.4s ease-in-out infinite, 
    colors 5.6s ease-in-out infinite;
}
@keyframes rotator {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(270deg); }
}
@keyframes colors {
	0% { stroke: #4285F4; }
	25% { stroke: #DE3E35; }
	50% { stroke: #F7C223; }
	75% { stroke: #1B9A59; }
  100% { stroke: #4285F4; }
}
@keyframes dash {
  0% { stroke-dashoffset: 187; }
  50% {
    stroke-dashoffset: 47;
    transform:rotate(135deg);
  }
  100% {
    stroke-dashoffset: 187;
    transform:rotate(450deg);
  }
}
```

#### 2.5 跳动弹性的盒子
![跳动弹性的盒子](https://s1.ax1x.com/2020/08/11/aLOAsg.png)
```
<div class="boxLoading" />

.boxLoading {  
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  width: 50px;
  height: 50px;
}
.boxLoading:before {
  position: absolute;
  top: 59px;
  left: 0;
  content: '';
  width: 50px;
  height: 5px;
  background: #000;
  opacity: 0.1;
  border-radius: 50%;
  animation: shadow .5s linear infinite;
}
.boxLoading:after {
  position: absolute;
  top: 0;
  left: 0;
  content: '';
  width: 50px;
  height: 50px;
  background: #1A6844;
  animation: animate .5s linear infinite;
  border-radius: 3px;
}

@keyframes animate {
  17% { border-bottom-right-radius: 3px; }
  25% { transform: translateY(9px) rotate(22.5deg); }
  50% {
    transform: translateY(18px) scale(1, .9) rotate(45deg);
    border-bottom-right-radius: 40px;
  }
  75% { transform: translateY(9px) rotate(67.5deg); }
  100% { transform: translateY(0) rotate(90deg); }
}
@keyframes shadow {
  0%, 100% { transform: scale(1, 1); }
  50% { transform: scale(1.2, 1); }
}
```

#### 2.6 闪烁的点
![闪烁跳动的点](https://s1.ax1x.com/2020/08/31/dLNg5d.png)
```
  <div class="spinner">
    <div class="double-bounce1"></div>
    <div class="double-bounce2"></div>
  </div>

  .spinner {
    width: 40px;
    height: 40px;
    position: relative;
    margin: 100px auto;
  }
  .double-bounce1, .double-bounce2 {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #333;
    opacity: 0.6;
    position: absolute;
    top: 0;
    left: 0;
    -webkit-animation: sk-bounce 2.0s infinite ease-in-out;
    animation: sk-bounce 2.0s infinite ease-in-out;
  }
  .double-bounce2 {
    -webkit-animation-delay: -1.0s;
    animation-delay: -1.0s;
  }
  @-webkit-keyframes sk-bounce {
    0%, 100% { -webkit-transform: scale(0.0) }
    50% { -webkit-transform: scale(1.0) }
  }
  @keyframes sk-bounce {
    0%, 100% { 
      transform: scale(0.0);
      -webkit-transform: scale(0.0);
    } 50% { 
      transform: scale(1.0);
      -webkit-transform: scale(1.0);
    }
  }
```

#### 2.7 三个球逐渐显现
![三个球逐渐显现](https://s1.ax1x.com/2020/08/31/dLNcUH.png)
```
  <div class="spinner">
    <div class="bounce1"></div>
    <div class="bounce2"></div>
    <div class="bounce3"></div>
  </div>

  .spinner {
    margin: 100px auto 0;
    width: 70px;
    text-align: center;
  }
  .spinner > div {
    width: 18px;
    height: 18px;
    background-color: #333;
    border-radius: 100%;
    display: inline-block;
    -webkit-animation: sk-bouncedelay 1.4s infinite ease-in-out both;
    animation: sk-bouncedelay 1.4s infinite ease-in-out both;
  }
  .spinner .bounce1 {
    -webkit-animation-delay: -0.32s;
    animation-delay: -0.32s;
  }
  .spinner .bounce2 {
    -webkit-animation-delay: -0.16s;
    animation-delay: -0.16s;
  }
  @-webkit-keyframes sk-bouncedelay {
    0%, 80%, 100% { -webkit-transform: scale(0) }
    40% { -webkit-transform: scale(1.0) }
  }
  @keyframes sk-bouncedelay {
    0%, 80%, 100% { 
      -webkit-transform: scale(0);
      transform: scale(0);
    } 40% { 
      -webkit-transform: scale(1.0);
      transform: scale(1.0);
    }
  }
```

#### 2.8 折叠积木
![折叠积木](https://s1.ax1x.com/2020/08/31/dLN6Ve.png)
```
  <div class="sk-folding-cube">
    <div class="sk-cube1 sk-cube"></div>
    <div class="sk-cube2 sk-cube"></div>
    <div class="sk-cube4 sk-cube"></div>
    <div class="sk-cube3 sk-cube"></div>
  </div>

  .sk-folding-cube {
    margin: 20px auto;
    width: 40px;
    height: 40px;
    position: relative;
    -webkit-transform: rotateZ(45deg);
            transform: rotateZ(45deg);
  }
  .sk-folding-cube .sk-cube {
    float: left;
    width: 50%;
    height: 50%;
    position: relative;
    -webkit-transform: scale(1.1);
        -ms-transform: scale(1.1);
            transform: scale(1.1); 
  }
  .sk-folding-cube .sk-cube:before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #333;
    -webkit-animation: sk-foldCubeAngle 2.4s infinite linear both;
            animation: sk-foldCubeAngle 2.4s infinite linear both;
    -webkit-transform-origin: 100% 100%;
        -ms-transform-origin: 100% 100%;
            transform-origin: 100% 100%;
  }
  .sk-folding-cube .sk-cube2 {
    -webkit-transform: scale(1.1) rotateZ(90deg);
            transform: scale(1.1) rotateZ(90deg);
  }
  .sk-folding-cube .sk-cube3 {
    -webkit-transform: scale(1.1) rotateZ(180deg);
            transform: scale(1.1) rotateZ(180deg);
  }
  .sk-folding-cube .sk-cube4 {
    -webkit-transform: scale(1.1) rotateZ(270deg);
            transform: scale(1.1) rotateZ(270deg);
  }
  .sk-folding-cube .sk-cube2:before {
    -webkit-animation-delay: 0.3s;
            animation-delay: 0.3s;
  }
  .sk-folding-cube .sk-cube3:before {
    -webkit-animation-delay: 0.6s;
            animation-delay: 0.6s; 
  }
  .sk-folding-cube .sk-cube4:before {
    -webkit-animation-delay: 0.9s;
            animation-delay: 0.9s;
  }
  @-webkit-keyframes sk-foldCubeAngle {
    0%, 10% {
      -webkit-transform: perspective(140px) rotateX(-180deg);
              transform: perspective(140px) rotateX(-180deg);
      opacity: 0; 
    } 25%, 75% {
      -webkit-transform: perspective(140px) rotateX(0deg);
              transform: perspective(140px) rotateX(0deg);
      opacity: 1; 
    } 90%, 100% {
      -webkit-transform: perspective(140px) rotateY(180deg);
              transform: perspective(140px) rotateY(180deg);
      opacity: 0; 
    } 
  }
  @keyframes sk-foldCubeAngle {
    0%, 10% {
      -webkit-transform: perspective(140px) rotateX(-180deg);
              transform: perspective(140px) rotateX(-180deg);
      opacity: 0; 
    } 25%, 75% {
      -webkit-transform: perspective(140px) rotateX(0deg);
              transform: perspective(140px) rotateX(0deg);
      opacity: 1; 
    } 90%, 100% {
      -webkit-transform: perspective(140px) rotateY(180deg);
              transform: perspective(140px) rotateY(180deg);
      opacity: 0; 
    }
  }
```

#### 2.9 伸拉的输入框
![伸拉的输入框](https://s1.ax1x.com/2020/09/15/wsXYbd.gif)
```
<input class="search" type="text" placeholder="搜索...">

.search{
    width:80px;
    height:40px;
    border-radius:40px;
    border:2px solid lightblue;
    position: absolute;
    right:200px;
    outline:none;
    text-indent:12px;
    color:#666;
    font-size:16px;
    padding:0;
    -webkit-transition:width 0.5s;
}
.search:focus{
    width:200px;
}
```

#### 2.10 手浮右侧切入动画
![手浮右侧切入动画](https://s1.ax1x.com/2020/09/15/wsXNVA.gif)
```
<div class="banner1">
    <a href="javascript:;">博</a>
    <span>这是我的个人博客</span>
</div>

.banner1{
    width:234px;
    height:34px;
    border-radius:40px;
    position:absolute;
    top:400px;
    left:600px;
}
.banner1 a{
    display:inline-block;
    width:30px;
    height:30px;
    line-height:30px;
    border-radius:50%;
    border:2px solid lightblue;
    position:absolute;
    left:0px;top:0px;
    background:lightgreen;
    color:#fff;
    text-align:center;
    text-decoration:none;
    cursor:pointer;
    z-index:2;
}
.banner1 a:hover + span{
    -webkit-transform:translateX(40px);
    opacity:1;
}
.banner1 span{
    display:inline-block;
    width:auto;
    padding:0 20px;
    height:30px;line-height:30px;
    background:lightblue;
    border-radius:30px;
    text-align: center;
    color:#fff;
    position:absolute;
    top:2px;
    opacity:0;
    -webkit-transition:all 1s;
    -webkit-transform:translateX(80px);
}
```

#### 2.11 骨架屏动画
```css
@keyframes loading {
  to {
    background-position-x: -20%;
  }
}
div {
  background-color: #ededed;
  background: linear-gradient(
    100deg,
    rgba(255, 255, 255, 0) 40%,
    rgba(255, 255, 255, .5) 50%,
    rgba(255, 255, 255, 0) 60%
  ) #ededed;
  background-size: 200% 100%;
  background-position-x: 180%;
  animation: 1s loading ease-in-out infinite;
}
```

#### 2.12 蚂蚁行军图
![蚂蚁行军图](https://ftp.bmp.ovh/imgs/2021/02/bac1e1bc8815c062.gif)
```html
<div class="ants-border">
   Lorem ipsum, dolor sit amet consectetur adipisicing elit. Laudantium, debitis vel quam tempora ut nisi voluptatibus rem ab. Quas, cum. Itaque, dignissimos! Quos, eos placeat eveniet quasi amet eius officiis!
</div>
```
```css
.ants-border{
   width: 500px;
   padding: 1em;
   border: 1px solid transparent;
   background-image: linear-gradient(white, white), 
                     repeating-linear-gradient(
                        -45deg,
                        #000 0%, #000 25%,
                        #fff 0%, #fff 50%
                     );
  background-size: 10px 10px;
  background-clip: padding-box, border-box;
  animation: ants 10s linear infinite;
}
@keyframes ants {
  to {
     background-position: 100%;
  }
}
```

#### 2.13 裁切路径
![裁切棱形](https://ftp.bmp.ovh/imgs/2021/02/78c7b7381b205277.png)
![裁切椭圆](https://ftp.bmp.ovh/imgs/2021/02/99682a2a7fc055bf.png)
```html
<img class="pic" src="images/pic.jpg">
```
```css
.pic{
   clip-path: polygon(50% 0,100% 50%,50% 100%,0 50%);
}

.pic{
   clip-path: circle(50%);
}
```

#### 2.100 动画参考列表
[动画参考列表loader](https://codepen.io/vineethtrv/pen/NWxZqMM?editors=1100)


### 三、常用的css样式 `box-shadow` `hover`

#### 3.1 box-shoadow
* 用法
```
/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);

/* 插页(阴影向内) | x偏移量 | y偏移量 | 阴影颜色 */
box-shadow: inset 5em 1em gold;

/* 任意数量的阴影，以逗号分隔 */
box-shadow: 3px 3px red, -1em 0 0.4em olive;
```

* 示例
```
.u-box-shadow1 {
  box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
}

.u-box-shadow2 {
  box-shadow: 
    inset 0 -3em 3em rgba(0,0,0,0.1), 
          0 0  0 2px rgb(255,255,255),
          0.3em 0.3em 1em rgba(0,0,0,0.3);
}

.u-box-shadow3 {
  box-shadow: 0 30px 40px -15px rgba(0,0,0,0.35);
}

.u-box-shadow4 {
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
}

.u-box-shadow5 {
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
}

.u-box-shadow6 {
  box-shadow: 0 0 50px rgba(0,0,0,0.75);
}

.u-box-shadow7 {
  box-shadow: 0 1px 1px rgba(0,0,0,0.15), 
              0 2px 2px rgba(0,0,0,0.15), 
              0 4px 4px rgba(0,0,0,0.15), 
              0 8px 8px rgba(0,0,0,0.15);
}

.u-box-shadow8 {
  box-shadow: 0 1px 1px rgba(0,0,0,0.11), 
              0 2px 2px rgba(0,0,0,0.11), 
              0 4px 4px rgba(0,0,0,0.11), 
              0 8px 8px rgba(0,0,0,0.11), 
              0 16px 16px rgba(0,0,0,0.11), 
              0 32px 32px rgba(0,0,0,0.11);
}

.u-box-shadow9 {
    box-shadow: 0 1px 2px rgba(0,0,0,0.07), 
                0 2px 4px rgba(0,0,0,0.07), 
                0 4px 8px rgba(0,0,0,0.07), 
                0 8px 16px rgba(0,0,0,0.07),
                0 16px 32px rgba(0,0,0,0.07), 
                0 32px 64px rgba(0,0,0,0.07);
}
```

#### 3.2 `Hover`
![霓虹效果](https://s1.ax1x.com/2020/08/18/dutW7Q.gif)
```
<div id="neon-btn">
  <button class="btn one">Hover me</button>
  <button  class="btn two">Hover me</button>
  <button  class="btn three">Hover me</button>
</div>

#neon-btn {
  display: flex;
  align-items: center;
  justify-content: space-around;
  height: 100vh;
  background: #031628; 
}
.btn {
  border: 1px solid;
  background-color: transparent;
  text-transform: uppercase;
  font-size: 14px;
  padding: 10px 20px;
  font-weight: 300;
}
.one {
  color: #4cc9f0;
}
.two {
  color: #f038ff; 
}
.three {
  color: #b9e769;
}

.btn:hover {
  color: white;
  border: 0;
}

.one:hover {
  background-color: #4cc9f0;
  -webkit-box-shadow: 10px 10px 99px 6px rgba(76,201,240,1);
  -moz-box-shadow: 10px 10px 99px 6px rgba(76,201,240,1);
  box-shadow: 10px 10px 99px 6px rgba(76,201,240,1);
}
.two:hover {
  background-color: #f038ff;
  -webkit-box-shadow: 10px 10px 99px 6px rgba(240, 56, 255, 1);
  -moz-box-shadow: 10px 10px 99px 6px rgba(240, 56, 255, 1);
  box-shadow: 10px 10px 99px 6px rgba(240, 56, 255, 1);
}
.three:hover {
  background-color: #b9e769;
  -webkit-box-shadow: 10px 10px 99px 6px rgba(185, 231, 105, 1);
  -moz-box-shadow: 10px 10px 99px 6px rgba(185, 231, 105, 1);
  box-shadow: 10px 10px 99px 6px rgba(185, 231, 105, 1);
}
```

![链接下划线动画1](https://s1.ax1x.com/2020/08/31/dLdWG9.png)
```
  h2 > a {
    position: relative;
    color: #000;
    text-decoration: none;
  }
  h2 > a:hover {
    color: #000;
  }
  h2 > a::before {
    content: "";
    position: absolute;
    width: 100%;
    height: 2px;
    bottom: 0;
    left: 0;
    background-color: #000;
    visibility: hidden;
    transform: scaleX(0);
    transition: all 0.3s ease-in-out 0s;
  }
  h2 > a:hover::before {
    visibility: visible;
    transform: scaleX(1);
  }
```

![阴影效果](https://s1.ax1x.com/2020/08/31/dLd3KP.png)
```
  .box {
    position: relative;
    display: inline-block;
    width: 100px;
    height: 100px;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    -webkit-transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
    transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  }
  .box::after {
    content: "";
    border-radius: 5px;
    position: absolute;
    z-index: -1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
    opacity: 0;
    -webkit-transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
    transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  }
  .box:hover {
    -webkit-transform: scale(1.25, 1.25);
    transform: scale(1.25, 1.25);
  }
  .box:hover::after {
      opacity: 1;
  }
```

![边框效果](https://s1.ax1x.com/2020/08/18/dut2nS.gif)
```
<div id="draw-border">
  <button>Hover me</button>
</div>

#draw-border {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
button {
  border: 0;
  background: none;
  text-transform: uppercase;
  color: #4361ee;
  font-weight: bold;
  position: relative;
  outline: none;
  padding: 10px 20px;
  box-sizing: border-box;
}
button::before, button::after {
  box-sizing: inherit;
  position: absolute;
  content: '';
  border: 2px solid transparent;
  width: 0;
  height: 0;
}
button::after {
  bottom: 0;
  right: 0;
}
button::before {
  top: 0;
  left: 0;
}
button:hover::before, button:hover::after {
  width: 100%;
  height: 100%;
}
button:hover::before {
  border-top-color: #4361ee;
  border-right-color: #4361ee;
  transition: width 0.3s ease-out, height 0.3s ease-out 0.3s;
}
button:hover::after {
  border-bottom-color: #4361ee;
  border-left-color: #4361ee;
  transition: border-color 0s ease-out 0.6s, width 0.3s ease-out 0.6s, height 0.3s ease-out 1s;
}
```

![冰冻效果](https://s1.ax1x.com/2020/08/18/dutR0g.gif)
```
<div id="frozen-btn">
  <button class="green">Hover me</button>
  <button class="purple">Hover me</button>
</div>

#frozen-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
button {
  border: 0;
  margin: 20px;
  text-transform: uppercase;
  font-size: 20px;
  font-weight: bold;
  padding: 15px 50px;
  border-radius: 50px;
  color: white;
  outline: none;
  position: relative;
}
button:before{
  content: '';
  display: block;
  background: linear-gradient(to left, rgba(255, 255, 255, 0) 50%, rgba(255, 255, 255, 0.4) 50%);
  background-size: 210% 100%;
  background-position: right bottom;
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  bottom:0;
  right:0;
  left: 0;
  border-radius: 50px;
  transition: all 1s;
  -webkit-transition: all 1s;
}
.green {
   background-image: linear-gradient(to right, #25aae1, #40e495);
   box-shadow: 0 4px 15px 0 rgba(49, 196, 190, 0.75);
}
.purple {
   background-image: linear-gradient(to right, #6253e1, #852D91);
   box-shadow: 0 4px 15px 0 rgba(236, 116, 149, 0.75);
}
.purple:hover:before {
  background-position: left bottom;
}
.green:hover:before {
  background-position: left bottom;
}
```

![闪亮效果](https://s1.ax1x.com/2020/08/18/dutcX8.gif)
```
<div id="shiny-shadow">
  <button><span>Hover me</span></button>
</div>

#shiny-shadow {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: #1c2541;
}
button {
  border: 2px solid white;
  background: transparent;
  text-transform: uppercase;
  color: white;
  padding: 15px 50px;
  outline: none;
  overflow: hidden;
  position: relative;
}
span {
  z-index: 20;  
}
button:after {
  content: '';
    display: block;
    position: absolute;
    top: -36px;
    left: -100px;
    background: white;
    width: 50px;
    height: 125px;
    opacity: 20%;
    transform: rotate(-45deg);
}
button:hover:after {
  left: 120%;
  transition: all 600ms cubic-bezier(0.3, 1, 0.2, 1);
   -webkit-transition: all 600ms cubic-bezier(0.3, 1, 0.2, 1);
}
```

![缓动上移)](https://s1.ax1x.com/2020/10/20/BSjC0P.gif)
```
<div class="lists">
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
</div>
.lists{
    display: flex;
    margin-top: 100px;
    justify-content: center;
}
.box{
    width: 200px;
    height: 200px;
    transition: all 0.3s;
}
.box~.box{
    margin-left: 15px;
}
.box img{width: 100%;height: 100%;}
.box:hover{
    transform: translateY(-6px);
}
```


![放大效果](https://s1.ax1x.com/2020/10/20/BSjjEV.gif)
```
<div class="lists">
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
</div>
.lists{
    display: flex;
    margin-top: 100px;
    justify-content: center;
}
.box{
    width: 200px;
    height: 200px;
    transition: all 0.3s;
}
.box~.box{
    margin-left: 15px;
}
.box img{width: 100%;height: 100%;}
.box:hover{
    transform: scale(1.03);
}
```

![框内放大效果](https://s1.ax1x.com/2020/10/20/BSvp34.gif)
```
<div class="lists">
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
</div>

.lists{
    display: flex;
    margin-top: 100px;
    justify-content: center;
}
.box{
    width: 200px;
    height: 200px;
    overflow:hidden;
}
.box~.box{
    margin-left: 15px;
}
.box img{
    width: 100%;
    height: 100%;
    transition: all 0.5s;
}
.box:hover img{
    transform: scale(1.1);
}
```

![蒙层效果](https://s1.ax1x.com/2020/10/20/BSvEE6.gif)
```
<div class="lists">
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
    <a href="#" class="box">
        <img src="avatar.jpg">
    </a>
</div>
.lists{
    display: flex;
    margin-top: 100px;
    justify-content: center;
}
.box{
    width: 200px;
    height: 200px;
    overflow:hidden;
    background-color: #4f4f4f;
}
.box~.box{
    margin-left: 15px;
}
.box img{
    width: 100%;
    height: 100%;
    transition: all 0.5s;
}
.box:hover img{
    opacity: 0.8
}
```

![文字滑动进入](https://s1.ax1x.com/2020/10/20/BSvsaV.gif)
```
<div class="lists">
    <div class="item">
        <div class="text">
          <h1>标题名称</h1>
          <img src="avatar.jpg">
          <h2 class="animate-text">Kitty 的测试案例</h2>
          <p class="animate-text">这里放内容简介，内容简介,这里放内容简介，内容简介,这里放内容简介，内容简介</p>
          <div class="dots"> <span></span> <span></span> <span></span> </div>
        </div>
    </div>
</div>
* { margin: 0; padding: 0;}
.lists{
    width: 100%;
    display: flex;
    justify-content: center;
    margin-top: 100px;
}
.item {
    width: 300px;
    height: 300px;
    background-color: #99aeff;
    position: relative;
    cursor: pointer;
    transition: all 0.4s ease-out;
    box-shadow: 0px 35px 77px -17px rgba(0, 0, 0, 0.44);
    overflow: hidden;
    color: #fff;
}
.item img {
    height: 100%;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 0;
    transition: all 0.4s ease-out;
}
.item .text {
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    z-index: 99;
    position: absolute;
    padding: 30px;
}
.item h1 {
    text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.3);
}
.item h2 {
    margin-top: 20px;
    font-style: italic;
    transform: translateX(200px);
}
.item p {
    margin-top: 20px;
    line-height: 25px;
    transform: translateX(-200px);
    transition-delay: 0.2s;
}
.animate-text {
    opacity: 0;
    transition: all 0.6s ease-in-out;
}
.item:hover {
    box-shadow: 0px 35px 77px -17px rgba(0, 0, 0, 0.64);
    transform: scale(1.05);
}
.item:hover img {
    opacity: 0.2;
}
.item:hover .animate-text {
    transform: translateX(0);
    opacity: 1;
}
.dots {
    position: absolute;
    bottom: 20px;
    right: 30px;
    margin: 0 auto;
    width: 30px;
    height: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
}
.dots span {
    width: 5px;
    height: 5px;
    background-color: #fff;
    border-radius: 50%;
    display: block;
    opacity: 0;
    transition: transform 0.4s ease-out, opacity 0.5s ease;
    transform: translateY(30px);
}
.item:hover span {
    opacity: 1;
    transform: translateY(0px);
}
.dots span:nth-child(1) {
    transition-delay: 0.05s;
}
.dots span:nth-child(2) {
    transition-delay: 0.1s;
}
.dots span:nth-child(3) {
    transition-delay: 0.15s;
}
```


### 四、常用的css样式 - `color background font` 渐变色

#### 4.1 传统色
[CHINESE COLOR](https://colors.ichuantong.cn/)

[Color Hunt](https://colorhunt.co/?nsukey=FXRkxgX1APqgqsz%2FDtFjIYurt4Ulhe%2BHECaAcwjEzniSmQC32%2FXWDBmECLVK%2FSd9x1a0iyKJq3fS4CFvZLYMGTYpa72pEMHgQ%2FnbRiDhh1IEYm4JtycTm90lIsseOJ30lIsWZkc%2FG49ULIuL1vOIwoPGrnFUZAWF25dMbOOl8us1LXHJ%2Fj%2B0Abh%2FLPzBsukZxUn8Ax3L7dChDYGxHDMXqg%3D%3D)

#### 4.2 渐变色
[渐变色参考链接](https://evankarageorgos.github.io/hue/grid.html)



### 五、404页面示例
![aONuAf.png](https://s1.ax1x.com/2020/08/11/aONuAf.png)

[暂停维护页面](https://codepen.io/takaneichinose/pen/oomjqm)
[404页面](https://codepen.io/ricardpriet/pen/MOKEam)
[404页面](https://codepen.io/rennan/pen/ACBKu)
[404页面](https://codepen.io/cluzier/pen/BOZmMp?editors=1100)
[404页面](https://codepen.io/nelsonleite/pen/zqZQLo)


### 六、常用的动画库
[Hover动画](https://github.com/IanLunn/Hover)
[制作动画工具生成代码](https://angrytools.com/css/animation/)
[animate.css](https://github.com/animate-css/animate.css)
[css3动画及效果开发参考大全](https://animista.net/play)


### 七、常见的css问题
[你不不知道的Css](https://lousanpang.github.io/2018/06/01/frontend/css/cssDonot/)

#### 7.1 不定宽溢出文本适配滚动
![不定宽溢出文本适配滚动](https://s1.ax1x.com/2020/10/10/0yttOS.gif)
```
  <div class="wrap">
    <p title="我的宽度是正常宽度">我的宽度是正常宽度</p>
    <p class="scroll" title="我的宽度是溢出了一小部分">我的宽度是溢出了一小部分</p>
    <p class="scroll" title="我的宽度是溢出了溢出了很大一部分">我的宽度是溢出了溢出了很大一部分</p>
  </div>

  .wrap {
    position: relative;
    width: 150px;
    overflow: hidden;
  }

  p {
      white-space: nowrap;
      display: inline-block;
  }
  p:hover {
      animation: move 1.5s infinite alternate linear;
  }
  <!-- 父级定宽 -->
  @keyframes move {
      0% {
          transform: translate(0, 0);
      }
      100% {
          transform: translate(calc(-100% + 150px), 0);
      }
  }
  <!-- 父级不定宽 -->
  <!-- @keyframes move {
      0% {
          left: 0;
          transform: translate(0, 0);
      }
      100% {
          left: 100%;
          transform: translate(-100%, 0);
      }
  } -->
```



<br>
<br>
<br>
