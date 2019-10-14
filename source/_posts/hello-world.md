---
title: Hello World
date: 2018-03-13
tags: 
    - 关于写作
categories: 关于写作
keywords: [写作]
description: Hello World
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://i.loli.net/2019/09/11/sR9xEpuAhTYcFiw.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

<!-- ![pexels-photo-2294878.jpeg](https://i.loli.net/2019/09/11/sR9xEpuAhTYcFiw.jpg) -->

### 一、命令
#### 1.1 安装前提
* [Node.js (Should be at least nodejs 6.9)](https://nodejs.org/en/)
* [Git](https://git-scm.com/)
* `npm install -g hexo-cli`

#### 1.2 相关依赖
```
    "hexo": "^3.9.0",
    "hexo-deployer-git": "^2.0.0",
    "hexo-generator-archive": "^0.1.5",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-index": "^0.2.1",
    "hexo-generator-search": "^2.4.0",
    "hexo-generator-searchdb": "^1.0.8",
    "hexo-generator-tag": "^0.2.0",
    "hexo-renderer-ejs": "^0.3.1",
    "hexo-renderer-jade": "^0.4.1",
    "hexo-renderer-marked": "^1.0.1",
    "hexo-renderer-stylus": "^0.3.3",
    "hexo-server": "^0.3.3"
```

#### 1.3 安装依赖
```
  cd lousanpang.github.io

  git checkout lgh

  npm install
```

### 二、 编码
#### 2.1 新建文档
```
    cd source/_posts
    cd frontend/ #在对应适合你的文件夹下边建立 博客文档
    hexo n [Your Blod Name]
```
#### 2.2 书写
```
 ---
 title:
 date:
 categories: # 分类
 tags: # 标签
 keywords: # 关键词
 description: # 描述
 top_img: # 除非特定需要，可以不写
 comments:  # 是否显示评论 除非设置false,可以不写
 cover: # 缩略图
 toc: # 章节目录 除非特定文章设置，可以不写
 toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
 copyright: # 是否显示版权 除非特定文章设置，可以不写
 ---
 <!-- more content -->
```

### 三、 部署
#### 3.1 建立远程分支 `lgh` 存放源码
```
rm -rf .git # 主题目录清除clone主题的git文件
git init # 主目录
git add --a
git commit -a -m ''
git branch lgh
git checkout lgh
git remote add origin https://github.com/LOUSANPANG/lousanpang.github.io.git # 建立连接
git push origin test # 推送分支
```
#### 3.2 `master` 分支存放转化代码
```
deploy: # 主目录的_config.yml
  type: git
  repository: https://github.com/LOUSANPANG/lousanpang.github.io.git
  branch: master
```
#### 3.3 日常提交代码
```
 git pull
 hexo clean
 hexo g
 hexo d
 git add --a
 git commit -a -m ''
 git push --set-upstream origin lgh
```

### 四、[常见命令](https://hexo.io/zh-cn/docs/commands)
```
hexo g == hexo generate # 生成静态文件
hexo s == hexo server # 启动本地web服务
hexo d == hexo deploy # 部署播客到远端 安装hexo-deployer-git依赖
hexo clean # 清除缓存文件
hexo new "postName" # 新建文章
hexo new page "pageName" # 新建章节
```

### 五丶 常见问题
#### 5.1 Hexo 主题无法上传到GitHub
```
git ls-files --stage | grep 160000
git rm --cached themes/Butterfly
```
原因如下：
这是因为用到了git的子模块（git submodule）功能（你在你的git项目里clone的别人的项目）
在你的主项目的git库里，子模块只是一个HEAD指针，指向子模块的commit
所以你需要清除一下暂存区

说一下这个功能的意义：
在这里，如果你需要修改Butterfly主题（可能需要很多文件），又想保证能够随时更新最新版本，其实用子模块功能是很方便的。
只需要clone下来新建一个branch，用来自己用，每次官方更新pull到另一个分支，merge一下就行。
相当于把一个大项目分成多个小项目，尽可能减少项目之间的关联，方便调试和修改


### 六、详细文档
[Butterfly主题文档](https://jerryc.me/posts/21cfbf15/#%E5%BF%AB%E9%80%9F%E9%96%8B%E5%A7%8B)

### 七、留言
* 📝
* [Give a ⭐️ if this project helped you!](https://github.com/LOUSANPANG)
* [If you know me please follow me or leave me a message.](https://github.com/LOUSANPANG/lousanpang.github.io/issues)





<br>
<br>
<br>
<br>
<br>