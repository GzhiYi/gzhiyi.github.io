---
title: 稍微研究下Vuex
date: 2018-08-12 22:37:24
tags:
categories: 'Vue'
---
### 前言
最近都在用vue全家桶作页面，里面就用到了vuex作为状态管理。在过去用redux的时候，要写actions、reducer，还要给状态更改事件增加const，在开发的时候添加logger工具就可以很容易追踪状态的变化。当用vuex的时候，发现实现很容易，仅仅注入一下store就能搭建一个最简易的vuex。但是，终究还是没有深究以前redux的原理，原以为vuex比redux容易多的多，但是实际上，在你项目越来越大的时候，store就会越发复杂。所以，最后还是可能需要可以写成类似于Flux的形式，更加方便监管项目状态的去向。所以，我觉得学习下更多的vuex用法还是非常有用的，至少在状态管理的理解上会有所进步。学习记录的内容，有理解错必改！😄
<!--more-->

### 基本的Vuex
在store上：
```javascript
...
const store = new Vuex.Store({
  state: {
    name: 'GzhiYi'
  },
  mutations: {
    changeName (state, payload) {   // 使用payload可以让handler更加简洁，当然可以直接附带参数
      state.name = payload.newName  // 但是参数过多的时候就会让参数列表过长啦。
    }
  }
})
```
在组件上：
```javascript
...
const Demo = {
  template: `<div @click="changeName">{{name}}</div>`,
  methods: {
    changeName () {
      this.$store.commit({
        type: 'chaneName',
        newName: 'name changed!'
      })
    }
  },
  computed: {
    name () {
      return this.$store.state.name
    }
  }
}
```
1. 在项目根实例上注入store，就可以将store实例注入到所有的子组件中，在子组件内部就可以通过`this.$store.state.stateName`访问状态，可以通过`this.$store.commit('mutationName')`进行状态的修改。
2. 在组件中，不要直接通过`this.$store.state = newStateValue`的形式进行修改，需要通过commit一个mutation进行显式的修改。然后在组件上可以通过在computed函数内更新组件所用到的state值。

### Getter
getter在vuex有什么用呢？当不仅一个组件需要在调用mutaion要对状态进一步处理的时候，就可以用到getter。原本可以在每个组件的computed函数内处理做处理，但是你总不会想重复写那么代码吧。
```javascript
const store = new Vuex.Store({
  state: {
    num: 120
  },
  getters: {
    handleNum: state => { // 在调用这个mutation的时候处理一下num这个state
      return state.num > 100 ? state.num - 100 : state.num
    }
  }
})
```
```javascript
const Demo = {
  template: `<div>{{name}}</div>`,
  methods: {

  },
  computed: {
    name () {                                  // 直接调用getters的值就可以对应处理state并更新处理后的state。
      return this.$store.getters.handleNum     // 要注意getter和mutation达到的不是同一个结果。
    }
  }
}
```
以上是vuex最基本的用法，如果需要给项目的状态管理更加分明清晰以及了解更多的细节，可以继续往下看。

### Mutation
在第一个基本的vuex中已经用到mutaion使用对象的风格提交，以后可以多用这个方式提交变更。另外，mutation需要遵守vue的响应规则：
- 在store中要提前写好组件需要的state。
- 当需要给state对象添加新属性的时候，应该：
1. 使用`Vue.set(obj, newKey, newValue)`，或者看2
2. `state.obj = { ...state.obj, newKey: newValue }`

### 使用常量替代 Mutation 事件类型
用常量代替mutation事件类型，好处就是可以让mutaion非常清晰。这种方式在各种Flux实现中是很常见的，不是不需要，但不妨可以试试习惯这种做法方便团队合作。
```javascript
// mutation-types.js
export const MUTATION_NAME = 'MUTATION_NAME' // 值为string，可以按要求自定义。
```

```javascript
// store.js
import Vuex from 'vuex'
import { MUTATION_NAME } from './mutation-types'
const state = new Vuex.Store({
  state: {
    ...
  },
  [MUTATION_NAME] (state) {
    // 状态变更
  }
})
```

### Mutation 必须是同步函数
假设有如下的mutation
```javascript
mutations: {
  mutationName: (state) => {
    api.call(res => {     // 这里假设调用了api，并将返回的结果赋予state
      state.name = res 
    })
  }
}
```
mutation的这个api调用回调，也就是异步结果作为新的state赋予state，是不会完成的。`实际上，任何在回调中进行的状态更改都是无法追踪的。`但也要注意，虽说是不可追踪，但是异步的修改还是可以正常执行的。

### Action
action类似于mutation，但action提交的是mutation，而不是直接修改状态；action可以包含任意异步操作。
```javascript
const store = new Vuex.Store({
  state: {
    firstState: 'GzhiYi'
  },
  mutations: {
    changeName(state, payload) {
      state.firstState = payload.newName
    },
  },
  actions: {
    changeName (context, payload) {
      context.commit('increment', payload)
    }
  }
})
```
```javascript
// 在组件上
changeName () {
  this.$store.dispatch('changeName', {
    newName: 'GGG'
  })
}
```
这样看的话，actions已有的一个changeName函数，为啥还要费劲里面再设置一个mutation呢？在actions里面的每一个函数都支持异步操作，这才是重要的。
在actions中：
```javascript
actions: {
  changeName (context, paylaod) {
    setTimeout(() => {
      context.commit('changeName', payload)
    }, 5000)
  }
}
```
这里说一下这两种提交store更改的区别。理解下`不可追踪`。提交的mutation是异步函数，那么在devtool会直接看到触发的mutation。对于actions而言，是要等待异步结果到了后才会触发mutation。

### Module
应用比较大的时候，就需要考虑将store按模块分割。
```javascript
const moduleA = {
  state: { ... },
  mutations: { ... },
  ...
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  ...
}
const store = new Vue.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})
// 获取状态
this.$store.state.a
this.$store.state.b
```