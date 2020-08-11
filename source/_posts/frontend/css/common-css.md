---
title: 项目总结-css记录篇
date: 2019-05-01
tags: 
    - Css
    - 项目总结
categories: 项目总结
keywords: [Css]
description: Css
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








### 三、常用的css样式 - `box-shadow`




### 四、常用的css样式 - `border border-radius`




### 五、常用的css样式 - `color background font`



### 六、404页面示例




<br>
<br>
<br>
