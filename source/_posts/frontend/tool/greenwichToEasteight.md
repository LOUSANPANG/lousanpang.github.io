---
title: 格林威治转东八区格式工具
date: 2018-4-10
tags: 
    - 前端工具
categories: 前端工具
keywords: [前端工具]
description: 格林威治转东八区格式工具
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://i.loli.net/2019/09/24/z9G3wp5Zatj2BFl.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

![格林威治转东八区格式工具.png](https://i.loli.net/2019/09/24/YJTRXIp4he8qm6V.png)

### 一、关于工具
#### 1.1 问题
* 中国的标准时间为格林威治时间 + 8
* 转换后的格式问题
#### 1.2 需求
* 我们可能想要的转化格式 `2017-08-27 22:40:21` `2017/08/27 22:40`
#### 1.3 应用code
```js
/**
 * 格林威治时间转东八区
 * newdata 需要转化的格林威治时间，例如"2019-09-24T00:41:41.201Z"
 * format 转化后的格式，默认"yyyy-MM-dd hh:mm:ss"
*/

const GreenwichToEasteight = (newdata, format = "yyyy/MM/dd hh:mm") => {
  let that = new Date(newdata);
  let o = {
    "M+": that.getMonth() + 1, //月份
    "d+": that.getDate(), //日
    "h+": that.getHours(), //小时
    "m+": that.getMinutes(), //分
    "s+": that.getSeconds(), //秒
    "q+": Math.floor((that.getMonth() + 3) / 3), //季度
    S: that.getMilliseconds()
  };
  if (/(y+)/.test(format))
    format = format.replace(
      RegExp.$1,
      (that.getFullYear() + "").substr(4 - RegExp.$1.length)
    );
  for (let k in o)
    if (new RegExp("(" + k + ")").test(format))
      format = format.replace(
        RegExp.$1,
        RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length)
      );
  return format;
};

export default GreenwichToEasteight;
```
#### 1.4 使用
```js
import GreenwichToEasteight from "";

GreenwichToEasteight("2019-09-24T00:41:41.201Z") // "2019-09-24 08:41:41"
GreenwichToEasteight("2019-09-24T00:41:41.201Z", "yyyy/MM/dd hh:mm") // "2019/09/24 08:41"
```

<br>
<br>
<br>
<br>
<br>