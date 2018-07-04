---
title: JavaScript基本类型和引用类型
date: 2018-06-21 12:14:23
tags: JavaScript
categories: JavaScript
---
### 前言
JavaScript变量有可能是两种不同的类型的值，既可以是基本类型，也可以是引用类型。对于这一块的知识还比较薄弱，有必要以摘录和理解的形式写下来。
- 基本类型：一些简单的数据段。`按值访问`，也就是可以直接操作他实体值。
- 引用数据类型： 由多个值构成的对象。`按引用访问`，保存在内存中，操作的是引用而不是实际的对象。
- 这两种不同类型的值，在操作的时候有很大的不同，引用类型可以为其添加属性，而基本类型则不能实时添加。

<!--more-->

```javascript
let a = 1
a.name = 'GzhiYi'                 // 不报错，但不会写入值

let obj = {}
obj.name = 'GzhiYi'               // 在打印obj的时候就可以看到obj包含一个name属性
```
### 复制变量值
复制变量值这样的操作平时就算写的多，但不去理解的话，终究还是不清楚他们之间的差异的。这里简单理解一下他们不同的地方。

#### 基本数据类型
```javascript
let value = 2
let anotherValue = value
anotherValue = 5                 // 改变anotherValue的值
console.log(value)               // 2，原有值还是没变
```
> 上面的操作是很基本的赋值操作，将value的值赋予anotherValue。可以准确的说是将value的值复制一份给anotherValue，这复制一份可以看成是一个副本，副本的一个用处就是隔离原本值和副本值，也就是说修改副本值也不会改变原来value的值。

#### 引用数据类型
```javascript
let obj = {
  name: 'GzhiYi'
}
let anotherObj = obj            // {name: 'GzhiYi'}
anotherObj.name = 'who'         // 改变name属性值
console.log(obj.name)           // {name: 'who'}
```
> 一样的操作，这次是将一个对象赋予另一个对象，这种复制的操作将会按引用操作。通俗地说就是引用这个东西，一旦改了，就会改掉所有引用的变量的值。

### 传递参数
**所有的函数传参都是按值传递的**
#### 基本数据类型
```javascript
let value = 10
changeValue = funValue => {
  funValue = 20
}
changeValue(value)
console.log(value)              // 10
```
参数是value的一个副本，基本类型的副本修改不会影响原有的数据

#### 引用数据类型
```javascript
let obj = {
  name: 'GzhiYi'
}
changeObj = funObj => {
  funObj.name = 'who'
}
changeObj(obj)
console.log(obj)               // {name: 'who'}
```
原本的对象name属性值也被改了
**要注意**
看到上面两个例子，对比一下，是不是跟这个小标题有出入，引用类型通过函数传参进行更改，连外面原来的对象也修改了。这不就说明了对于引用类型函数传参不就是引用传递吗？
#### 所有的函数传参都是按值传递的
那就看看引用类型函数传参是不是引用传递的。依旧是上面的例子。
```javascript
let obj = {
  name: 'GzhiYi'
}
changeObj = funObj => {
  funObj.name = 'who'
  funObj = {}
  funObj.name = 'me'
}
changeObj(obj)
console.log(obj)               // {name: 'who'}
```
你会发现，结果也一样。funObj如果是按引用操作的，应该会改变obj中name的值的。  
你可以这么理解，引用类型当参数传递到函数内部的时候，传递的值其实是对象的地址值，也就是指针。
函数内部重写funObj的时候，这个变量引用就是一个局部对象了。这个局部对象会在函数执行之后被马上销毁。

### 检测类型
检测基本数据类型，typeof可以直接用。但是哪来检测引用对象类型的时候，返回的都是object了。这时候就要用`instanceof`操作符。

#### instanceof 操作符
基本用法
```javascript
let obj = {}
obj instanceof Object         // true，obj 是 Object吗？
```