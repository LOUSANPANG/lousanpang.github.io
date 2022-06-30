---
title: Git高级语法
date: 2022-6-15
tags:
    - Git
categories: Git
keywords: [Git]
description: Git高级语法
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/11/29/QkuLb4.md.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### git-scm.com book

### 一、配置命令
```
查看所有的配置及所在文件夹
git config --list --show-origin

打开全局配置文件
git config --global --edit

配置commit编辑器
git config --global core.editor vim

删除git全局配置
git config --unset core.hooksPath

配置命令别名
git config --global alias.ci commit
// git ci === git commit 
```


### 二、Git基础
#### git status -s 状态简览
```
?? (untracked files；未追踪；需要git add)
A（已追踪；）
M（changes not staged for commit；已追踪未暂存；需要git commit）
MM（已追踪已暂存又再次修改；需要git commit;MM->M）
```

#### 高级基础常用
```
撤销移除暂存清单
git rm --cached xxx

重命名文件(无需手动变更文件名称)
git mv a.js b.js
    => mv a.js b.js
    => git rm a.js
    => git add b.js

查看提交记录
git log

查看提交记录显示补丁输出
git log -p

查看近2条提交记录显示补丁简约版
git log --stat -2

暂存后发现漏掉一个文件
git commit --amend
    => git add reademe.md
    => git commit -m 'amend1'
    => git add newfile.md
    => git commit --amend // newfile.md reademe.md具有一样的暂存commit

取消暂存文件
git reset HEAD reademe.md

远程分支更新 == git pull
git fetch origin + git merge FETCH_HEAD
远程分支拉去到新的本地分支
git fetch origin main:dev

查看远程仓库
git remote -v
删除本地分支
git branch -d main
删除远程分支
git remote remove main
重命名远程分支
git remote rename main master

推送到远程仓库
git push origin main
关联远程仓库
git remote add origin url.git
发布分支并推送到远程仓库
git push --set-upstream origin main

列出标签
git tag -l
查询标签
git tag -l 'v1.0.0*'
附注标签
git tag -a v1.0.0 -m 'version 1.0.0'
展示标签信息
git show v1.0.0
轻量标签
git tag v1.0.0
推送tag
git push origin --tags
删除本地tag
git tag -d v1.0.0
删除远程分支tag
git push origin --delete v1.0.0
```


#### 分支
```
创建并切换分支
git checkout -b dev

可视化合并工具
git mergetool

查看每一个分支的最后一次提交
git branch -v

变基（改变新分支基底）
git rebase main
```