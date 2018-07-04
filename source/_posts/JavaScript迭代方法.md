---
title: JavaScript迭代方法
date: 2018-06-22 10:34:20
tags: JavaScript
categories: JavaScript
---
### JavaScript迭代
JavaScript迭代方法有几个，每个的作用都不尽相同，对于业务写的多的，很有必要总结下各个迭代方法的不同点。先看看几个迭代方法的"硬描述"。
**数组每一项都过一个给定的函数**
- every()             如果每一项计算结果都是true，函数结果返回就是ture。
- filter()            结果将返回一个包含true的项组成的数组。
- forEach()           这个函数没有返回值。
- map()               返回每次函数调用的结果组成的数组。
- some()              如果该函数有一项返回true，则该函数返回true。

<!--more-->

### 举例
做再多也不如写下demo看看异同点。

#### every()
```javascript
let array = [1, 2, 3, 4, 5]
let result = array.every((item, index) => {
  return true
})
console.log(result)      // true
// 如果不在每一趟都返回true的话，遍历将会在返回第一个false的时候中断，且reuslt的值为false
// 适用于判断一个数组里面的是否都满足一个判断性条件。比如说一个数组内是否都为正数等。
```

#### filter()
```javascript
let array = [1, 2, 3, 4, 5]
let result = array.filter((item, index) => {
  return item % 2 === 0
})
console.log(result)      // [2, 4]
// 筛选出满足return中为true的值，结果是一个数组
// 适用于处理一个数组，因为结果返回的是筛选出来的数组
```

#### forEach()
```javascript
let array = [1, 2, 3, 4, 5]
let result = array.forEach((item, index) => {
  console.log(item)
})
console.log(result)      // undefined 注意没有返回值返回
// 适合只需要做数组遍历并在内部做业务处理
```

#### map
```javascript
let array = [1, 2, 3, 4, 5]
let result = array.map((item, index) => {
  return item + index
})
console.log(result)      // [1, 3, 5, 7, 9] result依赖于map内部的每次遍历的返回值
// 适用于遍历数组，如有需要将结果生成至一个新的数组上
```

#### some()
```javascript
let array = [false, false, false, false, true]
let result = array.some((item, index) => {
  return item
})
console.log(result)      // true，有true返回的话，result就为true
// 用的少，整体思路是数组内部有ture或者经过遍历内部有返回true的，结果都为true
```

#### reduce() 补充
```javascript
let array = [1, 2, 3, 4, 5]
let result = array.reduce((pre, cur, index) => {
  return pre + cur
})
console.log(result)   // 15
// 这一个迭代函数有些特殊，它最后返回一个值。同时参数也比较特殊，分别是上一个返回值pre，当前值cur，下标等。
// 当然，稍微想一下，这个遍历的话到底是怎么开始的呢，我们在return语句前加一行代码打印值。
console.log(pre, cur, index)
// 结果：
// 1 2 1
// 3 3 2
// 6 4 3
//10 5 4
// 从上面可以看出，index为0是不会打印出来的，直接从index 为1开始计算，整体遍历次数为数组长度 - 1，虽说如此，但数组每一项都进行了操作。
```
**小结**
以上迭代方法都不会更改原数组的值，请您放心使用。  
重点是，每个迭代的核心就是迭代，能够依次地检索数组的每一项，在业务逻辑处理中的使用也取决用你需要怎样的返回，值得注意的点就是如果你要用这几个函数的话，就要分清这个方法返回的到底是什么。如果只是简单的遍历，那都是可以用的。