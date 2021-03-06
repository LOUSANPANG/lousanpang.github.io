---
title: Git规范
date: 2019-11-01
tags:
    - Git
categories: Git
keywords: [Git]
description: Git规范
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/11/29/QkuLb4.md.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


### 一、[部署Angular 团队Git的规范](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

#### 1.1 Commitizen: 替代你的 git commit
```
yarn global add commitizen
yarn add -D commitizen cz-conventional-changelog

echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

#### 1.2 package.json中配置
```
"script": {
    "commit": "git-cz",
},
"config": {
  "commitizen": {
    "path": "node_modules/cz-conventional-changelog"
  }
}
```

#### 1.3 自动生成CHANGELOG
```
yarn add -D standard-version
```

```
"script": {
  "release": "standard-version"
}
```

#### 1.4 配置commitlint
```bash
yarn add husky @commitlint/config-conventional @commitlint/cli -D
```
``` js
// package,json
"husky": {
  "hooks": {
    "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
  }
}

// .commitlintrc.js
module.exports = {
    extends: ['@commitlint/config-conventional']
}
```

#### 1.5 试运行
- `git add --a`
- `git cz`
- `npm run release` 一步步升级
- `npm run release -- --release-as x.0.0` 升级到指定版本
- `git push master --tags` || `git push --follow-tags origin master`


### 二、`git cz` 介绍
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

#### 2.2 Provide a longer description of the change 提供更改的详细说明
#### 2.3 Are there any breaking changes? 有重大变化吗
#### 2.4 Does this change affect any open issues 此更改是否影响任何未解决的问题



### 三、[配合gitmoji表情包更加清晰](https://gitmoji.carloscuesta.me/)
#### 3.1 安装
```
npm i -g gitmoji-cli
```

#### 3.2 使用
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



### 四、应用分支流程

#### 4.1 自己分支
1. git pull // 更新自己的远程分支
2. git add --a // 将所有新文件至缓存仓库
3. git commit -a -m '' // 提交说明至缓存仓库
4. git push // 提交至自己的远程仓库

#### 4.2 更换至本地公共仓库
1. git checkout xxx

#### 4.3 公共分支
1. git pull // 将远程公共仓库更新至本地
2. git merge xx // 拉自己的代码至本地公共仓库
3. <!- 解决冲突 ->
4. git add --a // 将所有文件至缓存仓库
5. git commit -a -m '' // 提交说明至缓存仓库
6. git push // 提交至公共远程仓库

#### 4.4 更换至自己本地仓库
1. git checkout xx
2. git merge dev // 拉最全的公共代码仓库
3. git add --a
4. git commit -a -m ''
5. git push


#### 4.5 关于版本回退
1. git reflog 查找提交记录找到版本号
```
c920c31 HEAD@{4}: commit: fix unit test bug
c43a105 HEAD@{5}: commit: proxy request param value
d2d8134 HEAD@{6}: commit: add proxy http request address
aca04dd HEAD@{7}: commit: add unit test
f6f9b44 HEAD@{8}: commit: disable eureka in test env
c7ddf92 HEAD@{9}: pull: Merge made by the 'recursive' strategy.
f96380b HEAD@{10}: checkout: moving from proxyOpt to test
```

2. git reset --hard c920c31 通过gitreset命令回退到c920c31版本

3. git push origin test --force 强制将分区内容推送到远程服务器


### 五、子模块工程
工程项目下包含了子工程项目

#### 5.1 新建`.gitmodules`
```json
[submodule "src/geo-data-engine-service"]
	path = src/geo-data-engine-service
	# url = git@172.17.xxxx:xxxxxx.git // ip
	# url = https:xxxxx.git // 或者域名
```

#### 5.2 clone该子工程
```bash
git submodule add http://xxxxx.git src/xxxx

git submodule init

git submodule update // 根据gitmodule下的路径clone到src下
或者
git submodule update src/xxxx
```

#### 5.3 如何更新该子工程
- 在项目中切换到子模块工程，切换到master分支，git pull。
- 在子模块工程中提交代码
- 每次子模块工程有更新，总项目就会有变化


#### 5.4 如何彻底删除掉子模块工程
- 删除版本控制
```bash
git rm -r modules/subb
```

- 删除`.gitmodules`中的代码
```bash
 [submodule "modules/subb"]
          path = modules/subb
          url = https://xxxx.git
          branch = dev
```
- `.git/config` 删除代码
```bash
 [submodule "modules/subb"]
         url = https://xxxx.git
         active = true
```
- 删除`.git`下的缓存模块
```bash
rm -rf .git/modules/subb
```