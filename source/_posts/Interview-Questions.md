---
title: Interview-Questions
date: 2019-01-24 18:44:08
tags: 面试
---

> 本文只记录面试过程中遇到的面试题(不定更)，答案请自行百度(自己动手、丰衣足食)

# 框架知识

1.谈谈对ssr的了解
> seo与首屏渲染快
> 比较适合首屏使用ssr进行快速渲染，更利于seo

2.react和vue的区别
> **react**:
1.all in js
        2.函数式
        3.单向数据流

>**vue**:
1.html css js在一个文件下
     2.响应式
     3.双向绑定

3.vue数据驱动的原理
> 1.vue在实例化过程中，遍历data所有的属性并使用Object.defineProperty将属性全转化为getter/setter
> 2.每个实例对象都有一个watcher，在模板编译过程中使用getter访问data的属性，并且标记为依赖，建立视图与数据的联系
> 3.当依赖的数据发生了变化，就调用了setter方法，watcher会对比前后两个值的变化，决定是否通知视图重新渲染

4.v-if和v-show的区别
>1.DOM差异，CSS差异

5.Proxy 相比于 defineProperty 的优势
> - 数组变化也能监听到
  - 不需要深度遍历监听

6.nextTick()
>官方解释：在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

>何时使用：(DOM发生变化的操作（不严谨！！！具体看下面两个例子👇👇👇）)
  - 1.在created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中。
  **原因**：是在created()钩子函数执行的时候DOM 其实并未进行任何渲染，所以一定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。mounted该钩子函数执行时所有的DOM挂载和渲染都已完成，因此不需要使用nextTick()。

  - 2.在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作应该放进Vue.nextTick()的回调函数中。

7.vue的生命周期
> 参考本文，个人感觉写的比较详细了 https://segmentfault.com/a/1190000011381906
> 也可参考本文，讲的比较精干https://juejin.im/post/5c64d15d6fb9a049d37f9c20#heading-60

8.beforeCreate和Created的区别：
> beforeCreate 获取不到el和data
> Created 获取的到data，但获取不到el（mounted中可获取）


# Git知识

1.git中rebase是做什么的，有何意义
> 把提交的记录变得流畅

# 非框架知识

1.serviceworker
>参考本文https://www.cnblogs.com/dojo-lzz/p/8047336.html

2.哪些方式导致内存泄漏
>1.意外的全局变量: 无法被回收
>2.定时器: 未被正确关闭，导致所引用的外部变量无法被释放
>3.事件监听: 没有正确销毁 (低版本浏览器可能出现)
>4.闭包: 会导致父级中的变量无法被释放
>5.dom 引用: dom 元素被删除时，内存中的引用未被正确清空

3.本地存储及区别
> 这里重点讨论**IndexedDB**和**WebSQL**：参考本文加以理解http://www.cnblogs.com/ljwsyt/p/9760266.html

4.闭包、作用及处理，垃圾回收器
> **概念**：1. 作为一个函数变量的一个引用，当函数返回时，其处于激活状态。
　　      2. 一个闭包就是当一个函数返回时，一个没有释放资源的栈区。
> **作用**：实现封装，管理私有变量和私有方法，将变量（状态）的变化封装在安全的环境中
> **弊端**：让变量始终保持在内存中，不被释放，内存消耗会很大
> **处理**：使用后的变量置为Null，等待垃圾回收器回收

> 垃圾回收器：两种方式：1.**标记清除**（主要）2.**引用计数**
                     

5.按需加载（webpack）
> 参考本文https://segmentfault.com/a/1190000011519350
  
  1.import():
  ```
   {
            path: '/hello',
            name: 'Hello',
            // component: Hello
            component: import(/* webpackChunkName: 'Hello' */   '../components/Hello')
        }
  ```
  2.require.ensure()：多个路由指定相同的chunkName，会合并打包成一个js文件。
  ```
   {
            path: '/hello',
            name: 'Hello',
            // component: Hello
            component: resolve => require.ensure([], () => resolve(require('../components/Hello')), 'demo')
        }
  ```


6.阻止冒泡
> event.stopPropagation()

7.css动画和js动画的区别
> 参考本文https://www.cnblogs.com/simba-lkj/p/6139066.html

8.cdn的原理，哪些东西可以使用cdn
>   **介绍**：经策略性部署的整体系统，解决由于**网络带宽小、用户访问量大、网点分布不均而产生的用户访问网站响应速度慢**的问题。

>   **目的**:在现有的Internet中增加一层新的网络架构，将网站的内容发布到**最接近用户**的网络“边缘”，使用户可以**就近**取得所需的内容，**解决 Internet 网络拥塞状况，提高用户访问网站的响应速度。**

9.深拷贝和浅拷贝的区别
>参考本文https://www.cnblogs.com/echolun/p/7889848.html

>浅拷贝：一层遍历和Object.assign()

> 深拷贝主要针对**引用类型**,拷贝**所有层级**的属性

- 方法一：递归实现深拷贝
```
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                //判断ojb子元素是否为对象，如果是，递归复制
                if(obj[key]&&typeof obj[key] ==="object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}    
let a=[1,2,3,4],
    b=deepClone(a);
a[0]=2;
console.log(a,b);
```

- 方法二：JSON.Parse和JSON.stringify（兼容ie8+，对于**正则表达式类型和函数类型**无法深拷贝，会直接丢失相应的值,而且会抛弃**对象的constructer**）
```
function deepClone(obj){
    let _obj = JSON.stringify(obj),
        objClone = JSON.parse(_obj);
    return objClone
}    
let a=[0,1,[2,3],4],
    b=deepClone(a);
a[0]=1;
a[2][0]=1;
console.log(a,b);
```

10.Map和WeakMap与Object
> ES6新的数据结构，更为完善的hash结构
> 与Object的区别：Map的键可以是个各种类型的数值，但Object只能是字符串类型或Symbol类型
> Map与WeakMap的区别：WeakMap只接受对象作为键名，键名是对象的弱引用，当对象被回收后，WeakMap自动移除对应的键值对，WeakMap结构有助于防止内存泄漏。 

# Set和WeakSet
> Set集合，没有重复元素的集合,WeakSet的键必须为对象，与WeakMap类似

11.有哪几种继承
> 1.原型链继承：将父类的实例作为子类的原型
  2.构造继承：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）
  3.实例继承：为父类实例添加新特性，作为子类实例返回
  4.拷贝继承
  5.组合继承：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
  6.寄生组合继承：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
  7.ES6的Class继承

12.跨越的处理方案
>   协议，域名，端口不用
>   1、 通过jsonp跨域 （只支持get）
    2、 document.domain + iframe跨域（仅限主域相同，子域不同的跨域应用场景）
    3、 window.name + iframe跨域
    --  3.1name值在不同的页面（甚至不同域名）加载后依旧存在，并且可以支持非常长的 name 值（2MB）。
    4、 postMessage跨域
    5、 跨域资源共享（CORS）`Access-Control-Allow-Origin`
    --  5.1普通跨域请求：只服务端设置Access-Control-Allow-Origin即可，前端无须设置，
    --  5.2若要带cookie请求：前后端都需要设置。
    6、 nginx代理跨域
    7、 nodejs中间件代理跨域
    8、 Vue跨域：(proxy)
    
13.重绘和回流
重绘(repaint): 当元素样式的改变不影响布局时，浏览器将使用重绘对元素进行更新，此时由于只需要UI层面的重新像素绘制，因此 损耗较少


回流(reflow): 当元素的尺寸、结构或触发某些属性时，浏览器会重新渲染页面，称为回流。此时，浏览器需要重新经过计算，计算后还需要重新页面布局，因此是较重的操作。会触发回流的操作:



15.性能优化
>1.一些常用压缩方式
>2.服务器缓存策略
- Cache-Control/Expires:浏览器判断缓存是否过期，未过期时，直接使用强缓存，**如果同时出现，Cache-Control的 max-age 优先级高于 Expires**

- 协商缓存：唯一标识`Etag`和`If-None-Match`、最后一次修改时间`Last-Modified`和`If-Modified-Since`（Etag优先级高于Lst-Modifieds）

16.jsonp的原理以及为什么不是真正的ajax

17.lazyload的原理

19.sea和require的区别、优缺点
> 最大的区别：SeaJS对模块的态度是懒执行, 而RequireJS对模块的态度是预执行
  RequireJS:（1）实现js文件的异步加载，避免网页失去响应；
            （2）管理模块之间的依赖性，便于代码的编写和维护。  

21.web worker
> 简而言之就是子线程，本质就是数据刷新和页面渲染不产生冲突，可通过自带的**消息机制**进行通信但是弊端是兼容性不好，而且无法达到像websocket轮询的效果


# ！！！

22.箭头函数什么情况下不能用(**需要动态上下文的场景**)
>1.定义对象方法
2.定义原型方法
3.定义事件回调函数
4.定义构造函数
5.刻意追求过短的代码，可能会给代码阅读和逻辑理解带来困难。

23.rem和em的区别
>区别：rem是基于**html元素的字体大小**来决定，而em则根据**使用它的元素的大小**决定（很多人错误以为是根据父类元素，实际上是使用它的元素继承了父类的属性才会产生的错觉）

> **举例**：
    当使用 rem 单位，他们转化为像素大小取决于页根元素的字体大小，即 html 元素的字体大小。 根元素字体大小乘以你 rem 值。
    例如，根元素的字体大小 16px，10rem 将等同于 160px，即 10 x 16 = 160。 

>   当使用em单位时，像素值将是em值乘以使用em单位的元素的字体大小。
    弊端：继承

24.移动端适配
> 1.
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no;">
```
> 2.手淘团队flexible.js布局

25.数组如何去重（ES5,ES6）
> 1.ES6中新增了**Set数据结构**，类似于数组，但是 它的成员都是**唯一**的 ，其构造函数可以接受一个数组作为参数
> 2.ES6中Array新增了一个静态方法**Array.from**，可以把**类似数组的对象**转换为**数组**

```
ES6:
let array = [1,2,3,4,4,2,1,5];
let res = Array.from(new Set(array))
console.log(res)
```
```
ES5方法一
function unique(arr) {
    return arr.filter(function(item, index, arr) {
        //当前元素，在原始数组中的第一个索引==当前索引值，否则返回当前元素
        return arr.indexOf(item, 0) === index
    })
}
```
```
ES5方法二
function uniqueArr(arr){
    var arr2 = []
    var len = arr.length
    for(var i=0;i<len; i++) {
        if(arr2.indexOf(arr[i]) === -1) {
            arr2.push(arr[i])
        }
    }
    return arr2
} 
```

26.函数柯里化
> 柯里化是指这样一个函数(假设叫做createCurry)，他接收函数A作为参数，运行后能够返回一个新的函数。并且这个新的函数能够处理函数A的剩余参数。
> 详情参考https://www.jianshu.com/p/5e1899fe7d6b

27.vue的虚拟dom原理是什么？怎么实现的？！！！！！！！！！！！！！难以理解
>js创建dom，diff算法，同级比较

28.http请求头里都有什么内容
- Accept:浏览器能够处理的内容类型
- Accept-Charset:浏览器能够显示的字符集
- Accept-Encoding：浏览器能够处理的压缩编码
- Accept-Language：浏览器当前设置的语言
- Connection：浏览器与服务器之间连接的类型
- Cookie：当前页面设置的任何Cookie
- Host：发出请求的页面所在的域
- Referer：发出请求的页面的URL
- User-Agent：浏览器的用户代理字符串

29.常用状态码
> - 200: 成功，并返回数据
  - 302: 临时重定向
  - 304: 资源未修改，可使用缓存
  - 403: 拒绝请求
  - 404: 资源不存在
  - 500: 服务器错误

29.promise顺序执行 

> 方法一:  then()执行
> 方法二： 使用队列执行
> 方法三： 使用async、await实现类似同步编程

30.Event loop
> 首先放上参考文章，非常推荐，简单明了！！！https://juejin.im/post/59e85eebf265da430d571f89

> 概念： 简单来讲就是时间循环机制（废话，哈哈哈哈哈～～～～）
  
  首先看一下下面这张图，有助于理解js是如何运行的
{% asset_img eventloop.png eventloop %}

上图表达的意思就是：👇👇👇

- 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
- 当指定的事情完成时，Event Table会将这个函数移入Event Queue。
- 主线程内的任务执行完毕为空，（js引擎存在monitoring process进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去Event Queue那里检查是否有等待被调用的函数。）会去Event Queue读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的Event Loop(事件循环)。

除了广义的同步任务和异步任务，我们对任务有更精细的定义：
  - macro-task(宏任务)：包括整体代码script，IO，setTimeout，setInterval
  - micro-task(微任务)：Promise中的回调（Promise自身是宏任务，不知是否可以这么理解），process.nextTick
{% asset_img task-step.png task-step %}


30.call apply bind区别和用法

> 都是为了改变某个函数运行时的上下文而存在的（就是为了改变函数内部this的指向）；
apply的**第二个参数**是一个**数组**，call第二个及以后的参数都是数组中的元素
bind与apply、call**最大的区别**就是：bind不会立即调用，其他两个会立即调用   


31.**THIS**（**看完你就明白了**）
> this 就是你 call 一个函数时，传入的第一个参数。（请务必背下来「this 就是 call 的第一个参数」）
> 参考本文，一看就懂https://zhuanlan.zhihu.com/p/23804247

32.原型与原型链
> **原型链**:在JavaScript中万物都是对象，对象和对象之间也有关系，并不是孤立存在的。对象之间的继承关系，在JavaScript中是通过prototype对象指向父类对象，直到指向Object对象为止，这样就形成了一个原型指向的链条
> **原理**：当我们访问对象的一个属性或方法时，它会先在对象自身中寻找，如果有则直接使用，如果没有则会去原型对象中寻找，如果找到则直接使用。如果没有则去原型的原型中寻找,直到找到Object对象的原型，Object对象的原型没有原型，如果在Object原型中依然没有找到，则返回undefined。
>**方法**：使用对象的hasOwnProperty()来检查对象自身是否含有该属性，使用in来检查是否在整条原型链上是否含有该属性
>**注意点**：Object是JS中所有对象数据类型的基类(最顶层的类)在Object.prototype上没有__proto__这个属性。

> 所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（null除外）
  所有的引用类型（数组、对象、函数），都有一个__proto__属性，属性值是一个普通的对象
  所有的函数，都有一个prototype属性，属性值也是一个普通的对象
  所有的引用类型（数组、对象、函数），__proto__属性值指向它的构造函数的prototype属性值

33.GraphQL
>详情请关注官网链接http://graphql.cn/
> **概念**：一种用于 API 的查询语言，代替rest api


# 算法
1.冒泡排序
```
  function bubleSort(arr) {
	    var len = arr.length;
	    for (let i = len ; i >= 2;i--) {
	        for(let j = 0; j <= i - 1; j++) {
	            if(arr[j] > arr[j + 1]) {
	               // [arr[j],arr[j+1]] = [arr[j+1],arr[j]]
                 let tmp = arr[j+1]
                 arr[j+1] = arr[j]
                 arr[j] = tmp
	            }
	        }
	    }
	    return arr;
	}
```

2.快速排序
原理：选一个数，比较大小，放在当前数的左右数组，最后递归左右数组
```
function quickSort(arr) {
    if(arr.length <= 1) {
        return arr;  //递归出口
    }
    var left = [],
        right = [],
        current = arr.splice(0,1); 
    for(let i = 0; i < arr.length; i++) {
        if(arr[i] < current) {
            left.push(arr[i])  //放在左边
        } else {
            right.push(arr[i]) //放在右边
        }
    }
    return quickSort(left).concat(current,quickSort(right));
}

```
