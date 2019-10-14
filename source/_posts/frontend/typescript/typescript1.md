---
title: TypeScript 基础应用
date: 2019-10-09
tags: 
    - TypeScript
categories: TypeScript
keywords: [TypeScript]
description: TypeScript
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://i.loli.net/2019/10/09/yfMzZmIrKNJGlc7.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

<!-- ![TypeScript](https://i.loli.net/2019/10/09/yfMzZmIrKNJGlc7.jpg) -->

### 一、安装TypeScript
#### 1.1 通过npm（Node.js包管理器）
```
    npm install -g typescript
```
#### 1.2 安装Visual Studio的TypeScript插件 `ms-vscode.vscode-typescript-tslint-plugin`
### 1.3 编译代码
```
    tsc xxx.ts
```
<br>

### 二、基础类型
#### 2.1 布尔值
```ts
    let isDone: boolean = false;
```
#### 2.2 数字
TypeScript里的所有数字都是浮点数, 除了支持十进制和十六进制字面量, 还支持二进制和八进制字面量。
```ts
    let hexLiteral: number = 0xf00d;
    let decLiteral: number = 6;
    let octalLiteral: number = 0o744;
    let binaryLiteral: number = 0b1010;
```
#### 2.3 字符串
```ts
    let myName: string
    myName = 'lousanpang'
```
#### 2.4 数组
元素类型 + [ ]
```ts
    let list: number[] = [1, 2, 3];
    let list: string[] = ['1', '2', '3'];
```
Array + <元素类型>
```ts
    let list: Array<any> = [1, 'data', true];
```
#### 2.5 元组 准确的数组
类型与值一一对应
```ts
    let Tuple: [string, number];

    Tuple = ['hello', 1]; // OK
    Tuple = [1, 'hello']; // Error

    Tuple[0].substr(1); // OK
    Tuple[1].substr(1); // Error
```
#### 2.6 枚举 `enum` 类型
`enum` 型可以为一组数值赋予友好的值。
```ts
    enum Obj {
        Red = 3, // 默认 0
        Green = 2 // 默认 1
    };

    let c: Obj = Obj.Green;
    console.log(c); // 2

    let d: string = Obj[3];
    console.log(d); // 'Red'

    let h: boolean = Obj[2];
    console.log(h); // Error h只能赋予 string 类型
```
#### 2.7 任意类型 `any`
```ts
    let notSure: any = 4;
    notSure = "maybe a string instead";
    notSure = false; // ok

    let list: any[] = [1, 'a', true];
```
#### 2.8 空值 `void`
当一个函数没有返回值时
```ts
    function warnUser(): void {
        console.log("This is my warning message");
    }

    let unusable: void = undefined;
    unusable = null;
```
#### 2.9 null 和 undefined
```ts
    let u: undefined = undefined;
    let n: null = null;
```
#### 2.9 never
表示的是那些永不存在的值的类型
返回never的函数必须存在无法达到的终点
```ts
    function error(message: string): never {
        throw new Error(message);
    }

    function infiniteLoop(): never {
        while (true) {
        }
    }
```
#### 2.10 object
object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。
```ts
    let o: object | null;
    o = {
        props: 0
    }
    o = null
```
#### 2.11 类型断言
告诉编译器，“相信我，我知道自己在干什么”。
  
第一种表示方法：“尖括号”语法。
```ts
    let sValue: any = 'hi';
    let sLength: number = (<string>sValue).length;
```

第二种表示方法：`as`语法。
你在TypeScript里使用JSX时，只有as语法断言是被允许的。
```ts
    let sValue: any = 'hi';
    let sLength: number = (sValue as string).length;
```
<br>

### 三、接口 interface 
```ts
    interface LabeledValue {
        label: string;
    }
    let labeledObj: LabeledValue;

    labeledObj.label = 'hi';
```












<br>
<br>
<br>
<br>
<br>