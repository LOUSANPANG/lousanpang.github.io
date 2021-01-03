---
title: TypeScript开发应用
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
如何为对象动态分配属性
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
基础定义
```ts
type Dxx = {
    // 常见的函数类型
    handleClick: (id: number) => void;
    // 另一种声明方式
    handleClick(event: React.MouseEvent<HTMLButtonElement>): void;
    // 可选参数类型
    optional?: OptionalType;
}
```

函数重载
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


### 二、React
#### 2.1 相关类型
```ts
export declare interface AppProps {
    children: React.ReactNode;
    // 返回react节点的函数
    functionChildren: (name: string) => React.ReactNode;
    // 在内联style时使用
    style?: React.CSSProperties;
    // button props
    buttonProps: React.ComponentProps<'button'>;
    // React 的按钮点击事件类型
    onClickButton: React.ComponentProps<‘button">[’onClick‘];
}
```

#### 2.2 函数式组件
```ts
// 基础
interface AppProps = { message: string };
const App = ({ message }: AppProps) => <div>{message}</div>
```

包含`children`的，利用`React.FC`内置类型的话不光会包含你定义的`AppProps`还会自动加上一个children类型，以及其他组件上会出现的类型
```ts
AppProps & { 
  children: React.ReactNode 
  propTypes?: WeakValidationMap<P>;
  contextTypes?: ValidationMap<any>;
  defaultProps?: Partial<P>;
  displayName?: string;
}

interface AppProps = { message: string };
const App: React.FC<AppProps> = ({ message, children }) => {
  return (
    <>
     {children}
     <div>{message}</div>
    </>
  )
};
```

#### 2.3 Hooks

useState
```ts
// 如果你的默认值已经可以说明类型，那么不用手动声明类型，交给 TS 自动推断即
const [staLoad, setLoad] = React.useState(false);
// 如果初始值是 null 或 undefined，那就要通过泛型手动传入你期望的类型
const [user, setUser] = React.useState<IUser | null>(null);
```

useReducer
```ts
const initialState = { count: 0 };

type ACTIONTYPE =
  | { type: "increment"; payload: number }
  | { type: "decrement"; payload: string };

function reducer(state: typeof initialState, action: ACTIONTYPE) {
  switch (action.type) {
    case "increment":
      return { count: state.count + action.payload };
    case "decrement":
      return { count: state.count - Number(action.payload) };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = React.useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "decrement", payload: "5" })}>
        -
      </button>
      <button onClick={() => dispatch({ type: "increment", payload: 5 })}>
        +
      </button>
    </>
  );
}
```

useEffect

useRef
```ts
const ref1 = useRef<HTMLElement>(null);

// 赋值之后是有值的
// null! 这种语法是非空断言，跟在一个值后面表示你断定它是有值的
const ref1 = useRef<HTMLElement>(null!);

// 应用
function Use() {
  const listRef = useRef<{ scrollToTop(): void }>(null!)

  useEffect(() => {
    listRef.current.scrollToTop()
  }, [])

  return (
    <List innerRef={listRef} />
  )
}
```

自定义Hook
```ts
export function useLoading() {
  const [isLoading, setState] = React.useState(false);
  const load = (aPromise: Promise<any>) => {
    setState(true);
    return aPromise.finally(() => setState(false));
  };
  // 加了 as const 会推断出 [boolean, typeof load]
  return [isLoading, load] as const;[]
}
```


<br>
<br>
<br>
