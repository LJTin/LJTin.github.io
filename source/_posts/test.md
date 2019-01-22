---
title: When to use var vs let vs const in JavaScript
date: 2019-01-21 22:45:06
tags:
---


In this post you’ll learn two new ways to create variables in JavaScript (ES6), let and const. Along the way we’ll look at the differences between var, let, and const as well as cover topics like function vs block scope, variable hoisting, and immutability.

在这篇文章中，你将会在javascript（ES6）中学习两种新的方式创建变量，let和const。在此过程中，我们将看到var，let和const的不同之处，以及包括诸如函数与块作用域、变量提升和不变性等主题

ES2015 (or ES6) introduced two new ways to create variables, let and const. But before we actually dive into the differences between var, let, and const, there are some prerequisites you need to know first. They are variable declarations vs initialization, scope (specifically function scope), and hoisting

ES2015（或者ES6）介绍了两种新的方式创建变量，let和const。但是在我们真正区分var,let和const不同点之前，你应该先了解一些前提条件。它们是变量声明vs初始化，作用域（特别是函数作用域），和变量提升

# Variable Declaration vs Initialization

# 变量声明vs初始化

A variable declaration introduces a new identifier

一个变量声明介绍了一个新的标识符

```
var declaration
```

Above we create a new identifier called declaration. In JavaScript, variables are initialized with the value of undefined when they are created. What that means is if we try to log the declaration variable, we’ll get undefined.

上面我们创建了一个新的标识符叫做声明。在javascript中，当变量被创建，它们以undefined的值被初始化。这意味着，如果我们尝试打印声明的变量，我们将得到undefined

```
var declaration
console.log(declaration)
```

So if we log the declaration variable, we get undefined.

所以如果我们打印声明的变量，我们得到undefined

In contrast to variable declaration, variable initialization is when you first assign a value to a variable.

与变量声明相反，变量初始化是指当你首次给一个变量赋值

```
var declaration
console.log(declaration) // undefined
declaration = 'This is an initialization'
```

So here we’re initializing the declaration variable by assigning it to a string.
This leads us to our second concept, Scope.

所以在这里我们通过赋值给已经声明当变量一个字符串来初始化它。

这就引导我们第二个概念了，作用域

# Scope(作用域)

Scope defines where variables and functions are accessible inside of your program. In JavaScript, there are two kinds of scope — global scope, and function scope. According to the official spec,

作用域定义了在你的程序里，变量和函数在哪里可以获取到。在JavaScript中，有两种类型的作用域--全局作用域和函数作用域，根据官方的规范

```
“If the variable statement occurs inside a FunctionDeclaration, the variables are defined with function-local scope in that function.”.
```

如果变量语句发生在函数声明中，则变量在这个函数中通过函数局部作用域来定义

What that means is if you create a variable with var, that variable is “scoped” to the function it was created in and is only accessible inside of that function or, any nested functions.

这就意味着如果你使用var来创建一个函数，那么这个变量被运用于它所创建的函数，并且只能在该函数内部或者任何嵌套函数内部被访问。

```
function getDate () {
  var date = new Date()
  return date
}
getDate()
console.log(date) // ❌ Reference Error
```

Above we try to access a variable outside of the function it was declared. Because date is “scoped” to the getDate function, it’s only accessible inside of getDate itself or any nested functions inside of getDate (as seen below).

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
Now let’s look at a more advanced example. Say we had an array of pricesand we needed a function that took in that array as well as a discount and returned us a new array of discounted prices. The end goal might look something like this:

现在让我们来看一个更高级的例子。假设我们有一个prices的数组，并且我们需要一个函数，这个函数接收一个像折扣的数组，并且返回一个折扣后的新数组。最后的目标可能看起来像这样：

```
discountPrices([100, 200, 300], .5)
```
And the implementation might look something like this:

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
Seems simple enough, but what does this have to do with block scope? Take a look at that for loop. Are the variables declared inside of it accessible outside of it? Turns out, they are.

好像很简单，但是这与块作用域有什么关系？看一下这个for循环，它声明的变量是否可以在它外部被访问呢？事实证明，是可以的。
```
function discountPrices (prices, discount) {
  var discounted = []  for (var i = 0; i < prices.length; i++) {
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
If JavaScript is the only programming language you know, you may not think anything of this. However, if you’re coming to JavaScript from another programming language, specifically a programming language that is blocked scope, you’re probably a little bit concerned about what’s going on here.

如果JavaScript是你所了解的唯一一个编程语言，你可能不会想什么。然而，如果你从另一门编程语言（特别是这门语言是封闭的作用域）开始使用JavaScript，那么你很可能对这发生对事情感到有点担心。

It’s not really broken, it’s just kind of weird. There’s not really a reason to still have access to i, discountedPrice, and finalPrice outside of the forloop. It doesn’t really do us any good and it may even cause us harm in some cases. However, since variables declared with var are function scoped, you do.

它没有真正的破碎，他只是有点怪异。在for循环之外，这没有一个理由仍然能够访问到i，discountedPrice，和finalPrice。它不会对我们有任何的好处，并且甚至可能在一定程度上对我们造成伤害。然而，因为用var声明的变量是函数作用域的，所以你可以这样做。

Now that we’ve discussed variable declarations, initializations, and scope, the last thing we need to flush out before we dive into let and const is hoisting.

现在我们已经可以讨论过变量声明，初始化，和作用域，那么在我们深入了解let和const之前我们需要清除的最后一件事就是提升。

# Hoisting（提升）

Remember earlier we said that “In JavaScript, variables are initialized with the value of undefined when they are created.” Turns out, that’s all that “Hoisting” is. The JavaScript interpreter will assign variable declarations a default value of undefined during what’s called the “Creation” phase.

记得早的时候我们说过，‘在JavaScript中，变量被创建时可以用undefined值来初始化。事实证明，这就是“Hoisting”的全部。JavaScript解析器在被“创建”的阶段将分配一个默认的值undefined给变量的声明。

> For a much more in depth guide on the Creation Phase, Hoisting, and Scopes see “The Ultimate Guide to Hoisting, Scopes, and Closures in JavaScript”.

寻找更多更深入关于创建阶段，提升和作用域的指南，可以参阅 The Ultimate Guide to Hoisting, Scopes, and Closures in JavaScript

Let’s take a look at the previous example and see how hoisting affects it.

让我们看一下之前的例子和提升是如何影响它的
```
function discountPrices (prices, discount) {
  var discounted = undefined
  var i = undefined
  var discountedPrice = undefined
  var finalPrice = undefined  discounted = []
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
Notice all the variable declarations were assigned a default value of undefined. That’s why if you try access one of those variables before it was actually declared, you’ll just get undefined.

注意所有声明的变量都被分配一个默认的值undefined。这就是为什么如果你在变量被真正声明之前访问其中一个变量，你仅仅得到undefined
```
function discountPrices (prices, discount) {
  console.log(discounted) // undefined  
  var discounted = []  for (var i = 0; i < prices.length; i++) {
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
Now that you know everything there is to know about var, let’s finally talk about the whole point of why you’re here: what’s the difference between var, let, and const?

现在你已经知道了有关var的每一件事，让我们最后讨论你为什么在这的重点：var，let和const的区别在哪里？

# var VS let VS const
First, let’s compare var and let. The main difference between var and letis that instead of being function scoped, let is block scoped. What that means is that a variable created with the let keyword is available inside the “block” that it was created in as well as any nested blocks. When I say “block”, I mean anything surrounded by a curly brace {} like in a for loop or an ifstatement.

首先，让我们比较var和let。var和let主要的不同点不是函数作用域，let是块作用域。

这就意味着一个变量用let关键字创建可以被“块”内部以及任何嵌套的块中所访问。当我说“块”时，我是指像在一个for循环或者一个if语句中用一个大括号{}包围起来的任何东西

So let’s look back to our discountPrices function one last time.

所以让我们最后一次回顾下我们的discountPrices函数
```
function discountPrices (prices, discount) {
  var discounted = []  for (var i = 0; i < prices.length; i++) {
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
Remember that we were able to log i, discountedPrice, and finalPriceoutside of the for loop since they were declared with var and var is function scoped. But now, what happens if we change those var declarations to use let and try to run it?

记住我们能够在for循环之外打印i，discountedPrice和finalPrice，因为它们被var声明，并且var是一个函数作用域，但是现在，如果我们改变这些var声明，使用let声明，并且尝试运行它，会发生什么呢？
```
function discountPrices (prices, discount) {
  let discounted = []  for (let i = 0; i < prices.length; i++) {
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
We get ReferenceError: i is not defined. What this tells us is that variables declared with let are block scoped, not function scoped. So trying to access i (or discountedPrice or finalPrice) outside of the “block” they were declared in is going to give us a reference error as we just barely saw.

我们得到ReferenceError: i is not defined.这告诉我们的是，用let声明变量是块作用域而不是函数作用域。所以尝试在它们声明的“块”之外访问i（或者discountedPrice or finalPrice）将给我们一个引用错误，正如我们刚才所看到的。
# var VS let
> var: function scopedlet: block scoped

The next difference has to do with Hoisting. Earlier we said that the definition of hoisting was “The JavaScript interpreter will assign variable declarations a default value of undefined during what’s called the ‘Creation’ phase.” We even saw this in action by logging a variable before it was declared (you get undefined).

下一个区别与提升相关。之前我们说过，提升的定义是“JavaScript解析器将在“创建”阶段分配声明的变量一个默认值undefined”我们甚至在声明变量之前通过打印一个变量看到了这个行为（你得到了undefined）
```
function discountPrices (prices, discount) {
  console.log(discounted) // undefined  
  var discounted = []  for (var i = 0; i < prices.length; i++) {
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
I can’t think of any use case where you’d actually want to access a variable before it was declared. It seems like throwing a ReferenceError would be a better default than returning undefined.

我想不到任何你想要在声明变量之前访问变量的案例。这看起来就像抛出一个引用错误也比返回undefined更好

In fact, this is exactly what let does. If you try to access a variable declared with let before it’s declared, instead of getting undefined (like with those variables declared with var), you’ll get a ReferenceError.

事实上，这就是let做的事情。如果你尝试在声明之前访问一个用let声明过的变量，而不是得到undefined（就像用var声明那些变量），你将得到一个引用错误
```
function discountPrices (prices, discount) {
  console.log(discounted) // ❌ ReferenceError  
  let discounted = []  for (let i = 0; i < prices.length; i++) {
    let discountedPrice = prices[i] * (1 - discount)
    let finalPrice = Math.round(discountedPrice * 100) / 100
    discounted.push(finalPrice)
  }  console.log(i) // 3
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

Now that you understand the difference between var and let, what about const? Turns out, const is almost exactly the same as let. However, the only difference is that once you’ve assigned a value to a variable using const, you can’t reassign it to a new value.

现在你理解了var和let的区别，那么const呢？事实上，const更像是let，然而，唯一的不同点就是你一旦用const分配了一个变量的值，你将不能重新分配一个新的值。
```
let name = 'Tyler'
const handle = 'tylermcginnis'
      name = 'Tyler McGinnis' // ✅
handle = '@tylermcginnis' // ❌ TypeError: Assignment to constant variable.
```
The take away above is that variables declared with let can be re-assigned, but variables declared with const can’t be.

上面的内容用let声明的变量可以重新赋值，但是用const声明的变量不能

Cool, so anytime you want a variable to be immutable, you can declare it with const. Well, not quite. Just because a variable is declared with const doesn’t mean it’s immutable, all it means is the value can’t be re-assigned. Here’s a good example.

非常酷，所以你只要想一个变量永远不变，你可以用const来声明。当然也不完全，仅仅因为用const声明一个变量并不意味着它是永远不变的，这完全意味着值不能被重新赋值。这就是一个很好的例子
```
const person = {
  name: 'Kim Kardashian'
}
person.name = 'Kim Kardashian West' // ✅
person = {} // ❌ Assignment to constant variable.
```
Notice that changing a property on an object isn’t reassigning it, so even though an object is declared with const, that doesn’t mean you can’t mutate any of its properties. It only means you can’t reassign it to a new value.

注意，在一个对象上改变一个属性并不是重新赋值，所以即使一个对象用const声明，也不意味着你不能改变它的属性。这仅仅意味你不能重新分配一个新值

Now the most important question we haven’t answered yet: should you use var, let, or const? The most popular opinion, and the opinion that I subscribe to, is that you should always use const unless you know the variable is going to change. The reason for this is by using const, you’re signaling to your future self as well as any other future developers that have to read your code that this variable shouldn’t change. If it will need to change (like in a forloop), you should use let.

现在我们还没有回答最重要的问题是：你应该使用var，let或者const？最受欢迎的观点和我表述的观点是，你应该总是使用const，除非你知道变量将发生改变。这么做的原因是使用const，你向未来的自己以及必须阅读你代码的任何未来的开发者们发出信号，这个变量不应该改变。如果它将需要改变（像在for循环中），你应该使用let

So between variables that change and variables that don’t change, there’s not much left. That means you shouldn’t ever have to use var again.

所以，在可变的变量与不可改变的变量之间，就没有太多的东西剩下。这意味着你不应该再使用var

So to recap, var is function scoped and if you try to use a variable declared with var before the actual declaration, you’ll just get undefined. const andlet are blocked scoped and if you try to use variable declared with let orconst before the declaration you’ll get a ReferenceError. Finally the difference between let and const is that once you’ve assigned a value to const, you can’t reassign it, but with let, you can.

# 总结

var是函数作用域，并且如果你尝试在实际声明之前使用一个var声明的变量，你将得到undefined。

const和let是块作用域，并且如果你尝试在实际声明之前使用let或const声明的变量，你将得到一个引用错误。

最后在let和const之间的区别是一旦你用const分配了一个值，你将不行重新赋值，但是用let可以





本文参考

https://medium.freecodecamp.org/var-vs-let-vs-const-in-javascript-2954ae48c037



