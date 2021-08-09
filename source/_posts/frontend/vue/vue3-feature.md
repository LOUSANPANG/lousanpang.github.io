---
title: Vue3.0新特性
date: 2021-8-9
tags: 
    - Vue
categories: Vue
keywords: [Vue3]
description: Vue3.0新特性
top_img: # 除非特定需要，可以不写
comments: # 是否显示评论 除非设置false,可以不写
cover: https://z3.ax1x.com/2021/08/09/f3r81I.jpg # 缩略图
toc: # 章节目录 除非特定文章设置，可以不写
toc_number: # 是否显示toc数字 除非特定文章设置，可以不写
copyright: # 是否显示版权 除非特定文章设置，可以不写
---


## 目录
Vue3新增的功能和特性

- Performance：性能优化
- Tree-shaking support：支持摇树优化
- Composition API：组合API
- Fragment Teleport Suspense：新增的组件
- Better TypeScript support：更好的TypeScript支持
- Custom Renderer API：自定义渲染器

在性能方面，对比Vue2.x，性能提升了1.3～2倍左右；打包体积也更小。


## Tree-shaking
在vue2.x版本中，很多函数都挂载在全局Vue对象上，比如nextTick、set等函数，虽然可能用不到，但打包时只要引入这些全局函数仍然会打包进bundle中。

而在Vue3中，所有的API都通过ES6模块化的方式引入，这样就能让webpack或rollup等打包工具在打包时对没有用到的API进行剔除，最小化bundle体积。

```js
// src/main.js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

const appp = createApp(APP)
app.use(router).mount('#app')
```

创建app实例方式从原来的new Vue()变为通过createApp函数进行创建；不过一些核心的功能比如virtualDOM更新算法和响应式系统无论如何都是会被打包的；这样带来的变化就是以前在全局配置的组件（Vue.component）、指令（Vue.directive）、混入（Vue.mixin）和插件（Vue.use）等变为直接挂载在实例上的方法；我们通过创建的实例来调用，带来的好处就是一个应用可以有多个Vue实例，不同实例之间的配置也不会相互影响：

```js
const app = createApp(App)
app.use(/* ... */)
app.mixin(/* ... */)
app.component(/* ... */)
app.directive(/* ... */)
```

因此Vue2.x的以下全局API也需要改为ES6模块化引入：

- Vue.nextTick
- Vue.observable不再支持，改为reactive
- Vue.version
- Vue.compile (仅全构建)
- Vue.set (仅兼容构建)
- Vue.delete (仅兼容构建)

除此之外，vuex和vue-router也都使用了Tree-Shaking进行了改进，不过api的语法改动不大：

```js
//src/store/index.js
import { createStore } from 'vuex'
export default createStore({
  state: {},
  mutations: {},
  actions: {},
  modules: {},
})

//src/router/index.js
import { createRouter, createWebHistory } from 'vue-router'
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
})
```


## 生命周期函数
在Vue2.x中有8个生命周期函数:

- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeDestroy
- destroyed

在vue3中，新增了一个setup生命周期函数，setup执行的时机是在beforeCreate生命函数之前执行，因此在这个函数中是不能通过this来获取实例的；同时为了命名的统一，将beforeDestroy改名为beforeUnmount，destroyed改名为unmounted，因此vue3有以下生命周期函数：

- beforeCreate（建议使用setup代替）
- created（建议使用setup代替）
- setup
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeUnmount
- unmounted

同时，vue3新增了生命周期钩子，我们可以通过在生命周期函数前加on来访问组件的生命周期，我们可以使用以下生命周期钩子：

- onMounted
- onBeforeUpdate
- onUpdated
- onBeforeUnmount
- onUnmounted
- onErrorCaptured
- onRenderTracked
- onRenderTriggered

```js
import { onBeforeMount, onMounted } from 'vue'
export default {
  setup() {
    onBeforeMount(() => {})
    onMounted(() => {})
  }
}
```


## 新增的功能
### 响应式API
使用reactive来为JS对象创建响应式状态：

```js
import { reactive } from 'vue'
const user = reactive({
  name: 'Vue2',
  age: 18
})
user.name = 'Vue3'
```

reactive相当于Vue2.x中的Vue.observable。

reactive函数只接收object和array等复杂数据类型。

对于一些基本数据类型，比如字符串和数值等，我们想要让它变成响应式，我们当然也可以通过reactive函数创建对象的方式，但是Vue3提供了另一个函数ref：

```js
import { ref } from 'vue'
const num = ref(0)
const str = ref('')
const male = ref(true)

num.value++
console.log(num.value)

str.value = 'new val'
console.log(str.value)

male.value = false
console.log(male.value)
```

ref返回的响应式对象是只包含一个名为value参数的RefImpl对象，在js中获取和修改都是通过它的value属性；但是在模板中被渲染时，自动展开内部的值，因此不需要在模板中追加.value。

```html
<template>
  <div>
    <span>{{ count }}</span>
    <button @click="count ++">Increment count</button>
  </div>
</template>

<script>
  import { ref } from 'vue'
  export default {
    setup() {
      const count = ref(0)
      return {
        count
      }
    }
  }
</script>
```

reactive主要负责复杂数据结构，而ref主要处理基本数据结构；但是ref本身也是能处理对象和数组：

```js
import { ref } from 'vue'

const obj = ref({
  name: 'qwe',
  age: 1,
})
setTimeout(() => {
  obj.value.name = 'asd'
}, 1000)

const list = ref([1, 2, 3, 4, 6])
setTimeout(() => {
  list.value.push(7)
}, 2000)
```

当我们处理一些大型响应式对象的property时，我们很希望使用ES6的解构来获取我们想要的值：

```js
let book = reactive({
  name: 'Learn Vue',
  year: 2020,
  title: 'Chapter one'
})
let {
  name,
} = book

name = 'new Learn'
console.log(book.name) // Learn Vue
```

很遗憾，这样会消除它的响应式；对于这种情况，我们可以将响应式对象转换为一组ref，这些ref将保留与源对象的响应式关联：

```js
let book = reactive({
  name: 'Learn Vue',
  year: 2020,
  title: 'Chapter one'
})
let { name } = toRefs(book)

// 注意这里解构出来的name是ref对象
// 需要通过value来取值赋值
name.value = 'new Learn'
console.log(book.name) // new Learn
```

对于一些只读数据，我们希望防止它发生任何改变，可以通过readonly来创建一个只读的对象：

```js
import { reactive, readonly } from 'vue'
let book = reactive({
  name: 'Learn Vue',
  year: 2020,
  title: 'Chapter one'
})

const copy = readonly(book)
copy.name = 'new copy' // Set operation on key "name" failed: target is readonly.
```

有时我们需要的值依赖于其他值的状态，在vue2.x中我们使用computed函数来进行计算属性，在vue3中将computed功能进行了抽离，它接受一个getter函数，并为getter返回的值创建了一个「不可变」的响应式ref对象：

```js
const num = ref(0)
const double = computed(() => num.value * 2)
num.value++

console.log(double.value) // 2
double.value = 4 // Warning: computed value is readonly
```

或者我们也可以使用get和set函数创建一个可读写的ref对象：

```js
const num = ref(0)
const double = computed({
  get: () => num.value * 2,
  set: (val) => (num.value = val / 2)
})

num.value++
console.log(double.value) // 2

double.value = 8
console.log(num.value) // 4
```

### 响应式侦听
和computed相对应的就是watch，computed是多对一的关系，而watch则是一对多的关系；vue3也提供了两个函数来侦听数据源的变化：watch和watchEffect。

我们先来看下watch，它的用法和组件的watch选项用法完全相同，它需要监听某个数据源，然后执行具体的回调函数，我们首先看下它监听单个数据源的用法：

```js
import { reactive, ref, watch } from 'vue'

const state = reactive({ count: 0 })
//侦听时返回值得getter函数
watch(
  () => state.count,
  (count, prevCount) => {
    console.log(count, prevCount) // 1 0
  }
)
state.count++

const count = ref(0)
//直接侦听ref
watch(count, (count, prevCount) => {
  console.log(count, prevCount) // 2 0
})
count.value = 2
```

我们也可以把多个值放在一个数组中进行侦听，最后的值也以数组形式返回：

```js
const state = reactive({ count: 1 })
const count = ref(2)
watch([() => state.count, count], (newVal, oldVal) => {
  //[3, 2]  [1, 2]
  //[3, 4]  [3, 2]
  console.log(newVal, oldVal)
})

state.count = 3
count.value = 4
```

如果我们来侦听一个深度嵌套的对象属性变化时，需要设置deep:true：

```js
const deepObj = reactive({
  a: {
    b: {
      c: 'hello'
    }
  }
})

watch(
  () => deepObj,
  (val, old) => {
    console.log(val.a.b.c, old.a.b.c) // new hello new hello
  },
  { deep: true }
)

deepObj.a.b.c = 'new hello'
```

最后的打印结果可以发现都是改变后的值，这是因为侦听一个响应式对象始终返回该对象的引用，因此我们需要对值进行深拷贝：

```js
import _ from 'lodash'
const deepObj = reactive({
  a: {
    b: {
      c: "hello"
    }
  }
})

watch(
  () => _.cloneDeep(deepObj),
  (val, old) => {
    // new hello new hello
    console.log(val.a.b.c, old.a.b.c)
  }
  { deep: true }
)

deepObj.a.b.c = "new hello"
```

一般侦听都会在组件销毁时自动停止，但是有时候我们想在组件销毁前手动的方式进行停止，可以调用watch返回的stop函数进行停止：

```js
const count = ref(0)

const stop = watch(count, (count, prevCount) => {
  // 不执行
  console.log(count, prevCount)
})

setTimeout(()=>{
  count.value = 2
}, 1000)
// 停止watch
stop()
```

还有一个函数watchEffect也可以用来进行侦听，但是都已经有watch了，这个watchEffect和watch有什么区别呢？他们的用法主要有以下几点不同：

- watchEffect不需要手动传入依赖
- 每次初始化时watchEffect都会执行一次回调函数来自动获取依赖
- watchEffect无法获取到原值，只能得到变化后的值

```js
import { reactive, ref, watch, watchEffect } from 'vue'

const count = ref(0)
const state = reactive({ year: 2021 })

watchEffect(() => {
  console.log(count.value)
  console.log(state.year)
})
setInterval(() => {
  count.value++
  state.year++
}, 1000)
```

watchEffect会在页面加载时自动执行一次，追踪响应式依赖；在加载后定时器每隔1s执行时，watchEffect都会监听到数据的变化自动执行，每次执行都是获取到变化后的值。


## 组合API

Composition API（组合API）也是Vue3中最重要的一个功能了，之前的2.x版本采用的是Options API（选项API），即官方定义好了写法：data、computed、methods，需要在哪里写就在哪里写，这样带来的问题就是随着功能增加，代码也越来复杂，我们看代码需要上下反复横跳：

![composition](https://mmbiz.qpic.cn/mmbiz_png/VsDWOHv25bcwNwbJCS06elhpTjmHfrzEt4ibAsRBO7IE2uCbHC9OtuJRrkD73yvGg1LPeh4pFpf9YWw5gsJ2R2w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

之前Options API的写法：

```js
export default {
  components: {},
  data() {},
  computed: {},
  watch: {},
  mounted() {},
}
```

Options API就是将同一类型的东西放在同一个选项中，当我们的数据比较少的时候，这样的组织方式是比较清晰的；但是随着数据增多，我们维护的功能点会涉及到多个data和methods，但是我们无法感知哪些data和methods是需要涉及到的，经常需要来回切换查找，甚至是需要理解其他功能的逻辑，这也导致了组件难以理解和阅读。

而Composition API做的就是把同一功能的代码放到一起维护，这样我们需要维护一个功能点的时候，不用去关心其他的逻辑，只关注当前的功能；Composition API通过setup选项来组织代码:

```js
export default {
  setup(props, context) {}
};
```

我们看到这里它接收了两个参数props和context，props就是父组件传入的一些数据，context是一个上下文对象，是从2.x暴露出来的一些属性：
- attrs
- slots
- emit

> 注：props的数据也需要通过toRefs解构，否则响应式数据会失效。

setup具体的用法:

```html
<template>
  <div>{{ state.count }} * 2 = {{ double }}</div>
  <div>{{ num }}</div>
  <div @click="add">Add</div>
</template>

<script>
import { reactive, computed, ref } from "vue"
export default {
  name: "Button",
  setup() {
    const state = reactive({ count: 1 })
    const num = ref(2)

    function add() {
      state.count++
      num.value += 10
    }

    const double = computed(() => state.count * 2)

    return {
      state,
      double,
      num,
      add
    }
  }
}
</script>
```

我们可以将setup中的功能进行提取分割成一个一个独立函数，每个函数还可以在不同的组件中进行逻辑复用：

```js
export default {
  setup() {
    const { networkState } = useNetworkState()
    const { user } = userDeatil()
    const { list } = tableData()
    return {
      networkState,
      user,
      list
    }
  }
}

function useNetworkState() {}
function userDeatil() {}
function tableData() {}
```


## Fragment
所谓的Fragment，就是片段；在vue2.x中，要求每个模板必须有一个根节点，所以我们代码要这样写：

```html
<template>
  <div>
    <span></span>
    <span></span>
  </div>
</template>
```

或者在Vue2.x中还可以引入vue-fragments库，用一个虚拟的fragment代替div；在React中，解决方法是通过的一个React.Fragment标签创建一个虚拟元素；在Vue3中我们可以直接不需要根节点：

```html
<template>
    <span>hello</span>
    <span>world</span>
</template>
```


## Teleport
Teleport翻译过来就是传送、远距离传送的意思；顾名思义，它可以将插槽中的元素或者组件传送到页面的其他位置：

在React中可以通过createPortal函数来创建需要传送的节点；本来尤大大想起名叫Portal，但是H5原生的Portal标签也在计划中，虽然有一些安全问题，但是为了避免重名，因此改成Teleport。

Teleport一个常见的使用场景，就是在一些嵌套比较深的组件来转移模态框的位置。虽然在逻辑上模态框是属于该组件的，但是在样式和DOM结构上，嵌套层级后较深后不利于进行维护（z-index等问题）；因此我们需要将其进行剥离出来：

```html
<template>
  <button @click="showDialog = true">打开模态框</button>

  <teleport to="body">
    <div class="modal" v-if="showDialog" style="position: fixed">
      我是一个模态框
      <button @click="showDialog = false">关闭</button>
      <child-component :msg="msg"></child-component>
    </div>
  </teleport>
</template>

<script>
export default {
  data() {
    return {
      showDialog: false,
      msg: "hello"
    }
  }
}
</script>
```

这里的Teleport中的modal div就被传送到了body的底部；虽然在不同的地方进行渲染，但是Teleport中的元素和组件还是属于父组件的逻辑子组件，还是可以和父组件进行数据通信。Teleport接收两个参数to和disabled：
- to - string：必须是有效的查询选择器或 HTMLElement，可以id或者class选择器等。
- disabled - boolean：如果是true表示禁用teleport的功能，其插槽内容将不会移动到任何位置，默认false不禁用。


## Suspense
Suspense是Vue3推出的一个内置组件，它允许我们的程序在等待异步组件时渲染一些后备的内容，可以让我们创建一个平滑的用户体验；Vue中加载异步组件其实在Vue2.x中已经有了，我们用的vue-router中加载的路由组件其实也是一个异步组件：

```js
export default {
  name: "Home",
  components: {
    AsyncButton: () => import("../components/AsyncButton")
  }
}
```

在Vue3中重新定义，异步组件需要通过defineAsyncComponent来进行显示的定义：

```js
// 全局定义异步组件
//src/main.js
import { defineAsyncComponent } from "vue"
const AsyncButton = defineAsyncComponent(() =>
  import("./components/AsyncButton.vue")
)
app.component("AsyncButton", AsyncButton)


// 组件内定义异步组件
// src/views/Home.vue
import { defineAsyncComponent } from "vue"
export default {
  components: {
    AsyncButton: defineAsyncComponent(() =>
      import("../components/AsyncButton")
    )
  }
}
```

同时对异步组件的可以进行更精细的管理:

```js
export default {
  components: {
    AsyncButton: defineAsyncComponent({
      delay: 100,
      timeout: 3000,
      loader: () => import("../components/AsyncButton"),
      errorComponent: ErrorComponent,
      onError(error, retry, fail, attempts) {
        if (attempts <= 3) {
          retry()
        } else {
          fail()
        }
      }
    })
  }
}
```

这样我们对异步组件加载情况就能掌控，在加载失败也能重新加载或者展示异常的状态;

它主要是在组件加载时渲染一些后备的内容，它提供了两个slot插槽，一个default默认，一个fallback加载中的状态：

```html
<template>
  <div>
    <button @click="showButton">展示异步组件</button>
    <template v-if="isShowButton">

      <Suspense>
        <template #default>
          <AsyncButton></AsyncButton>
        </template>

        <template #fallback>
          <div>组件加载中...</div>
        </template>
      </Suspense>

    </template>
  </div>
</template>

<script>
export default {
  setup() {
    const isShowButton = ref(false)
    function showButton() {
      isShowButton.value = true
    }
    return {
      isShowButton,
      showButton
    };
}
</script>
```


## data、mixin和filter
在Vue2.x中，我们可以定义data为object或者function，但是我们知道在组件中如果data是object的话会出现数据互相影响，因为object是引用数据类型；

在Vue3中，data只接受function类型，通过function返回对象；同时Mixin的合并行为也发生了改变，当mixin和基类中data合并时，会执行浅拷贝合并：

```js
const Mixin = {
  data() {
    return {
      user: {
        name: 'Jack',
        id: 1,
        address: {
          prov: 2,
          city: 3,
        },
      }
    }
  }
}
const Component = {
  mixins: [Mixin],
  data() {
    return {
      user: {
        id: 2,
        address: {
          prov: 4,
        },
      }
    }
  }
}

// vue2结果：
{
  id: 2,
  name: 'Jack',
  address: {
    prov: 4,
    city: 3
  }
}

// vue3结果：
user: {
  id: 2,
  address: {
    prov: 4,
  },
}
```

我们看到最后合并的结果，vue2.x会进行深拷贝，对data中的数据向下深入合并拷贝；而vue3只进行浅层拷贝，对data中数据发现已存在就不合并拷贝。

在vue2.x中，我们还可以通过过滤器filter来处理一些文本内容的展示：

```html
<template>
  <div>{{ status | statusText }}</div>
</template>
<script>
  export default {
    props: {
      status: {
        type: Number,
        default: 1
      }
    },
    filters: {
      statusText(value){
        if(value === 1){
          return '订单未下单'
        } else if(value === 2){
          return '订单待支付'
        } else if(value === 3){
          return '订单已完成'
        }
      }
    }
  }
</script>
```

最常见的就是处理一些订单的文案展示等；然而在vue3中，过滤器filter已经删除，不再支持了，官方建议使用方法调用或者计算属性computed来进行代替。

> 过滤器filter已经删除!


## v-model
在Vue2.x中，v-model相当于绑定value属性和input事件，它本质也是一个语法糖：

```html
<child-component v-model="msg"></child-component>
<!-- 相当于 -->
<child-component :value="msg" @input="msg=$event"></child-component>
```

在某些情况下，我们需要对多个值进行双向绑定，其他的值就需要显示的使用回调函数来改变了：

```html
<child-component 
    v-model="msg" 
    :msg1="msg1" 
    @change1="msg1=$event"
    :msg2="msg2" 
    @change2="msg2=$event">
</child-component>
```

在vue2.3.0+版本引入了.sync修饰符，其本质也是语法糖，是在组件上绑定@update:propName回调，语法更简洁：

```html
<child-component 
    :msg1.sync="msg1" 
    :msg2.sync="msg2">
</child-component>

<!-- 相当于 -->

<child-component 
    :msg1="msg1" 
    @update:msg1="msg1=$event"
    :msg2="msg2"
    @update:msg2="msg2=$event">
</child-component>
```

 Vue3中将v-model和.sync进行了功能的整合，抛弃了.sync，表示：多个双向绑定value值直接用多个v-model传就好了；同时也将v-model默认传的prop名称由value改成了modelValue：

 ```html
<child-component 
    v-model="msg">
</child-component>

<!-- 相当于 -->
<child-component 
  :modelValue="msg"
  @update:modelValue="msg = $event">
</child-component>
 ```

如果我们想通过v-model传递多个值，可以将一个argument传递给v-model：

```html
<child-component 
    v-model.msg1="msg1"
    v-model.msg2="msg2">
</child-component>

<!-- 相当于 -->
<child-component 
    :msg1="msg1" 
    @update:msg1="msg1=$event"
    :msg2="msg2"
    @update:msg2="msg2=$event">
</child-component>
```


## v-for和key

在Vue2.x中，我们都知道v-for每次循环都需要给每个子节点一个唯一的key，还不能绑定在template标签上，

```html
<template v-for="item in list">
  <div :key="item.id">...</div>
  <span :key="item.id">...</span>
</template>
```

而在Vue3中，key值应该被放置在template标签上，这样我们就不用为每个子节点设一遍：

```html
<template v-for="item in list" :key="item.id">
  <div>...</div>
  <span>...</span>
</template>
```


## v-bind合并

在vue2.x中，如果一个元素同时定义了v-bind="object"和一个相同的单独的属性，那么这个单独的属性会覆盖object中的绑定：

```html
<div id="red" v-bind="{ id: 'blue' }"></div>
<div v-bind="{ id: 'blue' }" id="red"></div>

<!-- 最后结果都相同 -->
<div id="red"></div>
```

然而在vue3中，如果一个元素同时定义了v-bind="object"和一个相同的单独的属性，那么声明绑定的顺序决定了最后的结果（后者覆盖前者）：

```html
<!-- template -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- result -->
<div id="blue"></div>

<!-- template -->
<div v-bind="{ id: 'blue' }" id="red"></div>
<!-- result -->
<div id="red"></div>
```


## v-for中ref

vue2.x中，在v-for上使用ref属性，通过this.$refs会得到一个数组：

```html
<template
  <div v-for="item in list" :ref="setItemRef"></div>
</template>
<script>
export default {
  data(){
    list: [1, 2]
  },
  mounted () {
    // [div, div]
    console.log(this.$refs.setItemRef) 
  }
}
</script>
```

但是这样可能不是我们想要的结果；因此vue3不再自动创建数组，而是将ref的处理方式变为了函数，该函数默认传入该节点：

```html
<template>
  <div v-for="item in 3" :ref="setItemRef"></div>
</template>

<script>
import { reactive, onUpdated } from 'vue'
export default {
  setup() {
    let itemRefs = reactive([])

    const setItemRef = el => {
      itemRefs.push(el)
    }

    onUpdated(() => {
      console.log(itemRefs)
    })

    return {
      itemRefs,
      setItemRef
    }
  }
}
</script>
```


## v-for和v-if优先级
在vue2.x中，在一个元素上同时使用v-for和v-if，v-for有更高的优先级，因此在vue2.x中做性能优化，有一个重要的点就是v-for和v-if不能放在同一个元素上。

而在vue3中，v-if比v-for有更高的优先级。因此下面的代码，在vue2.x中能正常运行，但是在vue3中v-if生效时并没有item变量，因此会报错：

```html
<template>
  <div v-for="item in list" v-if="item % 2 === 0" :key="item">{{ item }}</div>
</template>

<script>
export default {
  data() {
    return {
      list: [1, 2, 3, 4, 5]
    }
  }
}
</script>
```

> v-if比v-for有更高的优先级!


<br>
<br>
<br>
