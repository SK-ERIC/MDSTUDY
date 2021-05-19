## 浏览器

## html

## css

## JavaScript

### JS 基础

- 谈谈你对原型链的理解？ ✨
- 如何判断是否是数组？
- ES6 模块与 CommonJS 模块有什么区别？
- 聊⼀聊如何在 JavaScript 中实现不可变对象？
- JavaScript 的参数是按照什么⽅式传递的？
- js 有哪些类型? 为什么会有 BigInt 的提案？
- null 与 undefined 的区别是什么？
- 0.1+0.2 为什么不等于 0.3？
- 类型转换的规则有哪些？
- 类型转换的原理是什么？

### JS 机制

#### 解释下变量提升？✨

JavaScript 引擎的⼯作⽅式是，先解析代码，获取所有被声明的变量，然后再⼀⾏⼀⾏地运⾏。这造成的结果，就是所 有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。

```js
  console.log(a) // undefined
  var a = 1 function b() {
    console.log(a)
  }
  b() // 1
```

上⾯的代码实际执⾏顺序是这样的: 第⼀步： 引擎将 `var a = 1` 拆解为 `var a = undefined` 和 `a = 1` ，并将 `var a = undefined` 放到最顶端， `a = 1` 还在原 来的位置 这样⼀来代码就是这样: 
```js
  var a = undefined 
  console.log(a) // undefined a = 1 
  function b() { 
    console.log(a) 
  }
  b() // 1 
```
第⼆步就是执⾏，因此 js 引擎⼀⾏⼀⾏从上往下执⾏就造成了当前的结果，这就叫变量提升。 
> 原理详解请移步,预解释与变量提升

- ⼀段 JavaScript 代码是如何执⾏的？✨
- JavaScript 的作⽤域链理解吗？✨
- 谈⼀谈你对 this 的了解？✨
- 箭头函数的 this 指向哪⾥？✨
- 理解闭包吗？

### JS 内存

- 讲讲 JavaScript 垃圾回收是怎么做的？
- JavaScript 的基本类型和复杂类型是储存在哪⾥的？

### 异步

- async/await 是什么？
- async/await 相⽐于 Promise 的优势？

## http

## React
