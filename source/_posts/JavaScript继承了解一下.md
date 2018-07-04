---
title: JavaScript继承了解一下
date: 2018-07-01 17:21:59
tags: JavaScript
categories: JavaScript
---
### 简单的理解原型链继承
JavaScript通过原型链继承。
简单的原型链继承例子：
```javascript
// 定义一个父构造函数
function Parent() {
  this.parentName = 'Parent' 
}

// 父构造函数的原型有一个getParentName方法用于获取父名称
Parent.prototype.getParentName = function() {
  return this.parentName
}

// 定义一个子函数
function Child() {
  this.childName = 'Child'
}
// 这时候将父构造函数创造的实例赋值给子构造函数的原型。
// 这里说明下，new Parent()就是一个生成的实例对象。
Child.prototype = new Parent()

// 给子构造函数的原型添加一个获取子名称的函数。
Child.prototype.getChildName = function() {
  return this.childName
}

// 创建一个子实例person
let person = new Child()

// 这时候person这个子实例就有了子的属性，也有了父的属性了。
console.log(person.getParentName()) // 'Parent'
console.log(person.getChildName()) // 'Child'
```
以上就是原型链继承的简单例子。 

<!--more-->

另外，要注意继承后的几点：
1. 子实例的构造函数指向父构造函数。
```javascript
person.constructor === Parent // true
// 原因：子构造函数原型的consturctor属性被重写。
```
2. 继承后属性的搜索将继续往上爬，直到找到为止。
3. 无论怎么继承，都会继承默认的原型Object。所有引用类型都默认继承Object。
4. 有必要说一下什么是构造函数。不要怀疑，上面的Child()、Parent()就是一个构造函数。

#### 确定原型和实例的关系
可以通过`instanceof`操作符来确定关系。翻译过来就是，`左边的是不是右边的一个实例呢？`  
如上：  
```javascript
console.log(person instanceof Object) // true
console.log(person instanceof Child) // true
console.log(person instanceof Parent) // true
let personUnderParent = new Parent() // true
console.log(personUnderParent instanceof Object) // true
console.log(personUnderParent instanceof Child) // false
console.log(personUnderParent instanceof Parent) // true
```

#### 原型链继承的问题
主要的问题还是引用类型的问题。包含引用类型值的原型属性会被所有实例共享，你改动了原型中引用类型的值，所有生成出来的实例的那个值也会发生改变，这并不是我们想要的操作。所以我们要在构造函数中，不是在原型对象中定义属性。
```javascript
function Parent() {
  this.arr = ['first']
}
function Child() {}
Child.prototype = new Parent()
let person = new Child()
person.arr.push('second')
console.log(person.arr)

let person2 = new Child()
console.log(person2.arr)
// 结果可想而知，两个数组结果一样，都是['first', 'second']
```
`单单是这一个原因，就让原型链继承很少在代码上呈现。`

### 构造函数继承
在有问题的情况下，当然会有更优的实现继承的方法。
```javascript
function Parent() {
  this.word = ['is', 'Parent']
}
function Child() {
  Parent.call(this) // or apply
}
let person1 = new Child()
person1.word.push('?')
console.log(person.word) // ['is', 'Parent', '?']
let person2 = new Child()
console.log(person2.word) // ['is', 'Parent']
```

#### 参数传递
借用构造函数有一个很大的优势，就是可以在子类型构造函数中向父类型构造函数传递参数。
```javascript
function Parent(name) {
  this.name = name
}
function Child() {
  Parent.call(this, 'GzhiYi')
  this.age = 18
}
Child.prototype = new Parent()
let person = new Child()
console.log(person.name) // 'GzhiYi'
console.log(person.age) // 18
```
构造函数不利于函数复用，所以普遍上也不会用。

#### 组合继承（或许最优）
额，每次都是在旧知识之下进步进而出现更好的方法。组合继承是结合原型链和构造函数的一种继承方式。
```javascript
// 父构造函数设置
function Parent(name) {
  this.name = name
  this.word = ['is', 'Parent']
}
Parent.prototype.sayName = function() {
  console.log(this.name)
}

// 子构造函数设置
function Child(name, age) {
  Parent.call(this, name) // 调用父构造函数，将父的word属性继承过来了
  this.age = age
}
Child.prototype = new Parent()
Child.prototype.sayAge = function() {
  console.log(this.age)
}
let person1 = new Child('GzhiYi', 18)
person1.word.push('?')
console.log(perspon1.word) // ['is', 'Parent', '?']
person1.sayName() // 'GzhiYi'
person1.sayAge()  // 18

let person2 = new Child('Hello', 20)
console.log(person2.word) // ['is', 'Parent']
person2.sayName() // 'Hello'
person2.sayAge()  // 20
// 注意生成的实例都是子实例对象。有些方法子有父不一定有！
```
可谓相互独立，互不干扰。这才是继承的正解！