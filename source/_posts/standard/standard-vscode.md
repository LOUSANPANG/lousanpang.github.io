---
title: 开发规范之VScode
date: 2018-05-21
tags:
    - 开发规范
categories: 开发规范
keywords: [开发规范]
description: 开发规范之VScode
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://i.loli.net/2019/11/19/1qQ5UGn2P3RA6NW.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 一、 关于彻底删除`vscode`
#### 1.1 c/{用户}/{用户}/.vscode 这是`vscode`放置安装的插件

#### 1.2 c/{用户}/{用户}/AppData/Roaming/Code 这是`vscode`放置用户信息和缓存信息


### 二、 快速生成代码
后代 >
```
div>span>a

<div>
    <span>
        <a href=""></a>
    </span>
</div>
```

兄弟 +
```
div+p+span

<div></div>
<p></p>
<span></span>
```

上级 ^
```
div>span^i

<div>
    <span></span>
</div>
<i></i>
```

乘法 *
```
ul>li*2

<ul>
    <li></li>
    <li></li>
</ul>
```

文本 {}
```
div>span{this is vv's test}

<div>
    <span>this is vv's test</span>
</div>
```

自增符 $
```
ul>li.list_${list $}*3

<ul>
    <li class="list_1">list 1</li>
    <li class="list_2">list 2</li>
    <li class="list_3">list 3</li>
</ul>
```

@3 表示从3开始计数
```
ul>li.item$@3*3 

<ul>
    <li class="item3">list 1</li>
    <li class="item4">list 2</li>
    <li class="item5">list 3</li>
</ul>
```


### 三、在根目录新建`.vscode`文件夹
#### 3.1 /.vscode/launch.json
```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
  {
    "type": "node",
    "request": "launch",
    "name": "CLI debug",
    "program": "${workspaceFolder}/packages/taro-cli/bin/taro",
    // "cwd": "${project absolute path}",
    // "args": [
    //   "build",
    //   "--type",
    //   "weapp",
    //   "--watch"
    // ],
    "skipFiles": [
      "<node_internals>/**"
    ]
  },
    {
      "type": "node",
      "name": "vscode-jest-tests",
      "request": "launch",
      "args": [
        "--runInBand"
      ],
      "cwd": "${workspaceFolder}",
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen",
      "disableOptimisticBPs": true,
      "program": "${workspaceFolder}/node_modules/jest/bin/jest"
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/index.js",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": [
        "${workspaceFolder}/lib/**/*.js"
      ]
    },
    {
      "type": "node",
      "request": "launch",
      "name": "E2E Test Current File",
      "cwd": "${workspaceFolder}",
      "program": "${workspaceFolder}/node_modules/.bin/stencil",
      "args": ["test", "--e2e", "${relativeFile}"],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen",
      "disableOptimisticBPs": true
    },
  ]
}
```

#### 3.2 /.vscode/settings.json
```json
{
    "workbench.colorTheme": "Atom One Dark",
    "editor.suggestSelection": "first",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "workbench.iconTheme": "vscode-icons",
    "vsicons.dontShowNewVersionMessage": true,
    "editor.tabSize": 2,
    "editor.fontWeight": "normal",
    "editor.lineHeight": 24,
    "diffEditor.ignoreTrimWhitespace": false,
    "terminal.integrated.shell.windows": "D:\\Git\\Git\\bin\\bash.exe",
    "window.zoomLevel": 0,
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[jsonc]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[html]": {
        "editor.defaultFormatter": "vscode.html-language-features"
    },
    "[json]": {
        "editor.defaultFormatter": "vscode.json-language-features"
    },
    "[vue]": {
        "editor.defaultFormatter": "octref.vetur"
    },
    "explorer.confirmDragAndDrop": false,
    "launch": {
        "configurations": [],
        "compounds": []
    },
    "todo-tree.tree.showScanModeButton": false
}
```


### 四、终端`code .`快速启动vscode
#### 4.1 `command` + `shift` + `p` 打开配置命令行
#### 4.2 输入`shell command` 点击安装



<br>
<br>
<br>
<br>
<br>