---
title: TypeScript系列之开发应用
date: 2020-09-27
tags: 
    - TypeScript
categories: TypeScript
keywords: [TypeScript]
description: TypeScript开发应用
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2021/01/02/sS5tNn.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

### 一、常见使用
#### 1.1 在window对象上显式设置属性
```ts
declare interface Window {
    MyNamespace: any;
}
window.MyNamespace = window.MyNamespace || {};
// => 等价于
(window as any).MyNamespace = {};
```


#### 1.2 对象
**object, Object 和 {} 之间有什么区别**

**object类型,用于表示非原始类型**
```ts
// node_modules/typescript/lib/lib.es5.d.ts
interface ObjectConstructor {
  create(o: object | null): any;
  // ...
}

const proto = {};
Object.create(proto);     // OK
Object.create(null);      // OK
Object.create(undefined); // Error
Object.create(1337);      // Error
Object.create(true);      // Error
Object.create("oops");    // Error
```

**Object 类型：它是所有 Object 类的实例的类型，它由以下两个接口来定义：**

**Object 接口定义了 Object.prototype 原型对象上的属性；**
```ts
// node_modules/typescript/lib/lib.es5.d.ts
interface Object {
  constructor: Function;
  toString(): string;
  toLocaleString(): string;
  valueOf(): Object;
  hasOwnProperty(v: PropertyKey): boolean;
  isPrototypeOf(v: Object): boolean;
  propertyIsEnumerable(v: PropertyKey): boolean;
}
```

**ObjectConstructor 接口定义了 Object 类的属性。**

**Object 类的所有实例都继承了 Object 接口中的所有属性。**
```ts
// node_modules/typescript/lib/lib.es5.d.ts
interface ObjectConstructor {
  /** Invocation via `new` */
  new(value?: any): Object;
  /** Invocation via function calls */
  (value?: any): any;
  readonly prototype: Object;
  getPrototypeOf(o: any): any;
  // ···
}
declare var Object: ObjectConstructor;
```

**{} 类型**
{} 类型描述了一个没有成员的对象。当你试图访问这样一个对象的任意属性时，TypeScript 会产生一个编译时错误。
```ts
// Type {}
const obj = {};
// Error: Property 'prop' does not exist on type '{}'.
obj.prop = "semlinker";
```

但是，你仍然可以使用在 Object 类型上定义的所有属性和方法，这些属性和方法可通过 JavaScript 的原型链隐式地使用：
```ts
// Type {}
const obj = {};
// "[object Object]"
obj.toString();
```

**如何为对象动态分配属性**
```ts
// ts
interface LooseObject {
  name: string;
  age?: number;
  [key: string]: any
}
let developer: Developer = { name: "semlinker" };
developer.age = 30;
developer.city = "XiaMen";

// 内置工具类型 Record
// type Record<K extends string | number | symbol, T> = { [P in K]: T; }
interface Developer extends Record<string, any> {
  name: string;
  age?: number;
}
let developer: Developer = { name: "semlinker" };
developer.age = 30;
developer.city = "XiaMen";
```

#### 1.3 函数
**基础定义**
```ts
type Point = {
    // 常见的函数类型
    handleClick: (id: number) => void;
    // 另一种声明方式
    handleClick(event: React.MouseEvent<HTMLButtonElement>): void;
    // 构造函数 - 构造签名
    // new C
    // new C (...)
    // new C <...>(...)
    new (x: number, y: number): Point;
}
```

**构造函数类型的应用**
```ts
interface Point {
  x: number;
  y: number;
}
interface PointConstructor {
  new (x: number, y: number): Point;
}

class Point2D implements Point {
  readonly x: number;
  readonly y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}
function newPoint(
  pointConstructor: PointConstructor,
  x: number,
  y: number
): Point {
  return new pointConstructor(x, y);
}
const point: Point = newPoint(Point2D, 1, 2);
```


**多种参数导致返回值的问题**
```ts
function add(x, y) {
  return x + y;
}
add(1, 2); // 3
add("1", "2"); //"12"

// 应用
type Combinable = string | number;
function add(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}

// 缺陷： number类型的对象上并不存在split属性
const result = add('semlinker', ' kakuqo');
result.split(' ');

// 需要函数重载
class Calculator {
  add(a: number, b: number): number;
  add(a: string, b: string): string;
  add(a: string, b: number): string;
  add(a: number, b: string): string;
  add(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
    return a + b;
  }
}
const calculator = new Calculator();
const result = calculator.add('Semlinker', ' Kakuqo');
```



<br>
<br>
<br>
