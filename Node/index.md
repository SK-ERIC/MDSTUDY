解释下变量提升？✨ 

JavaScript引擎的⼯作⽅式是，先解析代码，获取所有被声明的变量，然后再⼀⾏⼀⾏地运⾏。这造成的结果，就是所 

有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。 

```js
console.log(a) // undefined 

var a = 1 

function b() { 

console.log(a) 

}

b() // 1 
```



上⾯的代码实际执⾏顺序是这样的: 

第⼀步： 引擎将 var a = 1 拆解为 var a = undefined 和 a = 1 ，并将 var a = undefined 放到最顶端， a = 1 还在原 

来的位置 

这样⼀来代码就是这样: 

var a = undefined 

console.log(a) // undefined 

a = 1 

function b() { 

console.log(a) 

}

b() // 1 

第⼆步就是执⾏，因此js引擎⼀⾏⼀⾏从上往下执⾏就造成了当前的结果，这就叫变量提升。 

原理详解请移步,预解释与变量提升 