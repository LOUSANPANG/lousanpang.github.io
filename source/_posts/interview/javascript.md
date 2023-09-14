---
title: 前端面试复习计划之JavaScript
date: 2020-1-1
tags: 
    - JavaScript
categories: 面试
keywords: [面试]
description: 前端面试复习计划
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2022/10/26/7643dd8ec8d69755.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## 数据类型 & 类型检测

### 基本类型(值类型)、引用类型(复杂数据类型)

- Number(数字)、String(字符串)、Boolean(布尔)、null(空)、undefined(未定义)、Symbol(符号)、BigInt(精度整数)
- Object(对象)、Function(函数)、Array(数组)、Date(日期)、RegExp(正则表达式)、Global、Math(单体内置对象)


### 类型检测

- typeof
  - 优点：可以区分基本类型
  - 缺点：不能详细区分引用类型，`Null` 被认为 `Object` 类型
- instanceof
  - 用于判断一个引用类型是否属于某构造函数
  - 还可以在继承关系中用来判断一个实例是否属于它的父类型
  - 优点：能够区分Array、Object和Function，适合用于判断自定义的类实例对象
  - 缺点：Number，Boolean，String基本数据类型不能判断
- Object.prototype.toString.call()
  - 优点：精准判断任何数据类型



## 执行上下文 & 闭包 & this指向

### 暂时性死区

在块作用域内，let声明的变量只是**创建时被提升**，初始化并没有被提升，在初始化之前使用变量，就会形成一个暂时性死区。

```js
{
  console.log(a) // Cannot access 'a' before initialization
  let a = 1
}
```


### 执行上下文

- 执行环境：全局环境、函数环境、Eval环境
- 创建阶段：作用域链、变量对象、this
- 执行阶段：变量赋值、函数引用等


### 闭包


### this指向

- 普通函数调用：通过函数名()直接调用：this指向全局对象window（注意let定义的变量不是window属性，只有window.xxx定义的才是。即let a =’aaa’; this.a是undefined）
- 构造函数调用：函数作为构造函数，用new关键字调用时：this指向新new出的对象
- 对象函数调用：通过对象.函数名()调用的：this指向这个对象
- 箭头函数调用：箭头函数里面没有 this ，所以永远是上层作用域this（上下文）
- apply和call调用：函数体内 this 的指向的是 call/apply 方法第一个参数，若为空默认是指向全局对象window。
- 函数作为数组的一个元素，通过数组下标调用的：this指向这个数组
- 函数作为window内置函数的回调函数调用：this指向window（如setInterval setTimeout 等）


### call/apply/bind 的区别

- fn.call(obj, 1, 2) 函数立即调用
- fn.apply(obj, [1, 2]) 函数立即调用
- fn.bind(obj, 1, 2) 返回对应函数



## 原型 & 原型链 & 继承 & 作用域

### 作用域

- 当前创建函数或变量所处的上下文
- ES6之前有全局作用域、函数作用域，现在又有了块级作用域
- 内部作用域可访问外部作用域，外部作用域访问不了内部作用域，起到隔离保护作用


### 作用域链

- 变量取值，如果在当前作用域中没有查到值，就会向上级作用域去查，直到查到全局作用域，查找过程形成的链条就叫做作用域链


### 原型关系

- 每个 class都有显示原型 prototype
- 每个实例都有隐式原型 **proto**
- 实例的 **proto** 指向对应 class 的 prototype


### 原型

在 JS 中，每当定义一个对象（函数也是对象）时，对象中都会包含一些预定义的属性。其中每个函数对象都有一个prototype 属性，这个属性指向函数的原型对象。


### 原型链

函数的原型链对象constructor默认指向函数本身，原型对象除了有原型属性外，为了实现继承，还有一个原型链指针__proto__,该指针是指向上一层的原型对象，而上一层的原型对象的结构依然类似。因此可以利用__proto__一直指向Object的原型对象上，而Object原型对象用Object.prototype.**proto** = null表示原型链顶端。如此形成了js的原型链继承。同时所有的js对象都有Object的基本防范


### 面向对象

- 面向对象简而言之就是程序中所有的操作都是通过对象来完成。主要是对对象单词的理解，在计算机当中一种事物到了程序中就变成对象，一条狗就是一个事物。程序中所有的对象被分为两部分属性、方法，以一条狗为例，属性一只动物、性别公、毛色黑这是它的数据，它会叫这是它的功能，同样在程序中，数据是属性、功能是方法。
- 如何创建对象就要先定义类，可以理解为对象模型，Dog类创建狗的模型，不同的类用来创建不同的对象。


### 类、构造函数

- 类 class

```ts
  class Dog {
    name;
    age;
    say() {}
  }
  const dog = new Dog()
  const dog1 = new Dog()
```

- 构造函数 constructor

  ```ts
  class Dog {
    name: string;
    age: number;
    constructor(name: string, age: number) {
      // this -> dog dog1
      this.name = name
      this.age = age
    }
    say() {}
  }
  const dog = new Dog('Mike', 1)
  const dog1 = new Dog('Joi', 2)
  ```


### new操作符的实例化过程

- 创建空对象
- 空对象隐式原型__proto__指向构造函数的原型
- 将空对象作为构造函数的上下文(新对象和函数调用this相绑定)
- 执行构造函数中的代码
- 如何函数没有返回值，那么会自动返回这个新对象

  ```js
  /**
   * new Create() 的过程，可以通过该函数来模拟
   * @params {Function} fn new操作符的目标函数
   * @params {Array} args 参数
   */
  function create(fn, ...args) {
    let obj = Object.create({})

    // obj.__proto__ = fn.prototype
    Object.setPrototypeOf(obj, fn.prototype)

    // 改变构造函数指向 并 执行
    let result = fn.apply(obj, args)

    // 如果构造函数返回值为对象则返回对象，否则返回 obj
    return result instanceof Object ? result : obj
  }
  ```



## Event Loop

- 微任务：Promise回调 mutation回调
- 宏任务：DOM事件回调 Ajax回调 定时器回调

### 同步及异步执行循序

- 代码自上而下执行
- 遇到同步代码，执行同步代码
- 遇到异步代码，将它的回调函数存到事件队列中
- 同步代码执行完，从事件队列中将异步回调函数按顺序执行

  ```JS
  console.log(1) // 同步
  setTimeout(() => console.log(2), 0) // 异步
  console.log(3) // 同步
  // 输出： 1 3 2
  ```


### 异步在事件队列中的执行顺序

- 事件队列中先执行 微任务 再执行 宏任务
- 执行宏任务时，遇到微任务，就将它添加到微任务的任务队列中
- 宏任务执行完毕后，立即执行当前宏任务的微任务队列

  ```js
  console.log(1) // 同步
  setTimeout(() => {
    console.log(2) // 异步：宏任务 setTimeout1
    Promise.resolve().then(() => { // 异步：微任务 then1
      console.log(3)
    })
  });
  console.log(4) // 同步
  new Promise((resolve,reject) => {
    console.log(5) // 同步
    resolve()
  }).then(() => { // 异步：微任务 then2
    console.log(6)
    setTimeout(() => {
      console.log(7) // 异步：宏任务 setTimeout2
    })
  })
  console.log(8) // 同步

  // 1，4，5，8 6 2 3 7
  ```



## ES2015+

### 箭头函数的特性

- 没有自己的this，会捕获其所在的上下文的this值，作为自己的this值
- 没有constructor，是匿名函数，不能作为构造函数，不能通过new 调用
- 没有原型属性 Fn.prototype 值为 undefined
- 不能当做Generator函数,不能使用yield关键字



## 模块化

### namespace

- 优点：通过对象的形式对变量进行包裹，避免命名冲突
- 缺点：定义的命名空间可以被外部任意修改


### IIFE立即执行函数式

- 函数内的变量不会造成全局污染
- 函数执行完后就会立即销毁，不会造成资源浪费
- 缺点：很难对模块进行拆分并完成相互通讯，不可复用


### AMD 异步模块定义

- 它采用异步的方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在回调函数中，等到加载完成之后，这个回调函数才会运行。
- 引入模块需要用到 RequireJS

  ```js
  // 1 引入require.js

  // 2 moduleA.js 导出
  define(function(){
    const name = "module A";
    return {
      getName() {
        return name
      }
    }
  });

  // 3 引入
  require(["moduleA"], function(moduleA){
    moduleA.getName()
  });
  ```


### CMD 按需加载

- CMD 是 SeaJS 在推广过程中对模块定义的规范化产出
- 依赖就近，用的时候再require
- 相对于 AMD，对依赖模块的执行时机处理不同

  ```js
  define(function(require, exports, module) {
    var clock = require('clock');
    clock.start();
  });
  ```


### CJS

- CommonJs是Nodejs内置的同步加载的模块化机制
- 导出值为简单类型是复制，导出值为复杂类型是浅拷贝输出，模块会被缓存，可修改导出的值
- 可写在函数体中使用require引入
- this是当前模块
- 运行时进行加载模块


### UMD

- 通用模块定义，兼容了CmmonJS和AMD规范
- 判断全局是否存在exports和define，如果存在exports，那么以CommonJS的方式暴露模块，如果存在define那么以AMD的方式暴露模块


### ESM

- 存在提升行为，会提升到整个模块的头部，首先执行
- 不允许修改引入的值，属于只读引用
- 编译时输出
- 对于动态 import().then 来说，原始值发生变化，import加载的值也会发生变化
- this是undefined
- 浏览器加载 ESM，使用 `<script>` 标签，要加入 type="module" 属性
- 具有更好的可摇树性



## JavaScript的设计模式

### 解决某个特定场景下对某种问题的解决方案

设计模式 | 特点 | 案例
---------|----------|---------
单例模式 | 只有一个实例可以全局访问 | 弹框组件（先创建再隐藏，需要时显示创建；对应模式只有一个实例）
策略模式 | 根据不同参数可以命中不同的策略 | if else 根据不同参数做出处理
代理模式 | 代理对象和本体对象具有一致的接口 | 虚拟代理（图片预加载）；缓存代理（累积计算）
中介者模式 | 对象和对象之间借助第三方中介者进行通信 | 测试结束告知结果
装饰者模式 | 动态地给函数赋能 | 天冷了穿衣服，热了脱衣服
发布-订阅模式 | 事件发布/订阅模式 | 微信小程序的 emit/on 监听事件
观察者模式 | 当观察对象发生变化时自动调用相关函数 | vue 双向绑定 Proxy

