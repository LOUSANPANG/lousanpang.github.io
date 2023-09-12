---
title: 前端面试复习计划之TypeScript
date: 2022-10-1
tags: 
    - TypeScript
categories: 面试
keywords: [面试]
description: 前端面试复习计划
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.bmp.ovh/imgs/2022/11/02/6da727f52aaa6ad7.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## 实用技巧关键字

**typeof** 获取一个变量的类型

```ts
type Data = typeof xx
```

**keyof** 用来取得一个对象接口的所有key值

```ts
interface Person {
  name: string;
  age: number;
  gender: "male" | "female";
}
//type PersonKey = 'name'|'age'|'gender'
type PersonKey = keyof Person
```

**in** 去批量定义类型中的属性

```ts
interface Person {
  name: string;
  age: number;
  gender: "male" | "female";
}
//批量把一个接口中的属性都变成可选的
type PartPerson = {
  [Key in keyof Person]?: Person[Key]
}
```

**abstract** 抽象类

```ts
// 抽象类不能用来创建对象，只能专门用来被继承类
abstract class Animal {
  name: string;
  constructor(name) {
    this.name = name
  }

  // 定义一个抽象方法
  // 不需要方法体，子类继承后必须实现该抽象方法
  abstract say(): void
}

class Dog extends Animal {
  constructor(name) {
    super(name)
  }

  say() {
    console.log()
  }
}

new Aniaml('Mike') // error
new Dog('Mike') // success
```

**implements** 使类满足接口要求

```ts
interface MyInter {
  name: string;
  say(): void;
}

class MyClass implements MyInter {
  name: string;
  say() {
    console.log()
  }
}
```

**public protected private getter setter** 类的属性修饰符

```ts
class Person {
  // 任意关系访问和修改
  public name: string;
  // 仅类及类的子类可访问修改
  protected like: string;
  // 仅类内部访问和修改
  private age: number;

  // 下边这种方式是定义加复制的简写
  // 等于 this.paramA = paramA
  constrctor(public paramA: string, protected paramB: string, private paramC: string) {
  }
  
  // new Person().name
  get name() {
    return this.name
  }

  // new Person().name = 'Mike'
  set name(val) {
    this.name = val
  }
}
```



## 内置工具类型

**Exclude<T,U>** 从T可分配给的类型中排除U

```ts
type Exclude<T, U> = T extends U ? never : T

type E = Exclude<string | number, string>
let e: E = 10 // 符合条件的只有number
```

**Extract<T,U>** 从T可分配给的类型中提取U

```ts
type Extract<T, U> = T extends U ? T : never

type E = Extract<string | number, string>
let e: E = "1" // 符合条件的只有string
```

**NonNullable<T>** 从T中排除null和undefined

```ts
type NonNullable<T> = T extends null | undefined ? never : T

type E = NonNullable<string | number | null | undefined>
let e: E = 1 // 符合条件的只有 string | number
```

**infer** 声明一个类型变量并且对它进行使用

```ts
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any
```

**ReturnType** 表示在extends条件语句中待推断的类型变量

```ts
type ReturnType<T extends (...args: any[]) => any> = T extends (
  ...args: any[]
) => infer R
  ? R
  : any

function getUserInfo() {
  return { name: "hello", age: 10 }
}
// 通过 ReturnType 将 getUserInfo 的返回值类型赋给了 UserInfo
type UserInfo = ReturnType<typeof getUserInfo>
const userA: UserInfo = {
  name: "hello",
  age: 10,
}
```

**Parameters** 获取函数类型的参数类型

```ts
type Parameters<T> = T extends (...args: infer R) => any ? R : any

type T0 = Parameters<() => string> // []
type T1 = Parameters<(s: string) => void> // [string]
type T2 = Parameters<<T>(arg: T) => T> // [unknown]
```

**Partial** 将传入的属性由非可选变为可选

```ts
type Partial<T> = { [P in keyof T]?: T[P] }

interface A {
  a1: string;
  a2: number;
  a3: boolean;
}
type aPartial = Partial<A>
const a: aPartial = {} // 不会报错
```

**Required** 将传入的属性中的可选项变为必选项

```ts
type Required<T> = { [P in keyof T]-?: T[P] }

interface Person {
  name: string;
  age: number;
  gender?: "male" | "female";
}
let p: Required<Person> = {
  name: "hello",
  age: 10,
  gender: "male",
}
```

**Readonly** 为传入的属性每一项都加上readonly修饰符来实现

```ts
type Readonly<T> = { readonly [P in keyof T]: T[P] }

interface Person {
  name: string;
  age: number;
  gender?: "male" | "female";
}
let p: Readonly<Person> = {
  name: "hello",
  age: 10,
  gender: "male",
}
p.age = 11 // error
```

**Pick<T,K>** 能够帮助我们从传入的属性中摘取某些返回

```ts
// 从T中选择一组属性K
type Pick<T, K extends keyof T> = { [P in K]: T[P] }

interface Todo {
  title: string;
  description: string;
  done: boolean;
}
type TodoBase = Pick<Todo, "title" | "done">;
type TodoBase = {
  title: string;
  done: boolean;
};
```

**Record<K,T>** K对应对象的key，T对应对象的value，返回的就是一个声明好的对象

```ts
type Record<K extends keyof any, T> = {
  [P in K]: T;
}

type Point = "x" | "y"
type PointList = Record<Point, { value: number }>
const cars: PointList = {
  x: { value: 10 },
  y: { value: 20 },
}
```

**Omit<K,T>** 剔除k中T的属性

```ts
type Omit=Pick<T,Exclude<keyof T,K>>

type User = {
  id: string;
  name: string;
  email: string;
}
type UserWithoutEmail = Omit<User, "email"> // UserWithoutEmail ={id: string;name: string;}

```
