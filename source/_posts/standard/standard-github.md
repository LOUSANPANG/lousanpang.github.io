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


## 四、[简便的issues模板](https://help.github.com/articles/manually-creating-a-single-issue-template-for-your-repository/)
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
### 5.1 创建 `pull request` 模板

#### 默认模版
- 在代码库新建目录：`.github`
- 在 `.github` 目录下添加 `PULL_REQUEST_TEMPLATE.md` 文件作为 `pull request` 默认模版。当创建不带参数的 `pull request` 时，系统会引用该模版。

#### 多模版
- 在代码库新建目录：`.github/PULL_REQUEST_TEMPLATE`
- 该目录下可添加多个 `.md` 文件作为 `pull request` 模版。
- `pull request` 模版要通过查询参数来调用。例如，要使用 `pr-template-1.md` 这个模版，可使用如下查询：
```
https://github.com/用户名/代码库名称/compare/分支名称?expand=1&template=pr-template-1.md

或参考GitHub帮助文档的格式，如下。两者效果相同。

https://github.com/用户名/代码库名称/compare/master...分支名称?expand=1&template=pr-template-1.md
```
- 可选查询参数
    - `expand=1`，直接跳转到 `pull request` 界面。如果不带此参数会先到 `compare` 界面，需手动进入 `pull request` 界面。
    - `template=pr-template-1.md`，调用名为 `pr-template-1.md` 的模版。如果不带此参数，则调用默认模版。
    - `title=New+bug+report`（或者 `title=New%20bug%20report`），指定 `pull request` 的标题为 `New bug report`。
    - 其他参数可详见帮助文档：https://help.github.com/articles/about-automation-for-issues-and-pull-requests-with-query-parameters/



    



<br>
<br>
<br>
<br>
<br>