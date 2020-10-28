---
title: React Hooks - useEffect
date: 2020-10-30
tags: 
    - React
categories: React
keywords: [Hooks]
description: React Hooks
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://s1.ax1x.com/2020/10/28/B13nQP.png # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---

## [useEffect](https://gaohaoyang.github.io/2020/05/11/react-hooks2-useEffect/)


### 1.1 **useEffect 解决的问题**
- 可以认为是 componentDidMount, componentDidUpdate, componentWillUnmount 的替代品

### 1.2 **只执行一次 useEffect**
```ts
// class

import React, { Component } from 'react'

class RunEffectsOnlyOnce extends Component {
  state = {
    x: 0,
    y: 0
  }

  logMousePos = (e: MouseEvent) => {
    this.setState({
      x: e.clientX,
      y: e.clientY
    })
  }

  componentDidMount() {
    document.addEventListener('mousemove', this.logMousePos)
  }

  render() {
    return (
      <div>
        Y - {this.state.y}, X - {this.state.x}
      </div>
    )
  }
}

export default RunEffectsOnlyOnce
```

```ts
import React, { useState, useEffect } from 'react'

function RunEffectsOnlyOnce() {

  const [x, setX] = useState(0)
  const [y, setY] = useState(0)

  const logMousePos = (e: MouseEvent) => {
    setX(e.clientX)
    setY(e.clientY)
  }

  useEffect(() => {
    console.log('addEventListener')
    document.addEventListener('mousemove', logMousePos)
  }, [])

  return (
    <div>
      Y - {y}, X - {x}
    </div>
  )
}

export default RunEffectsOnlyOnce
```

- 注意到 useEffect 方法的第二个参数传入一个空数组，有效的避免了多次调用的问题
- 如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（[]）作为第二个参数。这就告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。这并不属于特殊情况 —— 它依然遵循依赖数组的工作方式。
- 如果你传入了一个空数组（[]），effect 内部的 props 和 state 就会一直拥有其初始值。尽管传入 [] 作为第二个参数更接近大家更熟悉的 componentDidMount 和 componentWillUnmount 思维模式，但我们有更好的方式来避免过于频繁的重复调用 effect。除此之外，请记得 React 会等待浏览器完成画面渲染之后才会延迟调用 useEffect，因此会使得额外操作很方便。

### 1.3 **有条件地执行 useEffect**
```ts
import React, { Component } from 'react'

interface stateType {
  count: number
  name: string
}

class ClassCounter extends Component {

  state = {
    count: 0,
    name: '',
  }

  componentDidMount() {
    document.title = `${this.state.count} times`
  }

  componentDidUpdate(prevProps: any, prevState: stateType) {
    if (prevState.count !== this.state.count) {
      console.log('Update title')
      document.title = `${this.state.count} times`
    }
  }

  render() {
    return (
      <div>
        <input
          type="text"
          value={this.state.name}
          onChange={(e) => {
            this.setState({
              name: e.target.value
            })
          }}
        />
        <button onClick={() => {
          this.setState({
            count: this.state.count + 1
          })
        }}>
          Clicked {this.state.count} times
        </button>
      </div>
    )
  }
}

export default ClassCounter
```

```ts
import React, { useState, useEffect } from 'react'

function HookCounter() {
  const [count, setCount] = useState(0)
  const [name, setName] = useState('')

  useEffect(() => {
    console.log('useEffect - update title')
    document.title = `You clicked ${count} times`
  }, [count])

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => {
          setName(e.target.value)
        }}
      />
      <button onClick={() => {
        setCount(prevCount => prevCount + 1)
      }} >Clicked {count} times</button>
    </div>
  )
}

export default HookCounter
```

- 注意到 useEffect 的第二个参数 [count]，这个参数是一个数组，元素是要被观察的 state 或 props，只有指定的这个变量发生变化时，才会触发 useEffect 中的第一个参数匿名函数的执行。这有利于性能的保证。

### 1.4 **需要清除的 Effect**
- 如何实现 willUnmount 这个生命周期，实现组件销毁时，清除 effect 逻辑。

```ts
componentWillUnmount() {
  document.removeEventListener('mousemove', this.logMousePos)
}
```

```ts
  useEffect(() => {
    console.log('addEventListener')
    document.addEventListener('mousemove', logMousePos)
    return () => {
      document.removeEventListener('mousemove', logMousePos)
    }
  }, [])
```

- 在 useEffect 的第一个参数中添加一个 return 匿名函数，这个匿名函数将在组件卸载的时候执行，因此我们在这里移除监听就好了。
- 如果需要一些在组件卸载时清除功能的代码，就写在 useEffect 第一个参数的返回匿名函数中吧。

### 1.5 注意到的问题：**useEffect 中依赖错误导致的 bug**

```ts
/**
 * 每秒 +1 的计数器
 */

import React, { Component } from 'react'

class Counter extends Component {

  state = {
    count: 0
  }

  timer: number | undefined

  tick = () => {
    this.setState({
      count: this.state.count + 1
    })
  }

  componentDidMount() {
    this.timer = window.setInterval(this.tick, 1000)
  }

  componentWillUnmount() {
    clearInterval(this.timer)
  }


  render() {
    return (
      <div>
        <span>{this.state.count}</span>
      </div>
    )
  }
}

export default Counter
```

下边的hooks代码错误 
```ts
import React, { useState, useEffect } from 'react'

function IntervalCouterHooks() {

  const [count, setCount] = useState(0)

  const tick = () => {
    setCount(count + 1)
  }

  useEffect(() => {
    const interval = setInterval(tick, 1000)
    return () => {
      clearInterval(interval)
    }
  }, [])

  return (
    <div>
      {count}
    </div>
  )
}

export default IntervalCouterHooks
```

- 传入空的依赖数组 []，意味着该 hook 只在组件挂载时运行一次，并非重新渲染时。但如此会有问题，在 setInterval 的回调中，count 的值不会发生变化。因为当 effect 执行时，我们会创建一个闭包，并将 count 的值被保存在该闭包当中，且初值为 0。每隔一秒，回调就会执行 setCount(0 + 1)，因此，count 永远不会超过 1。
- 解法一：这里我们不能将 useEffect 的第二个参数设置为空数组，而是 [count]。指定 [count] 作为依赖列表就能修复这个 Bug，但会导致每次改变发生时定时器都被重置。事实上，每个 setInterval 在被清除前（类似于 setTimeout）都会调用一次。但这并不是我们想要的。要解决这个问题，我们可以使用 setState 的函数式更新形式。它允许我们指定 state 该如何改变而不用引用当前 state
- 解法二：将
```ts
setCount(count + 1)
```
- 改为
```ts
setCount((preCount) =>  preCount + 1)
```

### 1.6 小结
useEffect api 的用法，第一个参数为匿名函数，作为 effect 要执行的内容。第二个参数为数组，用于观察改变的 props 或 state 进行有条件的触发 effect，或者传入空数组，让 effect 只执行一次。useEffect 返回一个匿名函数，在组件销毁是执行，可以有效避免内存泄露的风险。

<br>
<br>
<br>
