---
title: module模块的导入导出
date: 2018-10-31
tags: 
    - JavaScript
categories: JavaScript
keywords: [module]
description: module
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s2.ax1x.com/2019/10/31/KoDGRO.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

![MODULES](https://s2.ax1x.com/2019/10/31/KoD7SU.md.png)

<br>

对于模块化，这是我从认识到模块化这个概念，从`CommonJS`规范到现在`ES6`的模块化以来，自己的疑惑和认识。

### 一、CommonJS
#### 1.1 require(x)用法
a) 如果 X 是内置模块（比如 require('http'）)
> 返回该模块。
>> 不再继续执行。

b) 如果 X 以 "./" 或者 "/" 或者 "../" 开头
> 根据 X 所在的父模块，确定 X 的绝对路径。
>> 将 X 当成文件，依次查找下面文件(x、x.js、x.json、x.node)，只要其中有一个存在，就返回该文件，不再继续执行。

>> 将 X 当成目录，依次查找下面文件(X/package.json（main字段）、X/index.js、X/index.json、X/index.node)，只要其中有一个存在，就返回该文件，不再继续执行。

c) 如果 X 不带路径
> 根据 X 所在的父模块，确定 X 可能的安装目录。
>> 依次在每个目录中，将 X 当成文件名或目录名加载。

对于`require(x)`用法，前两种很好理解，对于第三种，引入第三方依赖文件，node是怎样找出该文件的呢？
1. const lib = require('lib'); 
当前脚本文件 /src/require/index.js 执行了 require('lib')
2. 首先，node 确定 lib 的绝对路径可能是下面这些位置，依次搜索每一个目录。
 --> /src/require/node_modules/lib
 --> src/node_modules/lib
 --> /node_modules/lib
3. 搜索时，Node 先将 lib 当成文件名，依次尝试加载下面这些文件，只要有一个成功就返回。
lib lib.js lib.json lib.node
4. 如果都不成功，说明 lib 可能是目录名，于是依次尝试加载下面这些文件。
lib/package.json lib/index.js lib/index.json lib/index.node

#### 1.2 关于Module构造函数
对于require执行后，是怎么找到`path`,`exports`,这些相对应的属性？
```
function Module(id, parent) {
    this.id = id;
    this.exports = {};
    this.parent = parent;
    this.filename = null;
    this.loaded = false;
    this.children = [];
}
// module.exports = Module;
// var module = new Module(filename, parent)
```
上面这是require的源码在 Node 的 lib/module.js 文件中的简化代码片段。这个构造函数有 `id exports parent filename loaded children path` 属性。
```
console.log('module.id: ', module.id); 
console.log('module.exports: ', module.exports); 
console.log('module.parent: ', module.parent); 
console.log('module.filename: ', module.filename); 
console.log('module.loaded: ', module.loaded); 
console.log('module.children: ', module.children); 
console.log('module.paths: ', module.paths); 

module.id = .
module.exports = {}
module.parent = null
module.filename = /src/require/require1.js
module.loaded = false
module.children = []
module.paths = [
  '\\src\\require\\node_modules',
  '\\src\\node_modules',
  '\\node_modules',
  '\\node_modules'
]
```
`$ node require1.js`
可以看到，如果没有父模块，直接调用当前模块，parent 属性就是 null，id 属性就是一个点。filename 属性是模块的绝对路径，path 属性是一个数组，包含了模块可能的位置。另外，输出这些内容时，模块还没有全部加载，所以 loaded 属性为 false 。

新建一`require2.js`文件，并引用 `const require2 = require('./require2.js');`
`$ node require2.js`
```
module.id:  /src/require/require1.js
module.exports:  {}
module.parent:  { object }
module.filename:   /src/require/require1.js
module.loaded:  false
module.children:  []
module.paths:  [ '/src/require/node_modules',
  '/src/node_modules',
  '/node_modules' ]
```
上面代码中，由于 a.js 被 b.js 调用，所以 parent 属性指向 b.js 模块，id 属性和 filename 属性一致，都是模块的绝对路径。

#### 1.3 模块实例require方法
每个模块实例都有一个 require 方法。
在`Module`的原型上有个`require`方法
```
Module.prototype.require = function(path) {
    return Module._load(path, this);
};
```
```
// Module._load 源码。
Module._load = function(request, parent, isMain) {
    //  解析出模块的绝对路径,以它作为模块的识别符。
    var filename = Module._resolveFilename(request, parent);
  
    //  第一步：如果有缓存，取出缓存
    var cachedModule = Module._cache[filename];
    if (cachedModule) {
      return cachedModule.exports;
    }
  
    // 第二步：是否为内置模块
    if (NativeModule.exists(filename)) {
      return NativeModule.require(filename);
    }
  
    // 第三步：生成模块实例，存入缓存
    var module = new Module(filename, parent);
    Module._cache[filename] = module;
  
    // 第四步：加载模块
    try {
      module.load(filename);
      hadException = false;
    } finally {
      if (hadException) {
        delete Module._cache[filename];
      }
    }
  
    // 第五步：输出模块的exports属性
    return module.exports;
};
```
require方法返回了`_load`方法，`_load`方法主要的步骤就二步，第一步是解析出文件的绝对路径，第二步加载模块。
在加载模块步骤中，相当于这样的一个函数：
```
(function (exports, require, module, __filename, __dirname) {
    // 模块源码
});
```
> 模块的加载实质
>> 注入exports、require、module三个全局变量
>>> 然后执行模块的源码
>>>> 将模块的 exports 变量的值输出。

#### 1.4 对于导出模块
从上边变得`require`源码就可以清晰知道，`module.exports` `exports`的区别。
```
exports.xx = ''
module.exports = {
	xx: ''
}
```

#### 1.5 CommonJS关于循环加载
特点：
1. 加载时执行，即脚本代码在require的时候，就会全部执行;
2. 关于循环加载：一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。
3. 关于缓存：同一文件执行第二次的时候，不会再次执行而是输出缓存结果。

```
// a.js
exports.done = false;

var b = require('./b.js');

console.log('在 a.js 之中，b.done = ', b.done);
exports.done = true;
console.log('a.js 执行完毕')
```

```
// b.js
exports.done = false;

var a = require('./a.js');

console.log('在 b.js 之中，a.done = ', a.done);
exports.done = true;
console.log('b.js 执行完毕');
```

```
// main.js
var a = require('./a.js');
var b = require('./b.js');
console.log('在 main.js 之中, a.done=, b.done=', a.done, b.done);
```
在`main.js`中运行文件
```
$ node main.js

在 b.js 之中，a.done = false
b.js 执行完毕
在 a.js 之中，b.done = true
a.js 执行完毕
在 main.js 之中, a.done=true, b.done=true
```
上面的代码证明了两件事。一是，在b.js之中，a.js没有执行完毕，只执行了第一行。二是，main.js执行到第二行时，不会再次执行b.js，而是输出缓存的b.js的执行结果，即它的第四行`exports.done = true;`。

### 二、ES6: 统一了服务端和客户端模块规范
#### 2.1 import用法
```
import {} from  --- export 声明或语句
import xxx from --- export default 表达式 （仅有一个）
```
as 用法
```
export{
   a as aaa,
   b as bbb
}
import {aaa as a, bbb} from './b.js'
```

#### 2.2 import 特点
1. import 可以是相对路径，也可以是绝对路径
import 'https://code.jquery.com/jquery-3.3.1.js';
2. import模块只会导入一次，无论你引入多少次
3. import './b.js';  如果这么用，相当于引入文件
4. 有提升效果，import会自动提升到顶部，首先执行
5. 不同于CommonJs规范缓存, 导出去模块内容，如果里面有定时器更改，外面也会随之改动
```
// a.js
import {foo} from './b.js';
console.log(foo); // 'bar'
setTimeout(() => console.log(foo), 5000); '4000-bar'

// b.js
export var foo = 'bar';
setTimeout(() => foo = '4000-bar', 4000);
```
6. import() 类似node里面require，可以动态引入, 默认import语法不能写到if之类里面
```
let a = 11;
if (a === 10) {
    import('./b.js')
} else {
    import('./c.js')
}
```
7. import()返回值，是个promise对象
```
console.log(import('./b.js')) // Promise {<pending>}

import('./b.js').then(res=>{
    console.log(res.a + res.b);
});

Promise.all([
    import('./b.js'),
    import('./c.js')
]).then(([res1, res2]) => {
    console.log(res1),
    console.log(res2)
})
```
优点：
    1. 按需加载
    2. 可以写if中
    3. 路径也可以动态
    4. Promise.all([])

#### 2.3 ES6关于循环加载
特点：
1. 它遇到模块加载命令import时，不会去执行模块，而是只生成一个引用。等到真的需要用到时，再到模块里面去取值；
2. ES6根本不会关心是否发生了"循环加载"，只是生成一个指向被加载模块的引用，需要开发者自己保证，真正取值的时候能够取到值；
3. ES6模块是动态引用，不存在缓存值的问题，而且模块里面的变量，绑定其所在的模块。
```
// a.js
import {bar} from './b.js';
export function foo() {
  bar();
  console.log('执行完毕');
}
foo();

// b.js
import {foo} from './a.js';
export function bar() {
  if (Math.random() > 0.5) {
    foo();
  }
}
```
这样的循环代码放在`CommonJs`规范里边，是会报错的，但在`ES6`中，并没什么影响。
它只有在被使用到的时候才会从模块里边取。
你只需要跟着代码去顺就可以得到结果了。








<br>
<br>
<br>
<br>
<br>