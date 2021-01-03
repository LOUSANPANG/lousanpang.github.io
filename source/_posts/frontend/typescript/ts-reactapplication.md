---
title: TypeScript系列之React开发应用
date: 2020-09-29
tags: 
    - TypeScript
categories: TypeScript
keywords: [TypeScript]
description: TypeScript系列之React开发应用
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s3.ax1x.com/2021/01/03/s9lWaq.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## React
### 一、相关类型
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

### 二、函数式组件
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

### 三、Hooks

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
