# 函数

函数是第一类对象，因为 JavaScript 的函数有以下特点：

- 可以通过字面量进行创建
- 可以拥有名称或匿名
- 可以赋值给变量数组或其他对象属性
- 可以拥有属性和方法
- 可以作为参数传递给其他函数
- 可以作为函数的返回值进行返回
- 可以用不同的方法进行调用
- 可以传递与声明不同数量的参数
- 函数跟变量一样提升
- 函数是唯一拥有自身作用域的结构

## 回调

定义一个函数稍后执行，无论何时定义，在浏览器执行还是其他地方执行，我们定义的就是回调（callback）。

例子：

```js
function useless (callback) {
  return callback()
}
var text = 'callback'
useless(function () {
  console.log(text)
})
```

一个数组排序：

```js
var arr = [12,34,45,5,2,4556]
arr.sort(function(a1, a2) {return a2 - a1})
// (6) [4556, 45, 34, 12, 5, 2]
// sort 函数执行了传入的回调函数
```

## 函数声明

函数名称是可选的，没有名称的函数是匿名函数。命名一个函数，改函数在整个函数声明范围内是有效的。如果一个函数声明在顶层，window 对象上的同名属性则会引用到改函数。

所有的函数都有一个 name 属性，该属性保存函数名称的字符串。匿名函数也有 name 属性，只是值为空字符串。

在 JavaScript 中，作用域由 function 进行声明，而不是代码块。变量声明的作用域开始于声明的地方，结束于所在函数的结尾，于代码嵌套无关。命名函数的作用域是指声明该函数的整个函数范围，与代码嵌套无关。（有些人称之为机制提升）。对于作用域声明，全局上下文就像一个包含页面所有代码的超大型函数。

## 函数调用

作为一个函数进行调用
作为一个方法进行调用，在对象上进行调用
作为构造器进行调用，创建一个新对象
通过 apply() 或 call() 方法进行调用

### 参数

- 如果实参数量大于函数声明的形参数量，超出的参数不会配给形参名称
- 如果函数声明的形参数量大于实参数量，则没有对应参数的形参会赋值为 undefined

所有的函数调用都会传递两个隐式参数：arguments 和 this

#### arguments

arguments 不是数组。它有一个 length 属性，可以使用数组表示法（arguments[2]）进行获取，可以用 for 循环进行遍历。但是不能使用数组方法。

#### this

一个函数被调用时，除了传入了函数的显示参数以外，名为 this 的隐式参数也被传入了参数。this 参数引用了与该函数调用进行隐式关联的一个对象，被称之为函数上下文（function context）。

函数上下文，来自想 Java 这样的面向对象语言，认为 this 是方法声明所在的类的一个实例。但是，函数作为方法进行调用只是四种函数调用方式中的一种。和 Java 中的 this 不同，Java 中的 this 依赖于函数的声明，而 JavaScript 中的 this 则依赖于函数的调用方式，基于这个事实，将 this 称为调用上下文（invocation context）可能更为清晰。

四种调用机制的不同，也在于如何定义每种调用类型的 this。

### 作为函数进行调用

```js
function foo () {

}
foo()
var bar = function () {

}
bar()
```

函数的上下文是全局对象——window。作为函数进行调用是作为方法进行调用的一个特殊情况。

### 作为方法进行调用

```js
var o = {
  foo: function () {
    
  }
}
o.foo()
```

一个方法所属的对象在该方法体内可以以 this 的形式进行引用。将函数作为对象的一个方法进行调用时，该对象就变成了函数上下文，并且在函数内部可以以 this 参数的形式进行访问。这是可以将 JavaScript 作为面向对象代码进行编写的主要手段之一（另一个是构造器）。

### 作为构造器进行调用

函数调用前使用 new 关键字

将函数作为构造器进行调用，是 JavaScript 的一个超级特性，构造器调用时发生以下行为：

- 创建一个新的空对象。
- 传递给构造器的对象是 this 参数，从而成为构造器的函数上下文。
- 如果没有显示的返回值，新创建的对象则作为构造器的返回值进行返回。

```js
function Foo () {
  this.test = function () {
    return this
  }
}
var foo1 = new Foo()
foo1.test() === foo1 // true

function Foo () {
  var o = {}
  o.test = function () {
    return this
  }
  return o
}
var foo2 = Foo()
foo2.test() === foo2 // true
```

### 使用 apply() 和 call() 方法进行调用

函数作为方法进行调用，上下文是方法的拥有者；作为全局函数进行调用，其上下文永远是 window，作为构造器进行调用，其上下文对象是新创建的对象实例。

函数调用时，apply、call 显式指定任何一个对象作为其函数上下文。apply 传入两个参数：作为函数上下文的对象，作为函数参数所组成的数组。call 一样，只不过给函数传入的参数是一个参数列表，而不是一个数组。

```js
function juggle() {
  var result = 0
  for (var n = 0; n < arguments.length; n++) {
    result += arguments[n]
  }
  this.result = result
}
var ninja1 = {}
var ninja2 = {}
juggle.apply(ninja1, [1, 2, 3, 4])
juggle.call(ninja2, 5, 6, 7, 8)
console.log(ninja1.result) // 10
console.log(ninja2.result) // 26
```

#### 在回调中强制指定函数上下文

forEach 函数，该函数在数组的每个元素上都进行回调调用。可以很简单的将当前元素作为参数传递给回调函数，但大多数情况下，都是将当前元素作为回调函数的函数上下文。

```js
function forEach (list, callback) {
  for (var n = 0; n < list.length; n++) {
    callback.call(list[n], n)
  }
}
var weapons = ['a', 'b', 'c']
forEach(weapons, function (index) {
  console.log(this == weapons[index])
})
```

## 递归

### 普通命名函数中的递归

```js
function chirp (n) {
  return n - 1 ? chirp(n-1) + '-chirp' : 'chirp'
}
console.log(chirp(3)) // chirp-chirp-chirp
```

### 方法中的递归

```js
var ninja = {
  chirp: function (n) {
   return n - 1 ? ninja.chirp(n-1) + '-chirp' : 'chirp' 
  }
}
console.log(ninja.chirp(3)) // chirp-chirp-chirp
```

### 引用的丢失问题

```js
var ninja = {
  chirp: function (n) {
   return n - 1 ? ninja.chirp(n-1) + '-chirp' : 'chirp' 
  }
}
var samurai = {
  chirp: ninja.chirp
}
ninja = {}
try {
  console.log(samurai.chirp(3))
} catch (e) {
  console.log("Uh, this isn't good") // Uh, this isn't good
}
```

修复：

```js
var ninja = {
  chirp: function (n) {
   return n - 1 ? this.chirp(n-1) + '-chirp' : 'chirp' 
  }
}
var samurai = {
  chirp: ninja.chirp
}
ninja = {}
try {
  console.log(samurai.chirp(3)) // chirp-chirp-chirp
} catch (e) {
  console.log("Uh, this isn't good")
}
```

内联命名函数：

```js
var ninja = {
  chirp: signal (n) {
   return n - 1 ? this.sigual(n-1) + '-chirp' : 'chirp' 
  }
}
var samurai = {
  chirp: ninja.chirp
}
ninja = {}

```

## 函数重载

面向对象语言里，方法重载通常是通过在同名方法（但不同参数）里声明不同的实现来达到目的。但在 JavaScript 中，通过传入参数的特性和个数进行相应修改来达到目的。

merge 函数：

```js
function merge (root) {
  for (var i = 1; i < arguments.length; i++) {
    for (var key in arguments[i]) {
      root[key] = arguments[i][key]
    }
  } 
  return root
} 
var merged = merge(
  // 第一个参数
  {
    name: 'Batou'
  },
  // 第二个参数
  {
    city: 'Niihama'
  }
)
```

root 是第一个参数，作为返回，arguments 可以获取其他参数，但是从第二个开始。





