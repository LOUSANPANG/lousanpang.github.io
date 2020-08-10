---
title: HTML5标签语义化你的HTML文档
date: 2019-12-25
tags: 
    - HTML5
categories: HTML5
keywords: [HTML5]
description: HTML5
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/12/25/li1aGT.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

## 一、列举
### 1.1 智能提示
```
<dfn id="abbr">
    <abbr title="这是个abbr标签">鼠标hover时查看详细信息</abbr>
</dfn>
```
<dfn id="abbr">
    <abbr title="这是个abbr标签">鼠标hover时查看详细信息</abbr>
</dfn>

### 1.2 搜索提示
```
<input list="browsers" placeholder="datalist" />
<datalist id="browsers">
    <option value="Chrome"></option>
    <option value="Firefox"></option>
    <option value="Internet Explorer"></option>
    <option value="Opera"></option>
</datalist>
```
<input list="browsers" placeholder="datalist" />
<datalist id="browsers">
    <option value="Chrome"></option>
    <option value="Firefox"></option>
    <option value="Internet Explorer"></option>
    <option value="Opera"></option>
</datalist>

### 1.3 展示更多
```
<details>
    <summary>details summary</summary>
    details元素可创建一个挂件，仅在被切换成展开状态时，它才会显示内含的信息。summary元素可为该部件提供概要或者标签。
</details>
```
<details>
    <summary>details summary</summary>
    details元素可创建一个挂件，仅在被切换成展开状态时，它才会显示内含的信息。summary元素可为该部件提供概要或者标签。
</details>

### 1.4 提示框
```
<dialog open>
    <p>dialog标签 类似提示框 open属性开关</p>
</dialog>
```
<dialog open>
    <p>dialog标签 类似提示框 open属性开关</p>
</dialog>
<br />
<br />
<br />
<br />

### 1.5 字体形态
```
<em>着重阅读元素 em标签</em>

<i>i标签斜体</i>

<small>small标签变小</small>

<strong>strong标签加粗</strong>

<p>sub标签字体更低: H<sub>2</sub>O</p>

<p>下划线 <u>speling</u> mistakes, so the writer can <u>u标签</u> them.</p>
```
<em>着重阅读元素 em标签</em>
<i>i标签斜体</i>
<small>small标签变小</small>
<strong>strong标签加粗</strong>
<p>sub标签字体更低: H<sub>2</sub>O</p>
<p>下划线 <u>speling</u> mistakes, so the writer can <u>u标签</u> them.</p>

### 1.6 地址
```
<address>
    <a href="mailto:1271255653@qq.com">联系邮箱</a>
</address>
```
<address>
    <a href="mailto:1271255653@qq.com">联系邮箱</a>
</address>

### 1.7 独立结构
```
<article>
    <h4>独立结构article</h4>
</article>
<section>section独立部分</section>
```
<article>
    <h4>独立结构article</h4>
</article>
<section>section独立部分</section>

### 1.8 侧边栏
```
<aside>aside独立的一部分</aside>
```
<aside>aside独立的一部分</aside>

### 1.9 呈现计算机代码片段
```
<code>code标签代码片段</code>
<var>var 表示变量标签</var>
```
<code>code标签代码片段</code>
<var>var 表示变量标签</var>

### 2.0 del 删除线
```
<del>del标签下架商品</del>
<s>s标签</s>
```
<del>del标签下架商品</del>
<s>s标签</s>

### 2.1 内容展示
```
<header>
    <main>main标签的内容<em>这是em标签</em></main>
    <p>time 标签<time>20:00</time>.</p>
    <progress value="40" max="100"></progress>
    <mark>mark标签高亮</mark>
</header>
<nav>nav标签</nav>
<footer>
    我是footer一个底部标签
</footer>
```
<header>
    <main>main标签的内容<em>这是em标签</em></main>
    <p>time 标签<time>20:00</time>.</p>
    <progress value="40" max="100"></progress>
    <mark>mark标签高亮</mark>
</header>
<nav>nav标签</nav>
<footer>
    我是footer一个底部标签
</footer>

### 2.2 图片上的字体
```
<figure>
    <img
    src="https://interactive-examples.mdn.mozilla.net/media/examples/elephant-660-480.jpg"
    />
    <figcaption>figure figcaption 组合</figcaption>
</figure>
```
<figure>
    <img
    src="https://interactive-examples.mdn.mozilla.net/media/examples/elephant-660-480.jpg"
    />
    <figcaption>figure figcaption 组合</figcaption>
</figure>

### 2.3 iframe
```
<iframe
    src="https://www.baidu.com"
    width="100"
    height="100"
    frameborder="10"
></iframe>
```
<iframe
    src="https://www.baidu.com"
    width="100"
    height="100"
    frameborder="10"
></iframe>

### 2.4 ol
```
<ol type="a">
    <li value="1">third item</li>
    <li>fourth item</li>
    <li>fifth item</li>
</ol>
```
<ol type="a">
    <li value="1">third item</li>
    <li>fourth item</li>
    <li>fifth item</li>
</ol>

### 2.5 link
```
<link href="favicon.ico" rel="icon" />
```
<link href="favicon.ico" rel="icon" />

### 2.5 select下的分界线
```
<select name="" id="">
    <optgroup label="我是optgroup">
        <option>111111</option>
        <option selected>222222</option>
    </optgroup>
</select>
```
<select name="" id="">
    <optgroup label="我是optgroup">
    <option>111111</option>
    <option selected>222222</option>
    </optgroup>
</select>


### 2.5 输出
```
<form oninput="result.value=parseInt(a.value)+parseInt(b.value)">
    <input type="range" name="b" value="50" /> +
    <input type="number" name="a" value="10" /> =
    <output name="result"></output>
</form>
```
<form oninput="result.value=parseInt(a.value)+parseInt(b.value)">
    <input type="range" name="b" value="50" /> +
    <input type="number" name="a" value="10" /> =
    <output name="result"></output>
</form>


### 2.5 带有格式的内容
```
<pre>
    L          TE
    A       A
        C    V
        R A
        DOU
        LOU
        REUSE
        QUE TU
        PORTES
    ET QUI T'
    ORNE O CI
        VILISÉ
    OTE-  TU VEUX
        LA    BIEN
    SI      RESPI
            RER       - pre标签
</pre>
```
<pre>
    L          TE
    A       A
        C    V
        R A
        DOU
        LOU
        REUSE
        QUE TU
        PORTES
    ET QUI T'
    ORNE O CI
        VILISÉ
    OTE-  TU VEUX
        LA    BIEN
    SI      RESPI
            RER       - pre标签
</pre>

### 2.5 拼音
```
<ruby>
    <rb>旧</rb>
    <rb>金</rb>
    <rb>山</rb>
    <rt>jiù</rt>
    <rt>jīn</rt>
    <rt>shān</rt>
    <rtc>San Francisco</rtc>
</ruby>
```
<ruby>
    <rb>旧</rb>
    <rb>金</rb>
    <rb>山</rb>
    <rt>jiù</rt>
    <rt>jīn</rt>
    <rt>shān</rt>
    <rtc>San Francisco</rtc>
</ruby>

### 2.5 trach 字幕



<br>
<br>
<br>
<br>
<br>