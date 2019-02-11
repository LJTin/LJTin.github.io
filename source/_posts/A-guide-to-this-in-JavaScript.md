---
title: A guide to this in JavaScript
date: 2019-02-01 19:50:58
tags:
---

原文参考https://medium.freecodecamp.org/a-guide-to-this-in-javascript-e3b9daef4df1

<br>
{% asset_img you_got_this.jpeg you got this %}
<br>
`this` 关键词是Javascript中最广为使用的并且令人误解的词。今天我将尝试改变它。

当学习代词时，让我们回到古老的学校时代。

>菲尔普斯游得很快，因为他想赢得比赛。

注意代词`he`的使用。我们在这不直接称呼菲尔普斯但是用代词`he`指代菲尔普斯。类似地，JavaScript使用`this`关键字作为指示对象来引用上下文中的对象，即主题。

例如：
<br>
{% asset_img ex1.png example %}
<br>

在上面的代码中，我们有一个对象`car`，它有`make`,`model`和`fullName`属性。
`fullName`的值是一个`function`，它可以用两种不同的语法打印`car`的全称

- 使用`this => this.make+” “ +this.model`，这个`this`指向上下文中的对象，所以`this.make`实际上是`car.make`,并且`this.model`也是如此

- 使用点表示法，我们可以访问对象的属性`car.make`和`car.model`。
<br>

# 这就是`this`!

现在我们已经理解了`this`是什么，这只是最基础的用法，让我们来做一些规则以致于我们可以记住。

JS中的`this`关键词指向它所属的对象。

```
var car={
  make:'....'
  func:()=>{
    console.log(this.make)
  }
}
```

上面代码片段中的`this`属于对象`car`

依赖于用法，它有不同的值
1. 在一个方法里
2. 在一个函数里
3. 单独
4. 在一个事件中
5. `call()`和`apply()`

<br>
**在一个方法里**

当`this`在一个方法里使用，它指向方法所有者对象。

在一个对象中的定义的函数被叫做方法。让我们再举一次`car`的例子。

```
var car= {
  make: "Lamborghini",
  model: "Huracán",
  fullName: function () {
    console.log(this.make+" " +this.model);
    console.log(car.make+ " " +car.model);
  }
}
car.fullName();
```

`fullName()`在这里是一个方法。在这个方法里的`this`属于`car`
<br>
**在一个函数里**

在一个函数里，`this`有一点复杂。首先要理解的是，与所有对象都具有属性一样，函数也具有属性。无论函数何时执行，它都能获取`this`属性，该属性是一个变量，其中包含调用它的对象的值。

如果函数没有被对象调用，那么函数内部的`this`属于全局对象，称之为窗口。在这个案例里，`this`将引用全局作用域中定义的值。让我们看一个更好理解的例子：

```
var make= "Mclaren";
var model= "720s"
function fullName(){ 
  console.log(this.make+ " " + this.model);
}
var car = {
    make:"Lamborghini",
    model:"Huracán",
    fullName:function () {
      console.log (this.make + " " + this.model);
    }
}
car.fullName(); // Lmborghini Huracán
window.fullName(); // Mclaren 720S
fullName(); // Mclaren 720S
```

这里`make`，`model`和`fullName`是全局定义的，而`car`对象也有`fullName`的实现。当`car`对象被调用时，这指的是在对象内定义的属性。另一方面，其他两个函数调用是相同的并返回全局定义的属性。
<br>
**单独**

当单独使用而不是在任何函数或对象内部时，`this`指的是全局对象。
```
var make = 'Mclaren';
var model = '720s'
var name = 'Ferrari';
console.log(this.name); //Ferrari
```
这里的`this`指的是全局名称属性。

<br>
**在一个事件中**

事件可以是任何类型，但为了简单和清晰起见，让我们进行点击事件。
{% asset_img 123.jpeg  %}

只要单击一个按钮并引发一个事件，它就可以调用另一个函数来根据点击执行某个任务。如果`this`在函数内部使用，它将引用引发事件的元素。在DOM中，所有元素都存储为对象。这就是为什么当引发一个事件时它会引用该元素，因为该`webpage`元素实际上是DOM中的一个对象。

比如：
```
<button onclick="this.style.display='none'">
  Remove Me!
</button>

```
<br>
**`call()`、`apply()`& `bind()`**
- bind：允许我们在方法上设置this的值。
- call和apply：允许我们借用函数并在函数调用上设置`this`的值。
<br>

# 未完待续···