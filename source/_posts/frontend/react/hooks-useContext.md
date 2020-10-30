---
title: React Hooks - useContext
date: 2020-10-31
tags: 
    - React
categories: React
keywords: [Hooks]
description: React Hooks
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s1.ax1x.com/2020/10/30/BteaFJ.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

## [useContext](https://gaohaoyang.github.io/2020/05/12/react-hooks3-useContext/)

组件树结构如下，现在想从根节点传递一个 userName 的属性到叶子节点 A D F，通过 props 的方式传递，会不可避免的传递通过 B C E，即使这些组件也没有使用这个 userName 属性。

如果这样的嵌套树形结构有5层或10层，那么将是灾难式的开发维护体验。如果能不经过中间的节点直接到达需要的地方就可以避免这种问题，这时 Context api 就是来解决这个问题的。

[![组件树](https://s1.ax1x.com/2020/10/30/BtF5QO.png)](https://imgchr.com/i/BtF5QO)


## class语法使用Context

**在根节点中使用 Provider 包裹子节点，将 context 提供给子节点**

```ts
import React from 'react'

import './App.css'

import ComponentC from './components/16ComponentC'

export const UserContext = React.createContext('')

const App = () => {
  return (
    <div className="App">
      <UserContext.Provider value={'chuanshi'}>
        <ComponentC />
      </UserContext.Provider>
    </div>
  )
}

export default App
```

**在使用的节点处消费 Context**

```ts
import React from 'react'

import { UserContext } from '../App'

function ComponentF() {
  return (
    <div>
      <UserContext.Consumer>
        {
          (user) => (
            <div>
              User context value {user}
            </div>
          )
        }
      </UserContext.Consumer>
    </div>
  )
}

export default ComponentF
```

**多个 Context 情况**

```ts
import React from 'react'

import './App.css'

import ComponentC from './components/16ComponentC'

export const UserContext = React.createContext('')
export const ChannelContext = React.createContext('')

const App = () => {
  return (
    <div className="App">
      <UserContext.Provider value={'chuanshi'}>
        <ChannelContext.Provider value={'code volution'}>
          <ComponentC />
        </ChannelContext.Provider>
      </UserContext.Provider>
    </div>
  )
}

export default App
```

**消费它们**

```ts
import React from 'react'

import { UserContext, ChannelContext } from '../App'

function ComponentF() {
  return (
    <div>
      <UserContext.Consumer>
        {
          (user) => (
            <ChannelContext.Consumer>
              {
                (channel) => (
                  <div>
                    User context value {user}, channel value {channel}
                  </div>
                )
              }
            </ChannelContext.Consumer>

          )
        }
      </UserContext.Consumer>
    </div>
  )
}

export default ComponentF
```


## hooks语法使用Context

**通过 useContext 使用根节点创建的 Context**

1. 从 react 对象中 import useContext 这个 hook api
2. import 根节点创建的 Context 对象（可以导入多个）
1. 执行 `useContext()` 方法，将 Context 传入

```ts
import React, { useContext } from 'react'

import ComponentF from './16ComponentF'
import {UserContext, ChannelContext} from '../App'

function ComponentE() {
  const user = useContext(UserContext)
  const channel = useContext(ChannelContext)
  return (
    <div>
      <ComponentF />
      --- <br/>
      {user} - {channel}
    </div>
  )
}

export default ComponentE
```


## 小结

`useContext` 方法接收一个 context 对象（`React.createContext` 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 `<MyContext.Provider>` 的 value prop 决定。

当组件上层最近的 `<MyContext.Provider>` 更新时，该 Hook 会触发重渲染，并使用最新传递给 `MyContext provider` 的 `context value` 值。即使祖先使用 `React.memo` 或 `shouldComponentUpdate`，也会在组件本身使用 `useContext` 时重新渲染。

可以理解为，`useContext(MyContext)` 相当于 class 组件中的 `static contextType = MyContext` 或者 `<MyContext.Consumer>`。

`useContext(MyContext)` 只是让你能够读取 context 的值以及订阅 context 的变化。你仍然需要在上层组件树中使用 `<MyContext.Provider>` 来为下层组件提供 context。


<br>
<br>
<br>
