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


[TypeScript](https://www.tslang.cn/docs/home.html)


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


### [webpack & react](https://www.tslang.cn/docs/handbook/react-&-webpack.html)




### 基础类型

|                    名称                    |                    用法                    |                    等价于                    |
| ------------------------------------------ | ------------------------------------------ | ------------------------------------------- |
|       数组       |         xx: number[]         |              xx: Array<number>              |
|       元祖       |         xx: [string, number]         |              -              |
|       枚举       |         enum weekday{sun=7,mon=1,tue=2,wed=3,thu=4,fri=5,sat=6} day         |        day: weekday = weekday.mon        |
|       永不存在的值       |         function error(message: string): never {}         |        -        |
|       无返回值       |         function (): void；      |        -        |
|       类型断言       |         let xx: number = (someValue as string).length;      |        let xx: number = (<string>someValue).length;        |


### 接口 interface 
函数调用，对函数的参数有一定的要求
```
interface LabelledValue {
  label: string;
}
function printLabel(labelledObj: LabelledValue) {}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

可选属性
```
interface SquareConfig {
  color?: string;
  width?: number;
}
```

额外的属性检查
```
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

只读属性
```
interface Point {
    readonly x: number;
    readonly y: number;
}
```

函数类型
```
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

### 类
private 不能在声明它的类的外部访问
```
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

new Animal("Cat").name; // 错误: 'name' 是私有的.
```

protected 不能在包含它的类外被实例化但是能被继承

readonly 只读属性必须在声明时或构造函数里被初始化

public 静态属性














<br>
<br>
<br>
<br>
<br>