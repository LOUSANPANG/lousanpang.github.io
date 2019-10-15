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

#### 2.3 框架
**页面文件中的config，在编译后将生成全局配置文件 app.json**
<br>

**页面生命周期**

`componentWillMount()`
页面加载时触发, 此时页面 DOM 尚未准备好，还不能和视图层进行交互

`componentDidMount()`
页面初次渲染完成时触发, 页面已经准备妥当，可以和视图层进行交互

`shouldComponentUpdate(nextProps, nextState)`
页面是否需要更新，返回 false 不继续更新，否则继续走更新流程

`componentWillUpdate(nextProps, nextState)`
页面即将更新

`componentDidUpdate(nextProps, nextState)`
页面更新完毕

`componentWillUnmount`
页面卸载时触发,如 redirectTo 或 navigateBack 到其他页面时

`componentDidShow()`
页面显示|切入前台时触发

 `componentDidHide()`
 页面隐藏/切入后台时触发， 如 navigateTo 或底部 tab 切换到其他页面，小程序切入后台等

 **页面事件处理函数**
 <table>
    <tr><td height=50px bgcolor=#F5F5D5>H5 暂时没有同步实现 onReachBottom 、 onPageScroll 这两个事件函数，可以通过给 window 绑定 scroll 事件来进行模拟，而 onPullDownRefresh 下拉刷新则暂时只能用 ScrollView 组件来代替了</td></tr>
</table>

**组件**
当你在 Taro 组件中引用原生小程序组件代码时，则需要通过配置 config 来实现

|属性|类型|描述|
|-|-|-|
|usingComponents|Object|组件自定义组件配置|

**组件生命周期**
`componentWillReceiveProps(nextProps)`
已经装载的组件接收到新属性前调用


#### 2.4 最佳实践
**Taro 的编译器也会对无法运行的代码进行警告，如果你需要在编译时禁用掉 ESLint 检查，可以在命令前加入 ESLINT=false 参数，例如：**
```
$ ESLINT=false taro build --type weapp --watch
```
[关于组件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html)
**组件样式**
微信小程序的自定义组件样式默认是不能受外部样式影响的，例如在页面中引用了一个自定义组件，在页面样式中直接写自定义组件元素的样式是无法生效的。这一点，在 Taro 中也是一样，而这也是与大家认知的传统 Web 开发不太一样。

**给组件设置 defaultProps**
自定义组件中，只有在 properties 中指定的属性，才能从父组件传入并接收。
```
Component({
  properties: {
    myProperty: { // 属性名
      type: String, // 类型（必填），目前接受的类型包括：String, Number, Boolean, Object, Array, null（表示任意类型）
      value: '', // 属性初始值（可选），如果未指定则会根据类型选择一个
      observer: function (newVal, oldVal, changedPath) {
         // 属性被改变时执行的函数（可选），也可以写成在 methods 段中定义的方法名字符串, 如：'_propertyChange'
         // 通常 newVal 就是新设置的数据， oldVal 是旧数据
      }
    },
    myProperty2: String // 简化的定义方式
  }
  ...
})
```

在接受props的属性时, defaultProps弥补编译时，可能会有某一属性没有使用而是直接传递给子组件的情况。
```
class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {
  color: 'blue'
}
```

**组件传递函数属性名以 on 开头**
```
// 在 Taro 中，父组件要往子组件传递函数，属性名必须以 on 开头
// 调用 Custom 组件，传入 handleEvent 函数，属性名为 onTrigger
class Parent extends Component {
  handleEvent () {}

  render () {
    return (
      <Custom onTrigger={this.handleEvent}></Custom>
    )
  }
}
```

**不要在 state 与 props 上用同名的字段，因为这些字段在微信小程序中都会挂在 data 上**

**环境变量使用 process.env.NODE_ENV**

**this.$componentType 可能取值分别为 PAGE 和 COMPONENT, 来判断当前 Taro.Component 是页面还是组件**















<br>
<br>
<br>
<br>
<br>