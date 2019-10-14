---
title: 编码规范
date: 2019-10-16
tags:
    - 编码规范
categories: 编码规范
keywords: [编码规范]
description: 编码规范
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/10/14/KSSRHK.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 选择编程风格

程序员固然可以自由选择编程风格，但是好的编程风格有助于写出质量更高、错误更少、更易于维护的程序。
这里整理了常常犯低级编码规范的错误。
<br>

[JavaScript 编码规范-Taro](https://nervjs.github.io/taro/docs/spec-for-taro.html#javascript-%E4%B9%A6%E5%86%99%E8%A7%84%E8%8C%83)
[Javascript 编程风格-阮一峰](http://www.ruanyifeng.com/blog/2012/04/javascript_programming_style.html)
<br>

### 一、文件命名规范

#### 1.1 `utils`工具文件夹

<table>
    <tr><td height=50px  bgcolor=#F5F5D5>普通 JS/TS 文件以小写字母命名，多个单词以下划线连接，例如 util.js、util_helper.js</td></tr>
</table> 
<br>

#### 1.2 `components`组件文件夹

<table>
    <tr><td height=50px bgcolor=#F5F5D5>组件文件命名遵循 Pascal 命名法，例如 ReservationCard.jsx</td></tr>
</table>
<br>

### 二、JavaScript 书写规范
**使用二个空格进行缩进**
```
    let num = 1 // 错误

  let num = 1 // 正确
```

**字符串统一使用单引号**
```
  let string = "hi" // 错误
 
  let string = 'hi' // 正确
```

**关键字后面加空格, 例如 `if` 、`for`**
```
  if() {} // 错误

  if () {} // 正确
```

**函数声明时括号与函数名间加空格, 例如**
```
  function name(arg) { ... } // 错误

  function name (arg) { ... } // 正确
```

**模板字符串中变量前后不加空格，例如**
```
const message = `Hello, ${ name }` // 错误

const message = `Hello, ${name}` // 正确
```

**点号操作符须与属性需在同一行，例如**
```
    console.
    log('hello') // 错误

    console
    .log('hello') // 正确
```

**对于变量和函数名统一使用小驼峰命名法**
```
  let theimport = '' // 错误

  let theImport = '' // 正确
```

**嵌套的代码块中禁止再定义函数，例如**
```
    if (authenticated) {
        function setAuthUser () {}    // 错误
    }

    function setAuthUser () {}
    if (authenticated) {
        this.setAuthUser()    // 正确
    }
```

**类名要以大写字母开头**
```
class animal {}
const dog = new animal()    // ✗ 错误

class Animal {}
const dog = new Animal()    // ✓ 正确
```

**子类的构造器中一定要调用 super**
```
class Dog {
  constructor () {
    super()   // ✗ 错误
  }
}

class Dog extends Mammal {
  constructor () {
    super()   // ✓ 正确
  }
}
```

**使用 this 前请确保 super() 已调用**
```
class Dog extends Animal {
  constructor () {
    this.legs = 4     // ✗ 错误
    super()
  }
}
```

**new 创建对象实例后需要赋值给变量**
```
new Character()                     // ✗ 错误
const character = new Character()   // ✓ 正确
```

**避免在 return 语句中出现赋值语句**
```
function sum (a, b) {
  return result = a + b     // ✗ 错误
}
```

**if/else 关键字要与花括号保持在同一行**
```
if (condition) { // ✓ 正确
  // ...
} else {
  // ...
}

if (condition) // ✗ 错误
{
  // ...
}
else
{
  // ...
}
```

**对于三元运算符 ? 和 : 与他们所负责的代码处于同一行**
```
// ✓ 正确
const location = env.development
  ? 'localhost'
  : 'www.api.com'

// ✗ 错误
const location = env.development ?
  'localhost' :
  'www.api.com'
```

**如果有更好的实现，尽量不要使用三元表达式**
```
let score = val ? val : 0     // ✗ 错误
let score = val || 0          // ✓ 正确
```

**多个属性，多行书写，每个属性占用一行，标签结束另起一行**
```
// bad
<Foo superLongParam='bar'
     anotherSuperLongParam='baz' />

// good
<Foo
  superLongParam='bar'
  anotherSuperLongParam='baz'
/>
```


### 三、注释
**注释首尾留空格，例如**
```
  // comment 

  /* comment */
```


<br>
<br>
<br>
<br>
<br>