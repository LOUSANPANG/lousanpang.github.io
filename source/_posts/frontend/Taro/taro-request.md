---
title: component-taro-request
date: 2019-10-12
tags: 
    - Taro
categories: Taro
keywords: [Taro]
description: component-taro-request
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/10/12/uLqqUK.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

![pexels-photo-2294878.jpeg](https://s2.ax1x.com/2019/10/12/uLqqUK.png)
<br>


### [Taro🔨] 封装 Taro.request、statusCodes 的请求工具。
<br>


### 一、Taro官方
详细请查阅官方 [Taro.request(OBJECT)](https://nervjs.github.io/taro/docs/apis/network/request/request.html) 项目。
<br>


### 二、[GitHub 下载地址](https://github.com/LOUSANPANG/component-taro-request)
<br>


### 三、文档使用

| 状态码 | promise简易版的Taro.request | promise简易版的Taro.showToast |
| ------ | --------------------------- | ----------------------------- |
| ✅      | ✅                           | ✅                             |

* 下载该文件，将文件内的 `fetch` `statusCode` 拷入到`utils`项目中。
* 在对应页面组件引入、定义工具配置参数即可。
* 应用
```
// index.jsx

import {fetch, showToast} from '../../utils/fetch'

fetch(url, {})
    .then(res => {
        // res.data 开发者服务器返回的数据
        // 如果状态码不为200，会调用工具中的showToast(statusCode(res.statusCode, 2000))
    })

```
<br>

#### fetch配置参数

| 属性名       | 说明                                                                               | 必选 |
| ------------ | ---------------------------------------------------------------------------------- | ---- |
| url          | 请求服务的路径，例如：`https://jsonplaceholder.typicode.com/users`,只需填写`users` | 是   |
| data         | 参数，默认`{}`                                                                     | 否   |
| contentType  | 设置请求的 header, 默认`application/json`                                          | 否   |
| method       | HTTP 请求方法 , 默认`GET`                                                          | 否   |
| responseType | 响应的数据类型 , 默认`text`                                                        | 否   |
<br>


#### showToast配置参数

| 属性名   | 说明                 | 必选 |
| -------- | -------------------- | ---- |
| title    | 提示框内容           | 是   |
| icon     | 图标，默认`none`     | 否   |
| duration | 提示事件, 默认`1500` | 否   |
<br>


#### statusCode配置参数
| 属性名 | 说明                   | 必选 |
| ------ | ---------------------- | ---- |
| code   | 服务返回的 Http 状态码 | 是   |
<br>

### 三、关于状态码、showToast
Taro的 `.catch` 中无法定位具体错误原因，但可以在 `.then` 中获取状态码。

将`Taro.request`封装一层,再将状态码封装进去。

```
    const showToast = (title, icon = 'none', duration = 1500) => {
        return new Promise((resolve, reject) => {
            Taro.showToast({
               //...
            })
        })
    }

    const statusCode = code => {
        const codes = {
           //...
        }
        return codes[code]
    }

    const fetch = (url, data = {}, contentType = 'application/json', method = 'GET', responseType = 'text') => {
        return new Promise(resolve => {
            Taro.request({
               // ...
            }).then(res => {
                resolve(res)

                if(res.statusCode !== 200) {
                    showToast(statusCode(res.statusCode, 2000))
                }
            })
        })
    }
```