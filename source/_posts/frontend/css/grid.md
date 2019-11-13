---
title: Grid网格布局
date: 2018-5-13
tags: 
    - Grid网格布局
categories: Css
keywords: [Css]
description: Css
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://i.loli.net/2019/11/13/RxAitfqmkL2uV7D.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 一、比Flex布局更强大的网格布局(Grid)
[练习网格库](https://cssgrid-generator.netlify.com/)
[资料库](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

### 二、失效
```
注意，设为网格布局以后，
容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align和column-*等设置都将失效。
```

### 三、指定行列
```
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```
可以写成百分比：
```
.container {
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
}
```

#### 3.1 repeat() 
第一个参数重复的次数，第二个参数重复的值或者是重复的模式
```
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}

grid-template-columns: repeat(2, 100px 20px 80px); // 100 20 80 100 20 80
```

#### 3.2 auto-fill 自动填充
容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用:
```
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

#### 3.3 fr 比例关系
两列的宽度分别为1fr和2fr，就表示后者是前者的两倍。
```
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

#### 3.4 minmax() 长度范围（最小值，最大值）
```
表示列宽不小于100px，不大于1fr
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

#### 3.5 auto 由浏览器自己决定长度
```
grid-template-columns: 100px auto 100px;
```

### 四、间距
#### 4.1 grid-column-gap grid-row-gap 行列间距
```
.container {
  grid-row-gap: 20px; // 行间距
  grid-column-gap: 20px; // 列间距
}
```

#### 4.2 grid-gap: <grid-row-gap> <grid-column-gap>;
```
.container {
  grid-gap: 20px 20px;
}
```

### 五、grid-template-areas 区域合并单元
```
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}

grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';

```












<br>
<br>
<br>
<br>
<br>