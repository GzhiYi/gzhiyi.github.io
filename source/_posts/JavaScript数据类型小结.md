---
title: JavaScript数据类型小结
date: 2018-06-20 09:27:39
tags: JavaScript
---
### 概要
JavaScript有五种基本数据类型，一种复杂数据类型。（ES6增加了Symbol）。
1. 五种基本数据类型：`Undefined`、`Null`、`Boolean`、`Number`、`String`。
2. 一种复杂数据类型：`Object`。

### 类型判断 typeof
typeof操作符返回的是一个string。
```javascript
  let data = x
  switch(typeof(data)) {
    case "undefined":
      break;
    case "boolean":
      break;
    case "string":
      break;
    case "array":
      break;
    case "function":
      break;
    case "object":
      break;
  }
```
**注意**
1. typeof不是函数，是一个操作符。
2. typeof(null) 返回的是"object"，null被认为是一个空的对象引用。
3. typeof一个正则表达式将返回"function"，也可能返回"object"。

### Undefined
- 未声明过的变量有一个值，那就是Undefined。  
- 判断值是否为undefined： `typeof(value === undefined) === 'boolean'` 为true。
- 所有的undefined对比均相等，比如定义两个没有值的变量a,b。则a === b 为true。  
- 未声明过的变量，有三种操作不会报错：
1. typeof判断其为'undefined'。
2. delete这个变量，严格模式下会报错。
3. 赋值。

### Null
- 这个数据类型也只有一个值，就是null。
- typeof(一个是null的变量)，Chrome返回的是"object"。
- 常常定义一个变量而还没有决定赋什么类型的值的时候，统一赋值为null。
  可以做如下判断有无其他值：
  ```javascript
  if (value !== null) {}
  ```
- undefined的值派生于null的值，所以在相等性判断下两者相等。
```javascript
null === undefined    // false ，所以写 === 的作用就有了
null == undefined     // true
```

### Boolean
也就是 true 还有 false
- ture不是1，false也不是0，这要搞清楚别弄错。
- Boolean转换类型为false的情况：
1. Boolean -> false
2. String  -> ''
3. Number  -> 0 和NaN
4. Object  -> null
5. Undefined -> undefined
- 条件判断内会自动将内容转为Boolean，所以要清楚被转换后的Boolean值是什么。 

### Number
- JavaScript有最大数和最小数，计算超过这个数值将之间返回`±Infinity`
1. 最大数： `Number.MAX_VALUE === 1.7976931348623157e+308`
2. 最小数： `Number.MIN_VALUE === 5e-324`
- NaN(Not a Number)，顾名思义，不是一个数值。在需要判断数值的地方可以避免抛出错误。例如1 / 0返回NaN。测试了下，返回的是Infinity。。。
- 有NaN的运算结果都为NaN，`NaN !== NaN`
- Number()进行类型转换：
```javascript
Number(true)         // 1
Number(1)            // 1
Number(undefined)    // NaN
Number(null)         // 0
Number('123')        // 123
Number('1.2')        // 1.2
Number(0xf)          // 10
Number('')           // 0
其他                 // NaN

```
3. parseInt()
在字符串的转换上，parseInt会比较好
```javascript
parseInt('12ab')        // 12
parseInt('')            // NaN
parseInt('0xA')         // 10
parseInt(12.5)          // 12
```

### String
1. 字符串均有一个length属性，为该字符串的长度。
2. null和undefined没有toString()。
3. toString()带一个数值参数说明要转换的进制数。
4. String()可以解决null和undefined转换出现的错误，也就是说String()可以轻松的使用。

### Object
小结不来，太多了。