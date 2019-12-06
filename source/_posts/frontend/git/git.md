---
title: Git
date: 2019-11-18
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

### 二、[提交规范](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

**Commit message 格式**

**Header Body Footer 三部分**
```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

#### 2.1 Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）。

**type**

1. `feat`: 新功能、特性（feature）
2. `fix`: 修补bug、修改问题
3. `docs`: 文档修改 （documentation）
4. `style`: 代码格式修改（不影响代码运行的变动）
5. `refactor`: 重构（既不是添加功能，也不是修改bug的代码变动）
6. `test`: 测试用例修改
7. `chore`: 其他修改、构建过程或辅助工具变动

**scope**

`scope`: 用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

**subject**

1. `subject`: commit 的概述, 建议符合  <50 formatting
2. 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
3. 第一个字母小写
4. 结尾不加句号（.）

**配合`gitmoji表情包更加清晰`**

安装
```
npm i -g gitmoji-cli
```

使用
```
git commit -m 'fix: :memo: 修改bug'
```

emoji | emoji代码 | 说明
------|---------|---
:art: | :art: | 改进代码结构或代码格式
:zap: | :zap: | 提升性能
:fire: | :fire: | 移除代码或文件
:bug: | :bug: | 修复bug
:ambulance: | :ambulance: | 关键补丁
:sparkles: | :sparkles: | 添加新功能
:memo: | :memo: | 写文档
:rocket: | :rocket: | 部署功能
:lipstick: | :lipstick: | 修改UI和样式文件
:tada: | :tada: | 初次提交
:white_check_mark: | :white_check_mark: | 添加测试
:lock: | :lock: | 修复安全问题
:apple: | :apple: | 修复macOS下的问题
:penguin: | :penguin: | 修复Linux下的问题
:checkered_flag: | :checkered_flag: | 修复Windows下的问题
:robot: | :robot: | 修复安卓下的问题
:green_apple: | :green_apple: | 修复IOS下的问题
:bookmark: | :bookmark: | 发行或版本标签
:rotating_light: | :rotating_light: | 移除linter警告
:construction: | :construction: | 工作中
:green_heart: | :green_heart: | 修复CI构建
:arrow_down: | :arrow_down: | 降级依赖
:arrow_up: | :arrow_up: | 升级依赖
:pushpin: | :puahpin: | 将依赖项固定到特定版本
:construction_worker: | :construction_worker: | 添加CI构建系统
:chart_with_upwards_trend: | :chart_with_upwards_trend: | 添加分析或跟踪代码
:recycle: | :recycle: | 重构代码
:whale: | :whale: | 关于Docker的工作
:heavy_plus_sign: | :heavy_plus_sign: | 添加依赖
:heavy_minus_sign: | :heavy_minus_sign: | 移除依赖
:wrench: | :wrench: | 更改配置文件
:globe_with_meridians: | :globe_with_meridians: | 国际化和本土化
:pencil2: | :pencil2: | 修改错别字
:hankey: | :hankey: | 编写需要改进的错误代码
:rewind: | :rewind: | 还原更改
:twisted_rightwards_arrows: | :twisted_rightwards_arrows: | 合并分支
:package: | :package: | 更新编译的文件或包
:alien: | :alien: | 由于外部API更改而更新代码
:truck: | :truck: | 移动或重命名文件
:page_facing_up: | :page_facing_up: | 添加或更新许可证
:boom: | :boom: | 引入重大变更
:bento: | :bento: | 添加或更新assets
:ok_hand: | :ok_hand: | 由于代码审查更改而更新代码
:wheelchair: | :wheelchair: | 改善可访问性
:bulb: | :bulb: | 记录源代码
:beers: | :beers: | 喝多了写的代码
:speech_balloon: | :speech_balloon: | 更新文本和文字
:card_file_box: | :card_file_box: | 执行与数据库相关的更改
:loud_sound: | :loud_sound: | 添加日志
:mute: | :mute: | 删除日志
:busts_in_silhouette: | :busts_in_silhouette: | 添加贡献者
:children_crossing: | :children_crossing: | 改善用户体验/可用性
:building_construction: | :building_construction: | 进行架构更改。
:iphone: | :iphone: | 响应式设计
:clown_face: | :clown_face: | Mocking things（嘲笑？？？）
:egg: | :egg: | 彩蛋
:see_no_evil: | :see_no_evil: | 添加或更新.gitignore文件
:camera_flash: | :camera_flash: | 添加或更新快照
:alembic: | :alembic: | 尝试新玩意儿
:mag: | :mag: | 提升SEO
:wheel_of_dharma: | :wheel_of_dharma: | 关于Kubernetes的工作
:label: | :label: | 添加或更新类型（Flow，Typescript）


#### 2.2 Body(可选)

`body`: commit 具体修改内容, 可以分为多行, 建议符合 50/72 formatting

#### 2.3 Footer(可选)

`footer`: 一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接.


### 三、撰写合格 Commit message 的工具

安装命令如下。
```
npm install -g commitizen
```

然后，在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式。
```
commitizen init cz-conventional-changelog --save --save-exact
```

以后，凡是用到`git commit`命令，一律改为使用`git cz`。这时，就会出现选项，用来生成符合格式的 Commit message。
```
git cz 替代 git commit
```

### 四、生成 Change log

`conventional-changelog` 就是生成 Change log 的工具，运行下面的命令即可
```
npm install -g conventional-changelog
cd my-project
conventional-changelog -p angular -i CHANGELOG.md -w
```

上面命令不会覆盖以前的 Change log，只会在CHANGELOG.md的头部加上自从上次发布以来的变动。
如果你想生成所有发布的 Change log，要改为运行下面的命令。
```
conventional-changelog -p angular -i CHANGELOG.md -w -r 0
```

为了方便使用，可以将其写入`package.json`的`scripts`字段。
```
{
  "scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -r 0"
  }
}
```

```
npm run changelog
```


<br>
<br>
<br>
<br>
<br>