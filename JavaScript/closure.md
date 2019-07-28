# 闭包

## 定义

> 在计算机科学中，闭包（英语：Closure），又称词法闭包（Lexical Closure）或函数闭包（function closures），是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例。

> 在没有闭包的语言中，变量的生命周期只限于创建它的环境。但在有闭包的语言中，只要有一个闭包引用了这个变量，它就会一直存在。

——[闭包](https://zh.wikipedia.org/w/index.php?title=%E9%97%AD%E5%8C%85_%28%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6%29&variant=zh-cn)

也就是：

> 闭包就是由函数创造的一个词法作用域，里面创建的变量被引用后，可以在这个词法环境之外自由使用。——JavaScript 里的闭包是什么？应用场景有哪些？ - Cat Chen的回答 - 知乎
https://www.zhihu.com/question/19554716/answer/12211031

> 闭包就是一种延缓垃圾回收的机制，一般一个函数执行完函数内部的变量会跟着销毁掉，但是有时候你还需要这个变量，为了不让这个变量被销毁掉，只要让这个变量有引用存在就行。——JavaScript 里的闭包是什么？应用场景有哪些？ - 银八的回答 - 知乎
https://www.zhihu.com/question/19554716/answer/281926045

以上说明了闭包创建的原则。

> 词法作用域（lexical scoping）是指，函数在执行时，使用的是它被定义时的作用域，而不是这个函数被调用时的作用域。——JavaScript 里的闭包是什么？应用场景有哪些？ - 倪云建的回答 - 知乎
https://www.zhihu.com/question/19554716/answer/98106832

```js
var scope = 'global scope'
function checkScope () {
  var scope = 'local scope'
  return function () {
    console.log(scope)
  }
}
var f = checkScope()
f() // local scope
```

其实，由于作用域链本身的特性，以及函数在定义时就能和作用域链关联起来的自然特性，可以简单说：在JavaScript 中，每个函数都是闭包。
理解了闭包本身后，你会发现，闭包的强大之处其实在于：JavaScript 中的函数，通过作用域链和词法作用域两者的特性，将该函数定义时的所处的作用域中的相关函数进行了捕获和保存，从而可以在完全不同的上下文中进行引用。 - 倪云建的回答 - 知乎
https://www.zhihu.com/question/19554716/answer/98106832

我认为，闭包之所以难理解，在于它本来就在 JavaScript 这种函数式编程语言中很常见，我们可能已经习惯了，但是并没有理解这个概念。理解这个概念有助于去使用它。

闭包重要的是理解它的定义，理解了它的含义，用途就很明白了。

```js
var outer = 'outer'
function outerFn () {
  console.log(outer)
}
outerFn()
```

这就是一个最简单的闭包，outerFn 函数引用了变量 outer。这太正常不过，不过也没有体现闭包的用处。

```js
var outer = 'outer'
var later
function outerFn () {
  var inner = 'inner'
  function innerFn () {
    console.log(outer)
    console.log(inner)
  }
  later = innerFn
}
outerFn()
later()
```

这是稍微复杂一点的闭包。内部函数 innerFn 对外部函数 outerFn 作用域内的变量 inner 进行了引用，当外部函数 outerFn 执行完毕之后，函数 outerFn 的作用域已经不存在，但是执行内部函数 innerFn（later），却依然可以访问变量 inner，而且被保存在内存中了。

闭包中，函数并不一定 return 某个函数，比如上面的例子。

在函数式编程中，闭包的逻辑就是：『让程序运行环境来管理状态』。命令式语言围绕状态来建模，每次指令操作都在控制状态的变化，需要人为管理；而闭包是对行为的控制，将运行逻辑和上下文绑定，执行闭包的时候，逻辑和上下文是关联的，一个十分简单的例子：

```js
const foo = () => {
  let v = 1
  return () => {
    return v++
  }
}
const bar = foo()
bar() // 1
bar() // 2
bar() // 3
```
——JavaScript 里的闭包是什么？应用场景有哪些？ - Barret李靖的回答 - 知乎
https://www.zhihu.com/question/19554716/answer/130780176

创建多个实例。

## 用途

其实用途也就是闭包的特性。

### 模块、私有变量

父函数形成局部作用域，父函数中有变量、子函数，可以在父函数外执行子函数，因为闭包，子函数可以操作父函数的属性。这样就模拟了私有变量。

局部作用域，是 JavaScript 函数特性决定的，只要有函数，就形成了私有变量。但是在函数外要控制私有属性，就需要一个子函数操作，这样就形成了闭包。this、return 或者赋值给外层。

### 抽象

闭包是数据和行为的组合，这使得闭包具有较好抽象能力.

对象是在数据中以方法的形式内含了过程，闭包是在过程中以环境的形式内含了数据。所谓“闭包是穷人的对象”、“对象是穷人的闭包”，就是说使用其中的一种方式，就能实现另一种方式能够实现的功能。——JavaScript 里的闭包是什么？应用场景有哪些？ - 天方夜的回答 - 知乎
https://www.zhihu.com/question/19554716/answer/23064179

### 回调、定时器中

没有闭包，同时做很多事情的时候，无论是事件处理，还是动画，甚至是 Ajax 请求，都是极其困难的。如果不使用闭包，就要在全局作用域中保存变量。

## 循环中使用闭包的问题

循环中使用闭包存在问题。循环中的异步，比如 setTimeout 或者事件处理。

```js
function closure () {
  for (var i = 0; i < 5; i++) {
    setTimeout(function() {
      console.log(i)
    })
  }
}
closure() // 5个5 
```

其实这里已经有闭包，setTimeout 的回调函数对变量 i 引用，形成闭包（就像上面最简单的闭包，函数对外层变量的引用）。这些闭包是由回调函数和在 closure 函数作用域中捕获的环境所组成，这 5 个闭包在循环中被创建，但它们共享了同一个词法作用域，回调执行时，i 值被决定。由于循环在 setTimeout 触发之前早已执行完毕，i 指向 5。解决这个问题是使用更多的闭包。
——https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures

我们可以再用一个闭包来修正当前的问题。

```js
function newClosure () {
  for (var i = 0; i < 5; i++) {
    setTimeout(log(i))
  }
  function log (i) {
    console.log(i)
  }
}
newClosure() // 0 1 2 3 4
```

所有的回调不再共享同一个环境，log 函数为每一个回调创建一个新的词法环境。
~~函数 log 在 for 循环外保持着对 i 的引用，即使 for 循环执行完毕，函数 log 依然保存着对变量的引用。~~

