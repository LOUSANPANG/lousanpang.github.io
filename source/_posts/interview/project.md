---
title: 前端面试复习计划之项目实战
date: 2023-3-6
tags: 
    - 项目实战
categories: 面试
keywords: [面试]
description: 前端面试复习计划
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2023/03/06/2ae30bde0c33a18f.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## 10种鉴权方法


#### HTTP 基本鉴权
    - 账号密码通过加密的方式调取服务认证
    - 缺点：请求过于暴露会被重放攻击

#### Seesion-Cookie鉴权
    - 利用服务端的 Session（会话）和 浏览器的 Cookie 来实现的前后端通信认证模式
    - 客户端发送请求到服务器，服务器生成 SessionId 存储在 Session 服务器中，并返回客户端并设置 Cookie 存储 SessionId
    - 客户端拿 SessionId 发送请求，服务器校验 Session
    - 缺点：过于依赖 Cookie、移动端对 Cookie 支持不友好、用户量大服务器开销大

#### Token鉴权
    - Token 组成：uid (用户唯一的身份标识) + time (当前时间的时间戳) + sign (签名，Token 的前几位以哈希算法压缩成的一定长度的十六进制字符串)
    - 客户端发送请求，请求通过服务端生成一个加密后的 Token 令牌
    - 客户端存储 Token 至缓存中，下次发送请求将 Token 放至请求头 Authorization 字段
    - 服务端拿到请求令牌后，进行解密和签名校验，如果验证不成功返回 401
    - 为了避免 Token 时效期短，采用 AccessToken 和 RefreshToken，先用 AccessToken 请求，如果过期再用 RefreshToken 请求
    - RefreshToken 过期就重新登录，如果没有过期，服务器返回新的 AccessToken，用于下次请求

#### JWT（JSON WEB TOKEN）鉴权
    - JWT 组成： Header 头部、 Payload 负载 和 Signature 签名
    - 客户端发送请求至服务器，服务器校验通过后使用密钥创建 JWT，发送给客户端
    - 客户端存储到本地，请求时设置请求头 Authorization 为 JWT
    - 服务端拿到 JWT，检验是否过期，解析用户信息，处理相关数据
    - 想比于 Token，降低服务端查询数据库的次数，减少查询用户信息的次数

#### 单点登录 SSO
    - 1同域名下的 SSO，采用 Seesion-Cookie 的方式，附加在响应头的 Set-Cookie 字段中，设置 Cookie 的 Domain 为 .主域.com
    - 客户端发送请求，携带主域名 Domain 下的 Cookie 给服务器，通过 Cookie 来验证登录状态
    - 2不同域名下的 SSO，通过中间方式进行颁布令牌
    - 通过 CAS 认证服务中心，生成 TGC 放入自己的 Session 中，同时以 Set-Cookie 形式写入 Domain 为 sso.com 的域下 ；同时生成一个授权令牌 ST（类似Token）
    - 客户端再发送请求需要拿着 ST 向 CAS 认证服务发送请求，CAS 认证服务验证票据 (ST) 的有效性

#### OAuth 2.0
    - 允许用户授权第三方网站 
    - 授权码模式（微信登录）：客户端跳转第三方网站授权登录换取授权码 - 客户端使用授权码调用服务端获取token - 客户端使用token访问资源
    - 授权码隐藏式：跳过授权码直接返回token - 前端将本地存储的 client_secret 与 token 进行校验
    - 用户名密码式模式：用户在客户端提交账号密码换token - 客户端使用token访问资源
    - 客户端模式：客户端使用自己的标识换token - 客户端使用token访问资源

#### 联合登录和信任登录
    - 联合登录：APP内嵌H5，登录态Token附加在URL参数上或者采用通信协议

#### 唯一登录
    - 客户端A与服务端产生对应的 Token，同时也保存登录状态
    - 客户端B在次登录，检测有登录状态，重新生成Token

#### 扫码登录
    - PC端请求二维码 - 生成二维码UUID、关联UUID和设备信息、设置过期时间 - 前端展示二维码、定时轮询刷新
    - 手机扫码获取UUID - 将设备信息和UUID发送给服务端 - 服务端将ID和用户信息绑定生成临时token - 二维码更新为待确认
    - 手机携带临时token登录 - 服务端将临时token转成真实token - 二维码状态更新为已确认

#### 一键登录
    - SKD初始化，调用 SDK 方法，参数 AppKey 和 AppSecret
    - 唤起授权页，授权页会显示手机号掩码以及运营商协议给用户确认
    - 同意授权并登录，客户端获取到token
    - 携带 token 调用服务一键登录




## 机智应对后端一次性返回10万条数据

1、数据进行分组、分批、分堆
2、使用 `requestAnimationFrame` 代替 定时器
2、使用 **数据分页** 或 **滚动触底加载** 的思维
3、采用虚拟列表
4、启用多线程 `Web Worker` 处理代码逻辑












<br />
<br />
<br />
<br />
