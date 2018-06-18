---
title: Vue组件的使用以及数据传递
date: 2018-06-01 12:43:15
tags:
categories: Vue
---
## Vue
由于项目需求，最近学习了一点Vue，官方文档也基本上又刷了遍，在有React组件化和函数编程的基础之下，文档看起来也是很易懂。之后会补充Vue踩的坑或者说是Vue编写过程中的一些思想等，也可以简单记录一下学习的过程。这一篇就概要的写一下Vue组件的使用和组件之间的数据传递。
## 组件
Vue的组件通常独立在一个文件中，这和React是类似的。一个组件常常就包含:  
```html
<template></template>      <!-- 组件html代码 -->
```
```javascript
<script></script>          // JavaScript
```
```css
<style></style>            /* 样式处理 */
```
## 组件写法以及父组件向子组件传递数据
那怎么写一个组件呢？这里以一个index.vue引入一个child-component组件为例。很明显，这里的父组件就是index，子组件就是child-component。  
定义index.vue
```html
<template>
  <div class="container">
    <child-component :data="message" :array="array"></child-component>
  </div>
</template>
<script>
  import childComponent from './childComponent.vue'

  export default {
    components: {
      childComponent,
    },
    data: function() {
      return {
        message: '传到子组件的消息',
        array: ['1', 2, {}]
        // 设置默认数据
      }
    },
    ...
  }
</script>
<style>
  ...略
</style>
```
子组件childComponent.vue
```html
<template>
  <div>this is a child component</div>
</template>
<script>
  export default {
    name: 'child-component',
    props: ['data', 'array'],
    data: function() {
      return {
        receivedData: this.data,
        receivedArray: this.array
      }
    }
  }
</script>
```
通过上面的例子就可以很容易的构造自己想要的组件了。其中有几点是需要注意的：
1. template需要有个“根节点”包括所有写的组件代码。
2. Vue官方建议组件的命名方式为`kebab Case`，如果写成React那种形式也是可以的。
3. 数据传递需要bind data，左边的值提供给子组件用于获取，右边的值为本组件定义的值。
4. 组件引入写法不可以使用`kebab Case`，引用需要转成`camel case`。

## 子组件向父组件传递数据
以上写了组件的定义以及引用，也体现了父组件如何向子组件传递，那反过来，子组件怎么向父组件传递数据呢？  
这个跟React也是一样的，大致的思路就是在子组件上调用父组件的方法，在子组件将需要的数据返回到父组件中。  
子组件:
```html
<template>
  <div>
    <button v-model="childMsg" @click="transfer">传递</button>
  </div>
  <script>
    ...
    data: function() {
      return {
        childMsg: 'message: child -> parent'
      }
    },
    methods: {
      tranfer: function() {
        this.$emit('transferToParent', this.childMsg)
      }
    }
  </script>
</template>
```
父组件:
```html
<template>
  <div>
    <child-component 
      @transferToParent="handleTransfer"
    ></child-component>
  </div>
</template>

<script>
  ...
  methods: {
    handleTransfer: function(res) {
      console.log(res)
    }
  }
</script>
```

兄弟组件之间的数据传递？  
思路：先跟爸爸说。
## 总结
其实Vue和React组件的写法上有很多类似的地方，他们的思路都很相似，只是在代码的体现上不一样。在Vue上，也是在拿到数据之后设置为一个初始的data。在React上也可以设置，也可以直接this.props进行调用。在熟悉了Vue的语法之后，觉得写起来还是很轻松的。