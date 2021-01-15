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



### 一、类
类接口会对类的属性和方法进行约束，类似非抽象类继承抽象类时必须实现某些方法和属性，但对属性和方法的类型的约束更加严格，除了方法void类型可被重新定义外，其他属性或方法的类型定义需要和接口保持一致。

#### 1.1 类的类型接口
```ts
interface Animals {
  name: string;
  eat(): void;
}

class Dogs implements Animals {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  eat() {}
}
```

#### 1.2 继承--接口可以继承接口
```ts
interface Dog {
  eat(): void;
}

interface Persons extends Dog {
  work(): void;
}

class Cat {
  code() {
    console.log("猫在敲代码");
  }
}

//可继承类后再实现接口
class SuperMan extends Cat implements Persons {
  eat(): void {
    console.log(1);
  }
  work(): void {
    console.log(2);
  }
}
let superMan = new SuperMan();
superMan.code();
```

#### 1.3 类的泛型
```ts
class MinClass<T> {
  public list: T[] = [];
  //添加
  add(value: T): void {
    this.list.push(value);
  }
  
  //求最小值
  min(): T {
    //假设这个值最小
    let minNum = this.list[0];
    for (let i = 0; i < this.list.length; i++) {
    //比较并获取最小值
    minNum = minNum < this.list[i] ? minNum : this.list[i];
    }
    return minNum;
  }
}
//实例化类 并且指定了类的T的类型是number
let minClass = new MinClass<number>();
```

#### 1.4 类-多态
抽象类无法实例化。

非抽象类继承抽象父类时不会自动实现来自父类的抽象成员,必须手动定义父类中的抽象成员，否则报错。

抽象成员包括属性和方法
```ts
// 抽象父类
abstract class Animal {
  private name: string;
  constructor(name: string) {
    this.name = name;
  }
  //抽象成员--方法
  abstract eat(): any;
  //抽象成员--属性
  protected abstract ages: Number;
  sleep(): void {
    console.log("睡觉");
  }
}

class cat extends Animal {
  ages: Number = 2;
  constructor(name: string) {
    super(name);
  }
  //非抽象类“cat”不会自动实现继承自“Animal”类的抽象成员“eat”,  必须手动定义父类中的抽象方法--多态
  eat(): string {
    return "猫吃鱼";
  }

  //多态
  sleep(): string {
    return "猫在睡觉";
  }
}

console.log(new cat("33").sleep());
```


#### 二、 命名空间
在代码量较大的情况下，为了避免各种变量命名相冲突，可将相似功能的函数、类、接口等放置到命名空间内

TypeScript的命名空间可以将代码包裹起来，只对外暴露需要在外部访问的对象。

命名空间：内部模块，主要用于组织代码，避免命名冲突。

```ts
 // modules/Animal.ts
export namespace A {
  interface Animal {
    name: String;
    eat(): void;
  }

  export class Dog implements Animal {
    name: String;
    constructor(theName: string) {
      this.name = theName;
    }
    eat() {
      console.log("我是" + this.name);
    }
  }
}

export namespace B {
  interface Animal {
    name: String;
    eat(): void;
  }

  export class Dog implements Animal {
    name: String;
    constructor(theName: string) {
      this.name = theName;
    }
    eat() {}
  }
}
```
```ts
 import { A, B } from "./modules/Animal";
 let ee = new A.Dog("小贝");
 ee.eat();
```



### 三、装饰器
类装饰器：类装饰器在类申明之前被申明(紧靠着类申明)，类装饰器应用于类构造函数，可以用于监视，修改或者替换类定义。

装饰器会覆盖被装饰的类中的方法。
```ts
function logClass(params: any) {
  console.log(params);
  //params 就是指代当前类--HttpClient
  params.prototype.apiUrl = "动态扩展属性";
  params.prototype.run = function () {
    console.log("动态扩展方法");
  };
  params.prototype.getDate = function () {
    console.log("动态扩展方法2");
  };
}

@logClass
class HttpClient {
  constructor() {}
  getDate() {
    console.log(1);
  }
}

let http: any = new HttpClient();
console.log(http.apiUrl);
http.run();
http.getDate();
```


### 十、其他开发声明
#### 11.1 在window对象上显式设置属性
```ts
declare interface Window {
    MyNamespace: any;
}
window.MyNamespace = window.MyNamespace || {};
// => 等价于
(window as any).MyNamespace = {};
```


#### 11.2 对象
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

#### 11.3 函数命名
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
