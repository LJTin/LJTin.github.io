---
title: When to use var vs let vs const in JavaScript
date: 2019-01-21 22:45:06
tags: 
---


本文参考

https://medium.freecodecamp.org/var-vs-let-vs-const-in-javascript-2954ae48c037


在这篇文章中，你将会在javascript（ES6）中学习两种新的方式创建变量，let和const。在此过程中，我们将看到var，let和const的不同之处，以及包括诸如函数与块作用域、变量提升和不变性等主题

ES2015（或者ES6）介绍了两种新的方式创建变量，let和const。但是在我们真正区分var,let和const不同点之前，你应该先了解一些前提条件。它们是变量声明vs初始化，作用域（特别是函数作用域），和变量提升


# 变量声明vs初始化


一个变量声明介绍了一个新的标识符

```
var declaration
```


上面我们创建了一个新的标识符叫做声明。在javascript中，当变量被创建，它们以undefined的值被初始化。这意味着，如果我们尝试打印声明的变量，我们将得到undefined

```
var declaration
console.log(declaration)
```


所以如果我们打印声明的变量，我们得到undefined


与变量声明相反，变量初始化是指当你首次给一个变量赋值

```
var declaration
console.log(declaration) // undefined
declaration = 'This is an initialization'
```


所以在这里我们通过赋值给已经声明当变量一个字符串来初始化它。

这就引导我们第二个概念了，作用域

# Scope(作用域)


作用域定义了在你的程序里，变量和函数在哪里可以获取到。在JavaScript中，有两种类型的作用域--全局作用域和函数作用域，根据官方的规范


如果变量语句发生在函数声明中，则变量在这个函数中通过函数局部作用域来定义


这就意味着如果你使用var来创建一个函数，那么这个变量被运用于它所创建的函数，并且只能在该函数内部或者任何嵌套函数内部被访问。

```
function getDate () {
  var date = new Date()
  return date
}
getDate()
console.log(date) // ❌ Reference Error
```


上面我们尝试获取一个被声明的函数之外的变量。因为date是作用域于getDate的函数，它只能被getDate它本身或者getDate中的任何嵌套函数所获取（如下所示）
```
function getDate(){
    var date = new Date()
    function formatDate(){
        return date.toDateString().slice(4) //可获取
    }
    return formatDate()
}
getDate()
console.log(date) // Reference Error 引用错误
```


现在让我们来看一个更高级的例子。假设我们有一个prices的数组，并且我们需要一个函数，这个函数接收一个像折扣的数组，并且返回一个折扣后的新数组。最后的目标可能看起来像这样：

```
discountPrices([100, 200, 300], .5)
```


并且实现可能看起来像这样：
```
function discountPrices (prices, discount) {
  var discounted = []  for (var i = 0; i < prices.length; i++) {
    var discountedPrice = prices[i] * (1 - discount)
    var finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  
  return discounted
}
```


好像很简单，但是这与块作用域有什么关系？看一下这个for循环，它声明的变量是否可以在它外部被访问呢？事实证明，是可以的。
```
function discountPrices (prices, discount) {
  var discounted = []  
  for (var i = 0; i < prices.length; i++) {
    var discountedPrice = prices[i] * (1 - discount)
    var finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  
  console.log(i) // 3
  console.log(discountedPrice) // 150
  console.log(finalPrice) // 150  
  return discounted
}
```

如果JavaScript是你所了解的唯一一个编程语言，你可能不会想什么。然而，如果你从另一门编程语言（特别是这门语言是封闭的作用域）开始使用JavaScript，那么你很可能对这发生对事情感到有点担心。


它没有真正的破碎，他只是有点怪异。在for循环之外，这没有一个理由仍然能够访问到i，discountedPrice，和finalPrice。它不会对我们有任何的好处，并且甚至可能在一定程度上对我们造成伤害。然而，因为用var声明的变量是函数作用域的，所以你可以这样做。


现在我们已经可以讨论过变量声明，初始化，和作用域，那么在我们深入了解let和const之前我们需要清除的最后一件事就是提升。

# Hoisting（提升）


记得早的时候我们说过，‘在JavaScript中，变量被创建时可以用undefined值来初始化。事实证明，这就是“Hoisting”的全部。JavaScript解析器在被“创建”的阶段将分配一个默认的值undefined给变量的声明。

> For a much more in depth guide on the Creation Phase, Hoisting, and Scopes see “The Ultimate Guide to Hoisting, Scopes, and Closures in JavaScript”.

寻找更多更深入关于创建阶段，提升和作用域的指南，可以参阅 The Ultimate Guide to Hoisting, Scopes, and Closures in JavaScript


让我们看一下之前的例子和提升是如何影响它的
```
function discountPrices (prices, discount) {
  var discounted = undefined
  var i = undefined
  var discountedPrice = undefined
  var finalPrice = undefined  
      discounted = []
  for (var i = 0; i < prices.length; i++) {
    discountedPrice = prices[i] * (1 - discount)
    finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  } 
  console.log(i) // 3
  console.log(discountedPrice) // 150
  console.log(finalPrice) // 150  
  return discounted
}
```

注意所有声明的变量都被分配一个默认的值undefined。这就是为什么如果你在变量被真正声明之前访问其中一个变量，你仅仅得到undefined
```
function discountPrices (prices, discount) {
  console.log(discounted) // undefined  
  var discounted = []  
  for (var i = 0; i < prices.length; i++) {
    var discountedPrice = prices[i] * (1 - discount)
    var finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  
  console.log(i) // 3
  console.log(discountedPrice) // 150
  console.log(finalPrice) // 150  
  return discounted
}
```

现在你已经知道了有关var的每一件事，让我们最后讨论你为什么在这的重点：var，let和const的区别在哪里？

# var VS let VS const

首先，让我们比较var和let。var和let主要的不同点不是函数作用域，let是块作用域。

这就意味着一个变量用let关键字创建可以被“块”内部以及任何嵌套的块中所访问。当我说“块”时，我是指像在一个for循环或者一个if语句中用一个大括号{}包围起来的任何东西


所以让我们最后一次回顾下我们的discountPrices函数
```
function discountPrices (prices, discount) {
  var discounted = []  
  for (var i = 0; i < prices.length; i++) {
    var discountedPrice = prices[i] * (1 - discount)
    var finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  
  console.log(i) // 3
  console.log(discountedPrice) // 150
  console.log(finalPrice) // 150  
  return discounted
}
```

记住我们能够在for循环之外打印i，discountedPrice和finalPrice，因为它们被var声明，并且var是一个函数作用域，但是现在，如果我们改变这些var声明，使用let声明，并且尝试运行它，会发生什么呢？
```
function discountPrices (prices, discount) {
  let discounted = []  
  for (let i = 0; i < prices.length; i++) {
    let discountedPrice = prices[i] * (1 - discount)
    let finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  
  console.log(i) // 3
  console.log(discountedPrice) // 150
  console.log(finalPrice) // 150  
  return discounted
}
discountPrices([100, 200, 300], .5) // ❌ ReferenceError: i is not defined
```

我们得到ReferenceError: i is not defined.这告诉我们的是，用let声明变量是块作用域而不是函数作用域。所以尝试在它们声明的“块”之外访问i（或者discountedPrice or finalPrice）将给我们一个引用错误，正如我们刚才所看到的。
# var VS let
> var: function scopedlet: block scoped


下一个区别与提升相关。之前我们说过，提升的定义是“JavaScript解析器将在“创建”阶段分配声明的变量一个默认值undefined”我们甚至在声明变量之前通过打印一个变量看到了这个行为（你得到了undefined）
```
function discountPrices (prices, discount) {
  console.log(discounted) // undefined  
  var discounted = []  
  for (var i = 0; i < prices.length; i++) {
    var discountedPrice = prices[i] * (1 - discount)
    var finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  
  console.log(i) // 3
  console.log(discountedPrice) // 150
  console.log(finalPrice) // 150  
  return discounted
}
```

我想不到任何你想要在声明变量之前访问变量的案例。这看起来就像抛出一个引用错误也比返回undefined更好


事实上，这就是let做的事情。如果你尝试在声明之前访问一个用let声明过的变量，而不是得到undefined（就像用var声明那些变量），你将得到一个引用错误
```
function discountPrices (prices, discount) {
  console.log(discounted) // ❌ ReferenceError  
  let discounted = []  
  for (let i = 0; i < prices.length; i++) {
    let discountedPrice = prices[i] * (1 - discount)
    let finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  
  console.log(i) // 3
  console.log(discountedPrice) // 150
  console.log(finalPrice) // 150  
  return discounted
}
```

# var VS let

>var: function scoped
      undefined when accessing a variable before it's declared

>let: block scoped
    ReferenceError when accessing a variable before it's declared

# let VS const


现在你理解了var和let的区别，那么const呢？事实上，const更像是let，然而，唯一的不同点就是你一旦用const分配了一个变量的值，你将不能重新分配一个新的值。
```
let name = 'Tyler'
const handle = 'tylermcginnis'
      name = 'Tyler McGinnis' // ✅
handle = '@tylermcginnis' // ❌ TypeError: Assignment to constant variable.
```

上面的内容用let声明的变量可以重新赋值，但是用const声明的变量不能


非常酷，所以你只要想一个变量永远不变，你可以用const来声明。当然也不完全，仅仅因为用const声明一个变量并不意味着它是永远不变的，这完全意味着值不能被重新赋值。这就是一个很好的例子
```
const person = {
  name: 'Kim Kardashian'
}
person.name = 'Kim Kardashian West' // ✅
person = {} // ❌ Assignment to constant variable.
```

注意，在一个对象上改变一个属性并不是重新赋值，所以即使一个对象用const声明，也不意味着你不能改变它的属性。这仅仅意味你不能重新分配一个新值


现在我们还没有回答最重要的问题是：你应该使用var，let或者const？最受欢迎的观点和我表述的观点是，你应该总是使用const，除非你知道变量将发生改变。这么做的原因是使用const，你向未来的自己以及必须阅读你代码的任何未来的开发者们发出信号，这个变量不应该改变。如果它将需要改变（像在for循环中），你应该使用let


所以，在可变的变量与不可改变的变量之间，就没有太多的东西剩下。这意味着你不应该再使用var


# 总结

var是函数作用域，并且如果你尝试在实际声明之前使用一个var声明的变量，你将得到undefined。

const和let是块作用域，并且如果你尝试在实际声明之前使用let或const声明的变量，你将得到一个引用错误。

最后在let和const之间的区别是一旦你用const分配了一个值，你将不行重新赋值，但是用let可以







