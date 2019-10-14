---
title: 重新学习Taro
date: 2019-10-14
tags: 
    - Taro
categories: Taro
keywords: [Taro]
description: taro-document
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/10/14/KSPUxJ.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


### 用Taro做了几个项目之后，产生了很多疑问，这篇文章整理了再次学习Taro的新收获。
<br>


### 一、文档-快速开始
#### 1.1 关于配置打包和命令
配置- `config文件下的index.js`
```
    cssModules: {
      enable: true, // 默认为 false，如需使用 css modules 功能，则设为 true
    }
```
配置- `project.config.json`
```
    "appid": " ", // 小程序的appid
    "setting": {
        "urlCheck": true,
        "es6": false,
        "postcss": false,
        "minified": true // 压缩
    }
```
打包（去掉 --watch 将不会监听文件修改，并会对代码进行压缩打包）
```
# yarn
    $ yarn build:weapp

# npm script
    $ npm run build:weapp
```
命令
```
# Taro 所有命令及帮助
    $ taro --help

# 更新 Taro CLI 工具
    # taro
    $ taro update self
    # npm
    npm i -g @tarojs/cli@latest
    # yarn
    yarn global add @tarojs/cli@latest

# 更新项目中 Taro 相关的依赖
    $ taro update project

# 诊断
    $ taro doctor
```


#### 1.2 开发前注意
微信开发者工具的项目设置
  * 关闭 ES6 转 ES5 功能
  * 关闭上传代码时样式自动补全
  * 关闭代码压缩上传

ReactNative
[ReactNative 样式表](https://nervjs.github.io/taro/docs/before-dev-remind.html#properties-属性)
<br>

#### 1.3 特殊问题
H5 模式下，tabBar 可能会挡住页面 fixed 元素问题
```
.fixed {
  bottom: 0;
  /* 在 H5 模式下将会编译成 margin-bottom: 50px，在小程序模式下则会忽略 */
  margin-bottom: taro-tabbar-height;
}
```

### 二、基础教程
#### 2.1 书写顺序
```
static 静态方法
constructor
componentWillMount
componentDidMount
componentWillReceiveProps
shouldComponentUpdate
componentWillUpdate
componentDidUpdate
componentWillUnmount
点击回调或者事件回调 比如 onClickSubmit() 或者 onChangeDescription()
render
```

#### 2.2 通用约束与建议
<table>
    <tr><td height=50px bgcolor=#F5F5D5>尽量避免在 componentDidMount 中调用 this.setState</td></tr>
    <tr><td height=50px bgcolor=#F5F5D5>因为在 componentDidMount 中调用 this.setState 会导致触发更新</td></tr>
</table>
<br>

<table>
    <tr><td height=50px bgcolor=#F5F5D5>不要在 componentWillUpdate/componentDidUpdate/render 中调用 this.setState</td></tr>
</table>
<br>

**组件最好定义 defaultProps**
```
import Taro, { Component } from '@tarojs/taro'

class MyComponent extends Component {
  static defaultProps = {
    isEnable: true
  }
  
  render () {
    const { isEnable } = this.props

    return ()
  }
}
```

**值为 true 的属性可以省略书写值**
```
<Hello personal />
<Hello personal={false} />
```

**JSX 属性或者表达式书写时需要注意空格**
```
<Hello name={firstname} />  // ✓ 正确 属性书写不带空格
<Hello name={{ firstname: 'John', lastname: 'Doe' }} />  // ✓ 正确 属性是一个对象，则对象括号旁边需要带上空格
```

**子组件传入函数时属性名需要以 on 开头**
```
  render () {
    const { myTime } = this.state

    return (
      <View className='test'>
        <Tab onChange={this.clickHandler} />    // ✓ 正确
        <Text className='test_text'>{myTime}</Text>
      </View>
    )
  }
```

















<br>
<br>
<br>
<br>
<br>