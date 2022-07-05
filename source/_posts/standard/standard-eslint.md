---
title: 开发规范之ESlint+Prettier
date: 2018-07-05
tags: 
    - 开发规范
categories: 开发规范
keywords: [开发规范]
description: 开发规范之ESlint+Prettier
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s1.ax1x.com/2020/08/17/dZBgUJ.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


### 一、ESLint

#### 1.1 安装
```bash
pnpm add eslint -D

// 借助webpack
// 安装eslint-loader 在webpack中解析
// 安装babel-eslint  对Babel解析器的包装与ESLint兼容
pnpm add eslint-loader babel-eslint -D
```

#### 1.2 配置webpack
```bash
// webpack.config.js

module.exports = {
    entry: '',
    output: {},
    module: {
        rules: [
            {
                test: /\.js$/,
                use: ['babel-loader', 'eslint-loader']
            }
        ]
    },
    plugins: [],
    devServer: {}
};
```

#### 1.3 运行初始化eslint配置文件
```bash
npx eslint --init
```

#### 1.4 vscode配置保存自动格式化
```bash
// 1 安装 eslint vscode插件
// 2 创建 .vscode/settings.json 文件

{
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```


### 二、Prettier

#### 2.1 安装
```bash
pnpm add prettier -D
```

#### 2.2 解决ESlint与Prettier的冲突
```bash
// 关掉与Prettier产生冲突的ESlint格式相关配置
pnpm add eslint-config-prettier -D

// 然后在.eslintrc.js加入perttier扩展
extends: ['airbnb-base', 'prettier'] // 覆盖eslint格式配置,写在最后
```

#### 2.3 Prettier的修复通过ESlint来体现
```bash
pnpm add eslint-plugin-prettier -D

// 修改eslint配置拓展
extends: ['airbnb-base', 'plugin:prettier/recommended'] // 覆盖eslint格式配置,写在最后
```


### 三、关于ESLint、Prettier的配置文件

#### 3.1 .eslintrc.js
```js
module.exports = {
    // 解析ES6
    'parser': 'babel-eslint',

    // 指定解析器选项
    'parserOptions': {
        // 启用ES8语法支持
        'ecmaVersion': 2017,    
        // module表示ECMAScript模块
        'sourceType': 'module',
        // 使用额外的语言特性
        'ecmaFeatures': {
            'experimentalObjectRestSpread': true,
            'jsx': true,
            'modules': true,
        }
    },

    // 指定脚本的运行环境，这些环境并不是互斥的，所以你可以同时定义多个
    'env': {
        'browser': true,
        'jquery': true,
        'node': true,
        'commonjs': true,
        'es6': true,
    },

    // 别人可以直接使用你配置好的ESLint
    'root': true,

    // 脚本在执行期间访问的额外的全局变量
    'globals': {
        'CONFIG': true
    },

    // 启用的规则及其各自的错误级别
    // 每个规则对应的0，1，2分别表示off, warning, error三个错误级别
    'rules': {
        // this 的别名规则，只允许 self 或 that
        'consistent-this': [2, 'self', 'that'],
        // 文件最后必须有空行
        'eol-last': 0,
        // 被prettier标记的地方抛出错误信息
        'prettier/prettier': [
            'error',
            {}
        ],
        'generator-star-spacing': 'off',
        "no-console": process.env.NODE_ENV === "production" ? "error" : "off",
        'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
        'no-alert': process.env.NODE_ENV === 'production' ? 'error' : 'off',
        "no-new": 'off'
    }
}
```

#### 3.2 .eslintignore 忽略文件
```
node_modules
/public
/config
/build
/dist
```

#### 3.3 .prettierrc.js
```js
module.exports = {
    // 超过最大值换行
    'printWidth': 100,
    // 缩进字节数
    'tabWidth': 2,
    // 单引号
    'singleQuote': true,
    // 尾逗号
    'trailingComma': 'none',
    // 尾分号
    'semi': false,
    // 结尾是 \n \r \n\r auto
    "endOfLine": "auto"
}
```




<br>
<br>
<br>
