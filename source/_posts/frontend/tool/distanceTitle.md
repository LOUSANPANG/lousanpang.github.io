---
title: 距离转换标题缩写
date: 2018-3-20
tags: 
    - 前端工具
categories: 前端工具
keywords: [前端工具]
description: 距离转换标题缩写
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://i.loli.net/2019/08/23/kK4B79VfgmSy5Ne.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

![tool1.png](https://i.loli.net/2019/08/23/kK4B79VfgmSy5Ne.png)

### 一、关于工具
#### 1.1 问题
* 地图展示出来的距离不符合我们展示的要求例如：`0.01km 1.001km`
* 对应地图信息标题展示内容过长例如：`北京市海淀区车道沟1号青东商务区`
#### 1.2 需求
* 我们可能想要的距离 `10m 1.0km`
* 我们想要的标题 `北京市...青东商务区`
#### 1.3 应用
```js
const StrEllipsis = (str, len, starlen = 0, endlen = 0) => {
  if (str.length > len) {
    return str.substr(0, starlen) + "..." + str.substr(str.length - endlen, str.length);
  }
  return str;
};

const StrKm = (juli, fixednum = 3) => {
    if (parseInt(juli) == juli) {
        return juli + 'km'
    } else {
        let left = juli.toString().split(".")[0]
        let leftlast = left[left.length-1]
        if (left.length == 1 && leftlast == 0) {
            return juli.toFixed(fixednum)*(10 ** fixednum) + 'm'
        } else {
            return juli.toFixed(1) + 'km'
        }
    }
}

export {
    StrEllipsis,
    StrKm
}
```
#### 1.4 使用
```js
import {StrEllipsis, StrKm} from ''

StrEllipsis('北京市海淀区车道沟1号青东商务区', 8, 3, 5) // '北京市...青东商务区'
StrEllipsis('北京市海淀区车道沟1号青东商务区', 30) // '北京市海淀区车道沟1号青东商务区'

StrKm(0.01) // 10m
StrKm(1.01) // 1.0km
StrKm(0.123456) // 123m
```

<br>
<br>
<br>
<br>
<br>