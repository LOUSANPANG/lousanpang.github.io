---
title: Git
date: 2018-7-15
tags:
    - Git
categories: Git
keywords: [Git]
description: Git
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/11/29/QkuLb4.md.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 应用分支流程

自己分支

1. git pull // 更新自己的远程分支
2. git add --a // 将所有新文件至缓存仓库
3. git commit -a -m '' // 提交说明至缓存仓库
4. git push // 提交至自己的远程仓库

更换至本地公共仓库

1. git checkout xxx

公共分支

1. git pull // 将远程公共仓库更新至本地
2. git merge xx // 拉自己的代码至本地公共仓库
3. <!- 解决冲突 ->
4. git add --a // 将所有文件至缓存仓库
5. git commit -a -m '' // 提交说明至缓存仓库
6. git push // 提交至公共远程仓库

更换至自己本地仓库

1. git checkout xx
2. git merge dev // 拉最全的公共代码仓库
3. git add --a
4. git commit -a -m ''
5. git push

### 提交规范

**Commit message 格式**

1. `<type>: <subject>`

**type**

1. `feat`: 新功能、特性（feature）
2. `fix`: 修补bug、修改问题
3. `docs`: 文档修改 （documentation）
4. `style`: 代码格式修改（不影响代码运行的变动）
5. `refactor`: 重构（既不是添加功能，也不是修改bug的代码变动）
6. `test`: 测试用例修改
7. `chore`: 其他修改、构建过程或辅助工具变动
8. `scope`: 影响的范围
9. `subject`: commit 的概述, 建议符合  50/72 formatting
10. `body`: commit 具体修改内容, 可以分为多行, 建议符合 50/72 formatting
11. `footer`: 一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接.

<br>
<br>
<br>
<br>
<br>