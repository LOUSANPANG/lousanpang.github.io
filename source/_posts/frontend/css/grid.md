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

### 六、grid-auto-flow 容器元素顺序排列
#### 6.1 默认 `row` 排列, 下边是纵向开始排列
```
grid-auto-flow: column;
```

#### 6.2 row dense / column dense 插缝，紧密填满
先行后列
```
grid-auto-flow: row dense;
```
![MJ74HK.png](https://s2.ax1x.com/2019/11/13/MJ74HK.png)

先列后行
```
grid-auto-flow: column dense;
```
![columndense.png](https://i.loli.net/2019/11/13/EHkeIhJDsjqpYUP.png)


### 七、单元格内容位置
```
start：对齐单元格的起始边缘。
end：对齐单元格的结束边缘。
center：单元格内部居中。
stretch：拉伸，占满单元格的整个宽度（默认值）。
```
#### 7.1 justify-items 单元格内容的水平位置
```
.container {
  justify-items: start | end | center | stretch;
}
```
![MJH5Mn.png](https://s2.ax1x.com/2019/11/13/MJH5Mn.png)

#### 7.2 align-items 单元格内容的垂直位置
```
.container {
  align-items: start | end | center | stretch;
}
```
![MJHbIU.png](https://s2.ax1x.com/2019/11/13/MJHbIU.png)


#### 7.3 place-items 合并 justify-items align-items
```
place-items: <align-items> <justify-items>;

place-items: start end;
```

### 八、作用在项目上
```
space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。
```
#### 8.1 justify-content align-content 项目在容器的水平、垂直位置
```
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

#### 8.2 place-content 合并 
```
place-content: space-around space-evenly;
```






<br>
<br>
<br>
<br>
<br>