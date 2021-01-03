---
title: TypeScript进阶
date: 2020-10-27
tags: 
    - TypeScript
categories: TypeScript
keywords: [TypeScript]
description: TypeScript进阶
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2021/01/02/sS5tNn.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 泛型
#### Partial <P>
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


### Required
`Required`的作用是将传入的属性变为必须项，源码如下：
```ts
type Required<T> = { [P in keyof T]-?: T[P] };
```
我们发现一个有意思的用法 `-?`, 这里很好理解就是将可选项代表的`?`去掉, 从而让这个类型变成必选项. 与之对应的还有个+`?`, 这个含义自然与`-?`之前相反, 它是用来把属性变成可选项的.


### Readonly
将传入的属性变为只读选项，源码如下：
```ts
type Readonly<T> = { readonly [P in keyof T]: T[P] };
```


### Mutable
未包含。

类似地, 其实还有对 `+` 和 `-`, 这里要说的不是变量的之间的进行加减而是对 `readonly` 进行加减.
以下代码的作用就是将 `T` 的所有属性的 `readonly` 移除,你也可以写一个相反的出来.
```ts
type Mutable<T> = {
  -readonly [P in keyof T]: T[P]
}
```


### Record
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


### Pick
从 `T` 中取出一系列 `K` 的属性
```ts
type Pick<T, K extends keyof T> = { [P in K]: T[P] };
```


### Exclude
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


### Extract
`Extract`源码：
```ts
type Extract<T, U> = T extends U ? T : never;
```
根据源码我们推断出 `Extract` 的作用是提取出 `T `包含在 `U` 中的元素, 换种更加贴近语义的说法就是从 `T` 中提取出 `U`.


### Omit
未包含.

用之前的 `Pick` 和 `Exclude` 进行组合, 实现忽略对象某些属性功能, 源码如下
```ts
type Omit<T, K> = Pick<T, Exclude<keyof T, K>>

// 使用
type Foo = Omit<{name: string, age: number}, 'name'> // -> { age: number }
```


### ReturnType
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


### AxiosReturnType
未包含

开发经常使用 `axios` 进行封装API层请求, 通常是一个函数返回一个 `AxiosPromise<Resp>`, 现在我想取到它的 `Resp` 类型, 根据上一个工具泛型的知识我们可以这样写.
```ts
import { AxiosPromise } from 'axios' // 导入接口
type AxiosReturnType<T> = T extends (...args: any[]) => AxiosPromise<infer R> ? R : any

// 使用
type Resp = AxiosReturnType<Api> // 泛型参数中传入你的 Api 请求函数
```







<br>
<br>
<br>
