---
title: JavaScript常见的报错及异常捕获与处理方法
date: 2019-09-17
tags: 
    - error
categories: 项目规范
keywords: [项目规范]
description: JavaScript常见的一些错误类型
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2020/12/07/DvMLd0.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

[![错误类型](https://s3.ax1x.com/2020/12/07/DvMLd0.png)](https://imgchr.com/i/DvMLd0)


## 一、常见的错误类型
* RangeError：标记一个错误，当设置的数值超出相应的范围触发。比如，new Array(-20)。
* ReferenceError：引用类型错误，当一个不存在的变量被引用时发生的错误。比如：console.log(a)。
* SyntaxError：语法错误。比如 if(true) {。
* TypeError：类型错误，表示值的类型非预期类型时发生的错误。


## 二、常见的错误

### 2.1 RangeError
1. RangeError: Maximum call stack size exceeded (超出了最大的堆栈大小)
- 使用递归时消耗大量堆栈，导致浏览器抛出错误，因为浏览器给分配的内存不是无限的.


### 2.2 ReferenceError
1. ReferenceError: "x" is not defined (“x”未定义)
- 引用一个没有定义的变量

### 2.3 SyntaxError
1. SyntaxError: Identifier 'x' has already been declared (标识符已申明)
- 某个变量名称已经作为参数出现了，又在再次声明
2. SyntaxError: Invalid or unexpected token (捕获无效或意外的标记)
- 代码中有非法的字符或者缺少必要的标识符号，比如减号 ( - ) 与连接符 ( – ) ，或者是英文双引号 ( " ) 与中文双引号 ( “ )
3. SyntaxError: Unexpected end of input (意外的终止输入)
- 码中某些地方的括号或引号不匹配缺失
4. SyntaxError: Invalid regular expression flags (正则表达式标志无效)
- 在代码中出现了无效的正则表达式的标记

### 2.4 TypeError
1. TypeError: Cannot read property 'x' of undefined (无法读取属性‘x’)
- 访问或设置未定义(undefined)或null值的属性时会发生这种报错
2. TypeError: 'x' is not a constructor (表示 ‘x’不是构造函数)
- 使用不是构造器的对象或者变量来作为构造器使用


## 三、异常调试及捕获
1. try/catch，Js中处理异常的一种模式，try用于可能会发生错误的代码，catch对错误的处理。
```js
try{
  console.log(a)
}catch(error) {
   // 打印错误信息
  console.log(error)  // ReferenceError: a is not defined
}
```

2. throw，用来抛出一个用户自定义的异常，执行将被停止。
```js
function getUserName(name) {
    if(!name) throw new Error('用户名无效');
    return name;
}
getUserName()
```

3. Promise 的异常处理,Promise执行中，本身自带try...catch的异常处理，出错时，将错误Rejact函数。
```js
new Promise((resolve, reject) => {
   throw new Error('error!');
}).catch(alert);
```

4. console

5. debugger
<br />
<br />
<br />
<br />
