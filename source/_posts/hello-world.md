---
title: Hello World
date: 2019-09-11
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
```
 git pull
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
hexo d == hexo deploy # 部署播客到远端
hexo clean # 清除缓存文件
hexo n == hexo new "postName" # 新建文章
hexo n == hexo new page "pageName" # 新建页面
```

### 五、详细文档
[Butterfly主题文档](https://jerryc.me/posts/21cfbf15/#%E5%BF%AB%E9%80%9F%E9%96%8B%E5%A7%8B)

### 六、留言
* 📝
* [Give a ⭐️ if this project helped you!](https://github.com/LOUSANPANG)
* [If you know me please follow me or leave me a message.](https://github.com/LOUSANPANG/lousanpang.github.io/issues)