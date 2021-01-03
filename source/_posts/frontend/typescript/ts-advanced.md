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

function identity <T, U>(value: T, message: U) : [T, U] {
  return [value, message];
}
```


#### 3.2 泛型接口
```ts
interface Identities<V, M> {
  value: V,
  message: M
}

function identity<T, U> (value: T, message: U): Identities<T, U> {
  console.log(value + ": " + typeof (value));
  console.log(message + ": " + typeof (message));
  let identities: Identities<T, U> = {
    value,
    message
  };
  return identities;
}
console.log(identity(68, "Semlinker"));
```


#### 3.3 泛型类

在类中使用泛型也很简单，我们只需要在类名后面，使用 `<T, ...>` 的语法定义任意多个类型变量，具体示例如下：
```ts
interface GenericInterface<U> {
  value: U
  getIdentity: () => U
}

class IdentityClass<T> implements GenericInterface<T> {
  value: T

  constructor(value: T) {
    this.value = value
  }

  getIdentity(): T {
    return this.value
  }

}

const myNumberClass = new IdentityClass<Number>(68);
console.log(myNumberClass.getIdentity()); // 68

const myStringClass = new IdentityClass<string>("Semlinker!");
console.log(myStringClass.getIdentity()); // Semlinker!
```


#### 3.4 泛型约束
**确保属性存在**

有时我们可能希望限制每个类型变量接受的类型数量，这就是泛型约束的作用。
```ts
function identity<T>(arg: T): T {
  console.log(arg.length); // Error
  return arg;
}

// ==> 缺陷：除数组外会报错
interface Length {
  length: number;
}
function identity<T extends Length>(arg: T): T {
  console.log(arg.length); // 可以获取length属性
  return arg;
}
// ==> 明确是数组类型
function identity<T>(arg: T[]): T[] {
   console.log(arg.length);  
   return arg; 
}
// or
function identity<T>(arg: Array<T>): Array<T> {      
  console.log(arg.length);
  return arg; 
}
```

**检查对象上的键是否存在**

泛型约束的另一个常见的使用场景就是检查对象上的键是否存在。
```ts
interface Person {
  name: string;
  age: number;
  location: string;
}
type K1 = keyof Person; // "name" | "age" | "location"
type K2 = keyof Person[];  // number | "length" | "push" | "concat" | ...
type K3 = keyof { [x: string]: Person };  // string | number

// ==> keyof 结合 extends
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```


#### 3.5 泛型参数默认类型
```ts
interface A<T=string> {
  name: T;
}

const strA: A = { name: "Semlinker" };
const numB: A<number> = { name: 101 };
```


#### 3.6 泛型条件类型
```ts
T extends U ? X : Y
```
以上表达式的意思是：若 `T` 能够赋值给 `U`，那么类型是 `X`，否则为 `Y`。在条件类型表达式中，我们通常还会结合 `infer` 关键字，实现类型抽取：
```ts
interface Dictionary<T = any> {
  [key: string]: T;
}
 
type StrDict = Dictionary<string>
type DictMember<T> = T extends Dictionary<infer V> ? V : never
type StrDictMember = DictMember<StrDict> // string
```
在上面示例中，当类型 `T` 满足 `T extends Dictionary` 约束时，我们会使用 `infer` 关键字声明了一个类型变量 `V`，并返回该类型，否则返回 never 类型。

```ts
async function stringPromise() {
  return "Hello, Semlinker!";
}

interface Person {
  name: string;
  age: number;
}

async function personPromise() {
  return { name: "Semlinker", age: 30 } as Person;
}

type PromiseType<T> = (args: any[]) => Promise<T>;
type UnPromisify<T> = T extends PromiseType<infer U> ? U : never;

type extractStringPromise = UnPromisify<typeof stringPromise>; // string
type extractPersonPromise = UnPromisify<typeof personPromise>; // Person
```


### 四、泛型内置工具类型
#### Partial
Partial 作用是将传入的属性变为可选项.

`Partial<T>` 的作用就是将某个类型里的属性全部变为可选项 `?`。
```ts
/**
 * node_modules/typescript/lib/lib.es5.d.ts
 * Make all properties in T optional
 */
type Partial<T> = {
    [P in keyof T]?: T[P];
};

// 应用
interface Todo {
  title: string;
  description: string;
}
function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: "organize desk",
  description: "clear clutter"
};
const todo2 = updateTodo(todo1, {
  description: "throw out trash"
});

// ==> Partial<Todo>等价于
{
   title?: string | undefined;
   description?: string | undefined;
}
```
在以上代码中，首先通过 keyof T 拿到 T 的所有属性名，然后使用 in 进行遍历，将值赋给 P，最后通过 T[P] 取得相应的属性值。中间的 ? 号，用于将所有属性变为可选。


#### Required
`Required`的作用是将传入的属性变为必须项，源码如下：
```ts
type Required<T> = { [P in keyof T]-?: T[P] };
```
我们发现一个有意思的用法 `-?`, 这里很好理解就是将可选项代表的`?`去掉, 从而让这个类型变成必选项. 与之对应的还有个+`?`, 这个含义自然与`-?`之前相反, 它是用来把属性变成可选项的.


#### Readonly
将传入的属性变为只读选项，源码如下：
```ts
type Readonly<T> = { readonly [P in keyof T]: T[P] };
```


#### Mutable
未包含。

类似地, 其实还有对 `+` 和 `-`, 这里要说的不是变量的之间的进行加减而是对 `readonly` 进行加减.
以下代码的作用就是将 `T` 的所有属性的 `readonly` 移除,你也可以写一个相反的出来.
```ts
type Mutable<T> = {
  -readonly [P in keyof T]: T[P]
}
```


#### Record
`Record<K extends keyof any, T>` 的作用是将 `K` 中所有的属性的值转化为 `T` 类型。
```ts
/**
 * node_modules/typescript/lib/lib.es5.d.ts
 * Construct a type with a set of properties K of type T
 */
type Record<K extends keyof any, T> = {
    [P in K]: T;
};

// 应用
interface PageInfo {
  title: string;
}
type Page = "home" | "about" | "contact";

const x: Record<Page, PageInfo> = {
  about: { title: "about" },
  contact: { title: "contact" },
  home: { title: "home" }
};
```


#### Pick
`Pick<T, K extends keyof T>` 的作用是将某个类型中的子属性挑出来，变成包含这个类型部分属性的子类型。
```ts
// node_modules/typescript/lib/lib.es5.d.ts
/**
 * From T, pick a set of properties whose keys are in the union K
 */
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};

// 应用
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false
};
```


#### Exclude
`Exclude<T, U>` 的作用是将某个类型中属于另一个的类型移除掉。
```ts
// node_modules/typescript/lib/lib.es5.d.ts
/**
 * Exclude from T those types that are assignable to U
 * 如果 T 能赋值给 U 类型的话，那么就会返回 never 类型，否则返回 T 类型。最终实现的效果就是将 T 中某些属于 U 的类型移除掉。
 */
type Exclude<T, U> = T extends U ? never : T;

// 应用
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // "c"
type T2 = Exclude<string | number | (() => void), Function>; // string | number
```


#### Extract
`Extract`源码：
```ts
type Extract<T, U> = T extends U ? T : never;
```
根据源码我们推断出 `Extract` 的作用是提取出 `T `包含在 `U` 中的元素, 换种更加贴近语义的说法就是从 `T` 中提取出 `U`.


#### Omit
未包含.

用之前的 `Pick` 和 `Exclude` 进行组合, 实现忽略对象某些属性功能, 源码如下
```ts
type Omit<T, K> = Pick<T, Exclude<keyof T, K>>

// 使用
type Foo = Omit<{name: string, age: number}, 'name'> // -> { age: number }
```


#### ReturnType
`ReturnType<T>` 的作用是用于获取函数 T 的返回类型。
```ts
// node_modules/typescript/lib/lib.es5.d.ts
/**
 * Obtain the return type of a function type
 */
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;

// 应用
type T0 = ReturnType<() => string>; // string
type T1 = ReturnType<(s: string) => void>; // void
type T2 = ReturnType<<T>() => T>; // {}
type T3 = ReturnType<<T extends U, U extends number[]>() => T>; // number[]
type T4 = ReturnType<any>; // any
type T5 = ReturnType<never>; // any
type T6 = ReturnType<string>; // Error
type T7 = ReturnType<Function>; // Error
```



### 五、[装饰器](https://mp.weixin.qq.com/s?__biz=MzI2MjcxNTQ0Nw==&mid=2247484552&idx=1&sn=fe548e36a4fcda8e103ae6a5cb6cec41&chksm=ea47a5d0dd302cc6fef0c6eab2e585aed4563e90a97a21094ee00d871d9cddaa7a2ba881533c&scene=21#wechat_redirect)




<br>
<br>
<br>
