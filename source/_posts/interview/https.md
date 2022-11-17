---
title: 前端面试复习计划之网络
date: 2020-10-1
tags: 
    - 网络
categories: 面试
keywords: [面试]
description: 前端面试复习计划
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2022/11/02/aa3441b06b937568.jpeg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## HTTP 工作原理

HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。客户端向服务器发送一个请求报文，服务器以一个状态行作为响应。


## HTTP请求/响应步骤

- 客户端连接到Web服务器
- 发送HTTP请求
- 服务器接受请求并返回HTTP响应
- 释放TCP连接
- 客户端解析HTML内容


## GET与POST区别

- 安全性：GET参数通过URL传递会暴露、不安全，而POST放在RequestBody中，相对更安全
- 针对数据操作的类型不同：GET对数据进行查询，POST主要对数据进行增删改，GET是读，POST是改
- 参数大小不同：GET请求在URL中传达的参数是有长度限制，而POST没有限制
- 浏览器回退表现不同：GET在浏览器回退时是无害的，而POST会再次提交请求
- 浏览器对请求地址的处理不同：GET请求地址会被浏览器主动缓存，而POST不会，除非手动设置
- 浏览器对响应的处理不同：GET请求参数会被完整的保留在历览器历史记录里，而POST中的参数不会被保留


## HTTP报文的组成成分

- Request Header 请求报文
    - 请求行
        - http方法
        - 页面地址
        - http协议
        - http版本
    - 请求头
    - 空行
    - 请求体
- Response Header 响应报文 
    - 状态行
    - 响应头
    - 空行
    - 响应体

```bash
Request Header:
    1 GET /sample.Jsp HTTP/1. # 请求行

Response Header:
```




<br />
<br />
<br />
<br />