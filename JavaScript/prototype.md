# 原型

熟悉 JavaScript 原型開發的人可能會認為`原型（prototype）`和對象的關係很親密，但是這全都和函數有關。原型雖然是定義對象的一種很方便的方式，但它的本質依然是函數特性。

## 對象實例化

**使用 new 操作符將函數作為構造器進行調用的時候，其上下文被定義為新對象的實例。**

在構造器函數內部定義了一個方法，同時在構造器的 `prototype` 屬性上添加了一個同名方法，創建構造器的實例，會執行哪一個方法？

```js
function Ninja () {
  this.swung = false;
  this.swingSword = function () {
    return !this.swung
  }
}
Ninja.prototype.swingSword = function () {
  return this.swung
}
var ninja = new Ninja()
console.log(ninja.swingSword()) // true
```

測試證明，會執行構造器內定義的方法。

初始化操作的優先級是很重要的，優先級如下：

1. 通過原型給對象實例添加屬性。
2. 在構造器函數內給對象添加屬性。

在構造器內的綁定操作優先級永遠高於在原型上的綁定操作優先級。

**因為構造器的 this 上下文指向的是實例自身，所以我們可以在構造器內對核心內容執行初始化操作。**

原型上的屬性並沒有複製到其他地方，而是實時附加到新創建的對象上：

```js
function Ninja () {
  this.swung = false;
}
var ninja = new Ninja()
Ninja.prototype.swingSword = function () {
  return this.swung
}
console.log(ninja.swingSword()) // false
```

引用過程：

1. 在引用對象的一個屬性時，首先檢查該對象本身是否擁有該屬性，如果有，則直接返回，如果沒有……
2. 則，再查看對象的原型（prototype），檢查該原型上是否有所要的屬性，如果有，則直接返回，若果沒有……
3. 則，該值是 undefined。

查詢屬性引用時，首先是查詢對象自身，如果不存在，才在原型上進行查找：

```js
function Ninja () {
  this.swung = true;
  this.swingSword = function () {        
    return !this.swung
  }
}
var ninja = new Ninja()
Ninja.prototype.swingSword = function () {
  return this.swung
}
console.log(ninja.swingSword()) // false
```

## instanceof

可以使用 `instanceof` 才確定一個實例是否是由特定的函數構造器所創建。

`constructor` 屬性為創建該對象的原始函數的引用。

```js
function Ninja () {}
var ninja = new Ninja()
console.log(typeof ninja == 'object') // true
console.log(ninja instanceof Ninja) // true
console.log(ninja.constructor == Ninja) // true
```

由於 `constructor` 是原始構造器的一個引用，所以可以使用該引用再實例化一個新對象：

```js
function Ninja () {}
var ninja = new Ninja()
var ninja2 = new ninja.constructor()
console.log(ninja2 instanceof Ninja) // true
console.log(ninja == ninja2) // false
```

## 繼承和原型鏈

使用一個對象的實例作為另一個對象的原型。

```js
subClass.prototype = new SuperClass()
```

這將保持原型鏈，因為 subClass 實例的原型是 SuperClass 的一個實例，該實例不僅擁有原型，還持有 SuperClass 的所有屬性，並且該原型指向其自身超類的一個實例，以此類推。

```js
function Person () {}
Person.prototype.dance = function () {}
function Ninja () {}
Ninja.prototype = new Person()
var ninja = new Ninja()
console.log(ninja instanceof Ninja) // true
console.log(ninja instanceof Person) // true
console.log(ninja instanceof Object) // true
console.log(ninja.dance)
```

可能容易想到直接將 Person 的原型賦值給 Ninja 的原型，`Ninja.prototype = Person.protype`，這樣做，Ninja 原型上的任何修改都會影響到 Person 的原型。

## HTML DOM 原型

所有的 DOM 元素都繼承于 HTMLElement 構造器。

```js
HTMLElement.prototype.remove = function () {
  if (this.parentNode) {
    this.parentNode.removeChild(this)
  }
}
```

## 疑難陷阱

### 擴展對象

```js
Object.prototype.keys = function () {
  let keys = []
  for (const key in this) {
    keys.push(key)
  }
  return keys
}
var obj = {a: 1, b: 2, c: 3}
console.log(obj.keys()) // (4) ["a", "b", "c", "keys"]
```

因為給所有的對象都添加了 keys 這一屬性。

### hasOwnProperty

```js
Object.prototype.keys = function () {
  let keys = []
  for (const key in this) {
    if (this.hasOwnProperty(key)) {
      keys.push(key)
    }
  }
  return keys
}
var obj = {a: 1, b: 2, c: 3}
console.log(obj.keys()) // (3) ["a", "b", "c"]
```

`hasOwnProperty` 可以確定一個屬性是在對象實例上定義的，還是從原型里導入的。

### 判斷函數是否作為構造器進行調用

```js
function Test () {
  return this instanceof arguments.callee
}
console.log(Test()) // false
console.log(new Test()) // Test {}
```

- `arguments.callee` 可以得到當前執行函數的引用
- “普通”函數的上下文是全局作用域
- 利用 `instanceof` 操作符測試已構建對象是否構建于指定的構造器


