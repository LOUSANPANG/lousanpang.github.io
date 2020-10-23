---
title: 编写高质量可维护的代码：一目了然的注释
date: 2020-09-05
tags: 
    - 规范注释
categories: 项目规范
keywords: [项目规范]
description: 编写高质量可维护的代码：一目了然的注释
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s1.ax1x.com/2020/10/21/BCAiNQ.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

[![注释规范](https://s1.ax1x.com/2020/10/23/BEUm8S.jpg)](https://imgchr.com/i/BEUm8S)


## 一、基础的注释

```html
<!-- 这是一行文字 -->
<p>这是一行文字</p>
```

```css
/* 文字颜色 */
p {
    color: '#fff';
}
```

```js
// 定义一个变量
const params = ''

```


## 二、拓展


### 2.1 [JSDoc规范](http://www.shouce.ren/api/view/a/13232)

- @param、参数类型、参数名、-参数说明

``` js
/**
 * Assign the project to an employee.
 * @param {Object} employee - The employee who is responsible for the project.
 * @param {string} employee.name - The name of the employee.
 * @param {string} employee.department - The employee's department.
 */
Project.prototype.assign = function(employee) {
    // ...
}; 


/**
 * Assign the project to a list of employees.
 * @param {Object[]} employees - The employees who are responsible for the project.
 * @param {string} employees[].name - The name of an employee.
 * @param {string} employees[].department - The employee's department.
 */
Project.prototype.assign = function(employees) {
    // ...
}; 


/**
 * @param {string} [somebody=John Doe] - Somebody's name.
 */
function sayHello(somebody) {
    if (!somebody) {
        somebody = 'John Doe';
    }
    alert('Hello ' + somebody);
} 
```

- @returns

```js
/**
 * Returns the sum of a and b
 * @param {Number} a
 * @param {Number} b
 * @param {Boolean} retArr If set to true, the function will return an array
 * @returns {Number|Array} Sum of a and b or an array that contains a, b and the sum of a and b.
 */
function sum(a, b, retArr) {
    if (retArr) {
        return [a, b, a + b];
    }
    return a + b;
} 
```

- @document

```js
  /*
   * 简述当前文件功能
   * @author 作者名称
   * @version 版本号 最近编辑时间
   * @description 该版本改动信息
   */
```


### 2.2 特殊标记注释

- `TODO` 功能未完成，待完善 (在该注释处有功能代码待编写，待实现的功能在说明中会简略说明)
- `FIXME` 待修复 (在该注释处代码需要修正，甚至代码是错误的，不能工作，需要修复，如何修正会在说明中简略说明)
- `XXX` 实现方法待确认 (在该注释处代码虽然实现了功能，但是实现的方法有待商榷，希望将来能改进，要改进的地方会在说明中简略说明)
- `NOTE` 代码功能说明 (在该注释处说明代码如何工作)
- `HACK` 此处写法有待优化 (在该注释处编写得不好或格式错误，需要根据自己的需求去调整程序代码)
- `BUG` 此处有Bug (在该注释处有 Bug)


## 三、`VScode` 插件

- [koroFileHeader 文件头部、函数参数注释插件](https://marketplace.visualstudio.com/items?itemName=OBKoro1.korofileheader)


- [Better Comments 警报，信息，TODO 等进行注释来改善代码注释](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)

- [TODO Highlight 突出显示TODO，FIXME和任何关键字](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)

<br />
<br />
<br />
<br />
