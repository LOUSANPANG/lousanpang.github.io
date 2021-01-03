---
title: TypeScript系列之进阶
date: 2020-10-27
tags: 
    - TypeScript
categories: TypeScript
keywords: [TypeScript]
description: TypeScript进阶
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2021/01/03/s9lzRO.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


### 一、interfaces 与 type 之间有什么区别
#### 1.1 Objects/Functions
接口和类型别名都可以用来描述对象的形状或函数签名

接口
```ts
interface Point {
  x: number;
  y: number;
}
interface SetPoint {
  (x: number, y: number): void;
}
```

类型别名
```ts
type Point = {
  x: number;
  y: number;
};
type SetPoint = (x: number, y: number) => void;
```

#### 1.2 Other Types
与接口类型不一样，类型别名可以用于一些其他类型，比如原始类型、联合类型和元组：
```ts
// primitive
type Name = string;

// object
type PartialPointX = { x: number; };
type PartialPointY = { y: number; };

// union
type PartialPoint = PartialPointX | PartialPointY;

// tuple
type Data = [number, string];
```

#### 1.3 Extend
接口和类型别名都能够被扩展，但语法有所不同。此外，接口和类型别名不是互斥的。接口可以扩展类型别名，而反过来是不行的。

`Interface extends interface`
```ts
interface PartialPointX { x: number; }
interface Point extends PartialPointX { 
  y: number; 
}
```

`Type alias extends type alias`
```ts
type PartialPointX = { x: number; };
type Point = PartialPointX & { y: number; };
```

`Interface extends type alias`
```ts
type PartialPointX = { x: number; };
interface Point extends PartialPointX { y: number; }
```

`Type alias extends interface`
```ts
interface PartialPointX { x: number; }
type Point = PartialPointX & { y: number; };
```

#### 1.4 Implements
类可以以相同的方式实现接口或类型别名，但类不能实现使用类型别名定义的联合类型：
```ts
interface Point {
  x: number;
  y: number;
}
class SomePoint implements Point {
  x = 1;
  y = 2;
}

type Point2 = {
  x: number;
  y: number;
};
class SomePoint2 implements Point2 {
  x = 1;
  y = 2;
}

type PartialPoint = { x: number; } | { y: number; };
// A class can only implement an object type or 
// intersection of object types with statically known members.
class SomePartialPoint implements PartialPoint { // Error
  x = 1;
  y = 2;
}
```

#### 1.5 Declaration merging
与类型别名不同，接口可以定义多次，会被自动合并为单个接口
```ts
interface Point { x: number; }
interface Point { y: number; }
const point: Point = { x: 1, y: 2 };
```

### 二、object, Object 和 {}
#### 


### 三、泛型
#### 3.1 T U
像传递参数一样，我们传递了我们想要用于特定函数调用的类型。

其中 `T` 代表 `Type`，在定义泛型时通常用作第一个类型变量名称。但实际上 `T` 可以用任何有效名称代替。除了 `T` 之外，以下是常见泛型变量代表的意思：

- `K（Key）`：表示对象中的键类型；
- `V（Value）`：表示对象中的值类型；
- `E（Element）`：表示元素类型;

其实并不是只能定义一个类型变量，我们可以引入希望定义的任何数量的类型变量。比如我们引入一个新的类型变量 U，用于扩展我们定义的 identity 函数：
```ts
function identity <T, U>(value: T, message: U) : T {
  return value;
}
identity(68, "Semlinker");
```

#### 3.2 Partial <P>
Partial 作用是将传入的属性变为可选项.

`keyof`可以用来取得一个对象接口的所有`key`值.
```ts
interface Foo {
    name: string;
    age: number;
}
type T = keyof Foo // 'name' | 'age'
```

`in`则可以遍历枚举类型
```ts
type Keys = 'a' | 'b'
type Obj = {
    [p in Keys]: any
} // { a: any, b: any }
```

`keyof`产生联合类型，`in`则可以遍历枚举类型，所以他们经常结合使用，看下`Partial`源码：
```ts
type Partial<T> = { [P in keyof T]?: T[P] };
```
意思是`keyof T`拿到`T`所有属性名，然后`in`进行遍历，将值赋给P，最后`T[P]`取得相应的属性的值，`?`就明白了`Partial`的含义了。


### 3.3 Required
`Required`的作用是将传入的属性变为必须项，源码如下：
```ts
type Required<T> = { [P in keyof T]-?: T[P] };
```
我们发现一个有意思的用法 `-?`, 这里很好理解就是将可选项代表的`?`去掉, 从而让这个类型变成必选项. 与之对应的还有个+`?`, 这个含义自然与`-?`之前相反, 它是用来把属性变成可选项的.


### 3.4 Readonly
将传入的属性变为只读选项，源码如下：
```ts
type Readonly<T> = { readonly [P in keyof T]: T[P] };
```


### 3.5 Mutable
未包含。

类似地, 其实还有对 `+` 和 `-`, 这里要说的不是变量的之间的进行加减而是对 `readonly` 进行加减.
以下代码的作用就是将 `T` 的所有属性的 `readonly` 移除,你也可以写一个相反的出来.
```ts
type Mutable<T> = {
  -readonly [P in keyof T]: T[P]
}
```


### 3.6 Record
将`K`中的所有的属性的值转化为`T`类型
```ts
type Record<K extends keyof any, T> = { [P in K]: T };

应用：
type a = Record<string, T>;
// ==>
type a = {
    [key: string]: T;
};
```


### 3.7 Pick
从 `T` 中取出一系列 `K` 的属性
```ts
type Pick<T, K extends keyof T> = { [P in K]: T[P] };
```


### 3.8 Exclude
```ts
T extends U ? X : Y
```
以上语句的意思就是 如果 T 是 U 的子类型的话，那么就会返回 X，否则返回 Y

```ts
type TypeName<T> =
    T extends string ? "string" :
    T extends number ? "number" :
    T extends boolean ? "boolean" :
    T extends undefined ? "undefined" :
    T extends Function ? "function" :
    "object";
```

对于联合类型来说会自动分发条件，例如 `T extends U ? X : Y`, `T` 可能是 `A | B` 的联合类型, 那实际情况就变成`(A extends U ? X : Y) | (B extends U ? X : Y)`

`Exclude`源码:
```ts
type Exclude<T, U> = T extends U ? never : T;

type T = Exclude<1 | 2, 1 | 3> // -> 2
```
根据代码和示例我们可以推断出 `Exclude` 的作用是从 `T` 中找出 `U` 中没有的元素, 换种更加贴近语义的说法其实就是从`T` 中排除 `U`.


### 3.9 Extract
`Extract`源码：
```ts
type Extract<T, U> = T extends U ? T : never;
```
根据源码我们推断出 `Extract` 的作用是提取出 `T `包含在 `U` 中的元素, 换种更加贴近语义的说法就是从 `T` 中提取出 `U`.


### 3.10 Omit
未包含.

用之前的 `Pick` 和 `Exclude` 进行组合, 实现忽略对象某些属性功能, 源码如下
```ts
type Omit<T, K> = Pick<T, Exclude<keyof T, K>>

// 使用
type Foo = Omit<{name: string, age: number}, 'name'> // -> { age: number }
```


### 3.11 ReturnType
在阅读源码之前我们需要了解一下 `infer` 这个关键字, 在条件类型语句中, 我们可以用 `infer` 声明一个类型变量并且对它进行使用,我们可以用它获取函数的返回类型， 源码如下:
```ts
type ReturnType<T> = T extends (
  ...args: any[]
) => infer R
  ? R
  : any;
```
其实这里的 infer R 就是声明一个变量来承载传入函数签名的返回值类型, 简单说就是用它取到函数返回值的类型方便之后使用, 具体用法:
```ts
function foo(x: number): Array<number> {
  return [x];
}
type fn = ReturnType<typeof foo>;
```


### 3。12 AxiosReturnType
未包含

开发经常使用 `axios` 进行封装API层请求, 通常是一个函数返回一个 `AxiosPromise<Resp>`, 现在我想取到它的 `Resp` 类型, 根据上一个工具泛型的知识我们可以这样写.
```ts
import { AxiosPromise } from 'axios' // 导入接口
type AxiosReturnType<T> = T extends (...args: any[]) => AxiosPromise<infer R> ? R : any

// 使用
type Resp = AxiosReturnType<Api> // 泛型参数中传入你的 Api 请求函数
```


### 四、[装饰器](https://mp.weixin.qq.com/s?__biz=MzI2MjcxNTQ0Nw==&mid=2247484552&idx=1&sn=fe548e36a4fcda8e103ae6a5cb6cec41&chksm=ea47a5d0dd302cc6fef0c6eab2e585aed4563e90a97a21094ee00d871d9cddaa7a2ba881533c&scene=21#wechat_redirect)




<br>
<br>
<br>
