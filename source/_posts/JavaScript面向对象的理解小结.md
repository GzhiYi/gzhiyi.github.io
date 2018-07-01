---
title: JavaScript面向对象的理解小结
date: 2018-06-27 18:00:17
tags: JavaScript
categories: JavaScript
---
### 理解对象
JavaScript的对象就是一组键值对，当中的值可以是任意合法的JavaScript数据类型。每个对象都是基于一个引用类型创建的。  
最常见的对象自定义方式：
```javascript
let obj = {
  name: 'GzhiYi',
  gender: 'F',
  sayName: function() {
    console.log(`I'm ${this.name}`)
  }
}
// 这些字段书上说是特征值，也就是key。
```

#### 属性类型
JavaScript有内部值，描述属性的各种特征。这些特征是为了实现JavaScript引擎用的，所以在JavaScript上是不能够直接访问的。  
为了表述特征是内部值，将这些值用两个方括号括着。比如`[[Enumerable]]`。JavaScript有两种属性：`数据属性`和`访问器属性`。
##### 数据属性
1. `[[ Configurable ]]` 默认true；能否delete删除属性从而定义属性；能否修改属性的特性；能否把属性修改为访问器属性。
2. `[[ Enumerable ]]` 默认true；能否通过`for...in`循环返回属性。
3. `[[ Writable ]]` 默认true; 能否修改属性的值。
4. `[[ Value ]]` 默认undefined；包含这个属性的数据值；读取属性的时候，从这个位置读，写入属性的时候，将值保存到这个位置。
那好，知道这个特性有什么作用呢？我们可以对一个对象进行属性的设置，包括他的属性能不能被改等，比如：
```javascript
let person = {
  name: 'gzhiyi'
}
Object.defineProperty(person, 'name', {
  writable: false,
  value: 'gzhiyi'
})
person.name = 'hello'
console.log(persion)  // {name: 'gzhiyi'}

// 不是在严格模式下，这种操作不会报错的，赋新值的话只是没写进去。
```
**注意:** 修改了对象的configurable属性为false的话会回不去的。不能再变为可配置！

##### 访问器属性
访问器属性没有数据值，有一个getter和一个setter函数，分别用于读取和写入属性。
1. `[[ Configurable ]]` 默认true；能否delete删除属性从而定义属性；能否修改属性的特性；能否把属性修改为数据属性。
2. `[[ Enumerable ]]` 默认true；能否通过`for...in`循环返回属性。
3. `[[ Get ]]`  默认undefined。读取属性的函数。
4. `[[ Set ]]`  默认undefined。写入属性的函数。
访问器不能直接被定义。
```javascript
let computer = {
  _price: 6999,
  discount: 1
}
Object.defineProperty(computer, 'price', {
  get: function() {
    return this._price
  },
  set: function(newValue) {
    if (newValue > this._price ) {
      this.discount = .9
      this._price = newValue * this.discount
    } else if (newValue > 10000) {
      this.discount = .8
      this._price = newValue * this.discount
    }
  }
})
computer.price = 10000
console.log(computer)  // {_price: 9000, discount: 0.9}
```

### 对象的创建
平时我们创建一个临时对象可以这么创建：
```javascript
let mathBook = {
  pages: 200,
  price: 88
}
```
这样的好处是方便快捷，随用随写。但这个方法有一个很明显的短板，那就是创建很多相同对象的时候，代码重复就很多。比如说创建一个englishBook对象，它也有一个页数和一个价格的属性，那就出现重复了！所以，出现了创建很多对象的方法：
#### 工厂模式
创建一个工厂函数用于创建属性类似的对象，可无限次使用。
```javascript
function createBook(pages, price) {
  let obj = new Object()
  obj.pages = pages
  obj.price = price
  return obj
}
let book1 = createBook(232， 333)  // 创建一个有232页，333元的书...
// 工厂模式解决了创建大量相似对象的问题，但没有解决对象的识别问题。
// 书上说是没有解决怎么知道一个对象的类型的问题。
```

#### 构造函数模式
```javascript
function Book(pages, price) {
  this.pages = pages
  this.price = price
  console.log('this', this)
  this.priceInPage = function() {
    console.log(this.price / this.pages)
  }
}
let book1 = new Book(121, 222)
book1.priceInPage()   // 1.834710743801653
// 直接利用一个Book的构造函数创建对象。
// 如果直接new一个Book带参数的对象，this就是Book这个object，如：new Book(232, 32)
// {pages: 232, price: 32,  priceInPage: f()}
**特点：**
```
- 没有显式地创建对象。
- 直接把属性和方法赋给this对象。
- 没有有return语句，也就说没有return一个object。
- 规范：构造函数用于创建对象，所以构造函数首字母大写。
- 当new一个Book的时候，有如下顺序：
1. 创建一个对象。
2. 将构造函数的作用域赋给新对象。this指向转为这个新对象。
3. 执行构造函数内的代码。
4. 返回新对象。
新创建的对象都有一个constructor属性，这个属性和Book构造函数相等：
`boo1.constructor === Book` // true

##### 将构造函数当作一个函数
**任何函数，只要用new操作符调用，那它就可以是构造函数。如果不用new操作符，就是普通的函数。**
```javascript
// 当构造函数
let newBook = new Book(100, 100)
newBook.priceInPage()  // 1

// 当普通函数，会将这个函数添加到window对象中
Book(100, 100)
window.priceInPage() // 1

// 在另一个对象的作用于调用
let obj = {}
Book.call(obj, 100, 100)
obj.priceInPage() // 1
```
如果当作普通的函数，可以将对象的函数添加到window对象中，这样可以在其他地方全局调用函数。

##### 构造函数缺点
构造函数如果内部存在函数的话，通过new操作符生成的对象的函数是不同的。
```javascript
boo1.priceInPage === book2.priceInPage // false。
```
**插入补充:** 判断函数或者查看函数内部结构等不需要带括号，记住带括号就是调用该函数。
也就是说，每次用构造函数创建对象的时候，总是悄悄的生成一个函数实例(priceInPage)
有解决办法：
```javascript
function Book(pages, price) {
  this.pages = pages
  this.price = price
  this.priceInPage = priceInPage
}
function priceInPage() {
  console.log(this.price / this.pages)
}
let book2 = new Book(123, 222)
let book3 = new Book(1222, 211)
console.log(book2.priceInPage === book3.priceInPage)  // true
```
看起来问题解决了。可是又有问题来了：在全局作用域定义的函数实际上只能被某个对象调用，这样全局作用域就没用了啊！
```javascript
priceInPage()  //NaN。直接调用的话返回NaN，也就是只能被通过构造函数创建的对象使用。
```
如果函数多起来，就有些多余了。  
所以，又出了个。。。👇

#### 原型模式
创建的每个函数都有个原型属性。(prototype)，这个属性是一个指针，指向一个对象。  
prototype是原型对象。通过new操作符调用构造函数创建的对象的原型对象。
```javascript
function Book() {}
Book.prototype.pages = 100
Book.prototype.price = 100
Book.prototype.priceInPage = function() {
  console.log(this.price / this.pages)
}
let book1 = new Book()
let book2 = new Book()
console.log(book1.priceInPage === book2.priceInPage)  // true。这就解决了上面说到的问题。
console.log(Book) // ƒ Book() {}
```
这一次，把Book的所有属性和方法都加到了Book的原型对象中，这个构造函数看起来还是空的。也就是说并不是加到Book这个对象的本身上。  
`Book.prototype就包含隐藏的属性和方法`
```javascript
console.log(Book.prototype)  // {pages: 100, price: 100, constructor: ƒ}
```

1. 理解原型对象
无论什么时候，创建函数都会在函数上创建一个prototype属性，这个属性指向函数的原型对象。默认情况下，所有原型对象都会自动获得一个constructor(构造函数)属性。该属性包含一个指向prototype属性所在的指针。
按照例子说，就是:
```javascript
console.log(Book.prototype.constructor === Book) // true
```
![prototype](http://p7b9iw239.bkt.clouddn.com/prototype.png)

属性的查找是利用查找对象属性的过程实现的。  
获取对象的一个属性-从对象实例搜索-原型对象-继续搜索，这样就可以搜索到需要的属性。按照例子，也就是这样：  
比如说`book1.price`，第一次会查book1本身有没有price这个属性，很明显，没有，返回`Book {}`，第二次会找原型`Book.prototype`，然后发现有了。  
虽然可以读取到原型的值，但是我们不能修改原型的值。可以理解为，能修改通过原型生成的对象的值，但是不能反着修改原型原本的值。
```javascript
book1.price = 9999
console.log(book1.price)  // 9999
console.log(book2.price)  // 100，在原型的值是不会被修改到的！
Book.prototype.price = 999
console.log(book1.price, book2.price) // 理所应当，修改原型的属性值将直接影响到实例属性
                                      // 的值。这样的操作要十分注意，不建议使用。
```
**注意:** 
`book1.hasOwnProperty('price')`这个函数是判断属性是在原型还是在实例属性上的，不能乱用，只能判断本身属性上有无。  
```javascript
book1.hasOwnProperty('price') // false，判断对象有没属性的时候要十分注意
book1.__proto__.hasOwnProperty('price') // true。实例的__proto__指向原型对象
book1.price = 1000
book1.hasOwnProperty('price') // true，改写了对象实例自身属性的时候才会返回ture
```

------

2. 原型与in操作符
对于对象，有两种方式使用`in`操作符：
- 单独使用。
- 在`for...in`中使用。
```javascript
function Book() {}  // 还是用这个原型创建对象的例子
Book.prototype.pages = 100
Book.prototype.price = 100
Book.prototype.priceInPage = function() {
  console.log(this.price / this.pages)
}
let book1 = new Book()
let book2 = new Book()
console.log(book1.hasOwnProperty('price'))  // false， 这个例子上面也出现了。
console.log('price' in book1) // ture，这个方法是返回true的。
// 无论属性是在实例本身中还是在原型中，如果有，就返回true
```
通过hasOwnProperty和in操作符的对比，我们可以思考一下，这两者如果结合那不就可以找到这个属性是在实例本身上还是在处于原型链"上层"的原型对象上了。
```javascript
function hasPrototypeProperty(obj, property) { // 属性是否在原型上？
  return !obj.hasOwnProperty(property) && (property in obj)
  // in只要对象有这个属性就返回true，无论属性是在实例本身上还是原型对象上。
  // hasOwnProperty只有存在实例本身上才返回ture。
}
// 该函数功能：属性只在原型上就返回ture，属性咋实例上就返回false
```

------


3. 遍历属性的方法
`Object.keys()`
例如：
```javascript
console.log(Object.keys(Book)) // ["priceInPage"]
console.log(Object.keys(Book.prototype)) // ["pages", "price"]
```

------


4. 原型创建对象的简化方法
```javascript
function Book() {}
Book.prototype = {
  price: 100,
  pages: 100,
  priceInPages: function() {
    console.log(this.price / this.pages)
  }
}
```
这个简化方法获得的结果跟之前是一样的，但还是有不同的地方。那就是原型的constructor不再指回Book了。
```javascript
原本的方法：
Book.prototype.constructor === Book // true
现在的方法：
Book.prototype.constructor // ƒ Object() { [native code] }，变成了Object啦。
Book.prototype.constructor = Book // 如果需要你可以赋值回去！
```

------


5. 原型的动态性
原型修改为另一个对象就切断了构造函数和最初原型的联系，实例的指针仅指向原型，而不指向构造函数。
```javascript
function Book() {}
let book1 = new Book()
Book.prototype = {
  price: 100,
  pages: 100,
  priceInPages: function() {
    console.log(this.price / this.pages)
  }
}
book1.price // undefined，引用的还是最初的原型
```

------


6. 原型对象的问题
最大的问题： 实例共享。  
对于基本数据类型，不同的实例可以单独一份属性值，但是对于引用数据类型的操作，将直接共享到各自的实例对象中。以下例子很明显：
```javascript
function Book() {}
Book.prototype = {
  price: 100,
  pages: 100,
  arr: ['first'],
  priceInPages: function() {
    console.log(this.price / this.pages)
  }
}
let book1 = new Book()
let book2 = new Book()
console.log(book1.price, book2.price) // 100， 100
book1.price = 1000
console.log(book1.price, book2.price) // 1000，100，基本数据类型book1的价格
                                      // 发生变化不影响book2的价格

console.log(book1.arr, book2.arr) // ['first'], ['first']
book1.arr.push('second')
console.log(book1.arr, book2.arr) // ['first', 'second'], ['first', 'second']
                                  // ，引用类型变化将改变所有实例变量的变化
```
就是因为这个问题，所以基本不会直接用原型模式创建对象。

#### 组合使用构造函数模式和原型模式
本模式是最常见的模式。我怎么没怎么看见过！  
构造函数模式用于定义实例属性，原型模式用于定义方法和共享的属性。好处就是每个实例都有一个自己实例属性的副本。同时又共享着对方法的引用，最大限制的节省了内存。另外这种模式还可以向构造函数传参。
```javascript
function Book(pages, price) {
  this.pages = pages
  this.price = price
  this.arr = ['first']
}
Book.prototype = {
  constructor: Book,
  priceInPages: function() {
    cosole.log(this.price / this.pages)
  }
}
let book1 = new Book(100, 100)
let book2 = new Book(200, 200)
book1.arr.push('second')
console.log(book1.arr, book2.arr) // ['first', 'second'], ['first']
```

#### 动态原型模式
动态原型模式把所有信息都封装到构造函数中，通过在构造函数中初始化原型，又保持了同时使用构造函数和原型的优点。
```javascript
function Book(pages, price) {
  this.pages = pages
  this.price = price
  if (typeof this.priceInPages !== 'function') {
    Book.prototype.priceInPages = function() {
      console.log(this.price / this.pages)
    }
  }
}
// 在priceInPages不存在的情况下才将这个函数添加到原型中
```