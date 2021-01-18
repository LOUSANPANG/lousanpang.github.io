---
title: 开发规范之GitHub
date: 2020-12-11
tags:
    - 开发规范
categories: 开发规范
keywords: [开发规范]
description: 开发规范之GitHub
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2020/12/11/rA0RsK.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

## 一、最优美的README文档


## 二、醒目的版本小图标


## 三、规范的提交日志、未来规划


## 四、简便的issues模板
### 4.1 在指定`repositories`的根目录下创建新目录`.github`
```
- .github
- node_modules
- src
- ...
```

### 4.2 创建`ISSUES`模板文件夹`ISSUE_TEMPLATE`
```
- .github
    - ISSUE_TEMPLATE
- node_modules
- src
- ...
```

### 4.3 在新建的`.github/ISSUE_TEMPLATE`下添加多个md文件`ISSUE_TEMPLATE_1.md`和`ISSUE_TEMPLATE_2.md`。当创建`issue`时会让开发进行选择。
```
- .github
    - ISSUE_TEMPLATE
        - ISSUE_TEMPLATE_1.md
        - ISSUE_TEMPLATE_2.md
- node_modules
- src
- ...
```

### 4.4 多模板的每个md文件的头部需要加入以下内容
```
---
name: 模版名称（创建issue时，供选择的模版列表中将显示该名称）
about: 模版的相关描述（创建 issue 时，供选择的模版列表将显示该描述）
---
(以下是模板正文内容)
```

### 4.5 示例
```
---
name: 模版名称（创建issue时，供选择的模版列表中将显示该名称）
about: 模版的相关描述（创建 issue 时，供选择的模版列表将显示该描述）
---
## Issue 标题
### 版本
### 重现链接
### 重现步骤
1. ...
2. ...
3. ...
### 期望的结果是什么？
### 实际的结果是什么？
### 环境信息
### 补充说明（可选）
```


## 五、更好PR


<br>
<br>
<br>
<br>
<br>