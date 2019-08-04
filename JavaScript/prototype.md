# 原型

熟悉 JavaScript 原型開發的人可能會認為`原型（prototype）`和對象的關係很親密，但是這全都和函數有關。原型雖然是定義對象的一種很方便的方式，但它的本質依然是函數特性。

## 對象實例化

**使用 new 操作符將函數作為構造器進行調用的時候，其上下文被定義為新對象的實例。**

`new` 關鍵字的過程：

1. 聲明一個中間對象；
2. 將該中間對象的原型指向構造函數的原型；
3. 將構造函數的 this 指向該中間對象；
4. 返回該中間對象，即返回實例對象。

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


## 繼承

```js
function Person(name, age) {
  this.name = name
  this.age = age
}
Person.prototype.getName = function () {
  return this.name
}
Person.prototype.getAge = function () {
  return this.age
}
function Student(name, age, grade) {
  // 构造函数继承
  Person.call(this, name, age)
  this.grade = grade
}
// 原型继承
Student.prototype = Object.create(Person.prototype, {
  // 不要忘了重新指定构造函数
  constructor: {
    value: Student
  },
  getGrade: {
    value: function () {
      return this.grade
    }
  }
})
var s1 = new Student('ming', 22, 5)
console.log(s1.getName()) // ming
console.log(s1.getAge()) // 22
console.log(s1.getGrade()) // 5
```

流傳的幾種繼承方式，其實都沒有真正實現繼承：

### 構造函數繼承：`Parent.call(this)`

```js
function Parent1() {
  this.name = 'parent1'
}
Parent1.prototype.say = function() {}
function Child1() {
  Parent1.call(this)
  this.type = 'child'
}
console.log(new Child1())
```

這種智能繼承父類的實例屬性和方法，不能繼承原型屬性和方法。

### 原型鏈繼承：`Child.prototype = new Parent()`

```js
function Parent2() {
  this.name = 'parent2'
  this.play = [1, 2, 3]
}
function Child2() {
  this.type = 'child2'
}
Child2.prototype = new Parent2()
console.log(new Child2())
var s1 = new Child2()
var s2 = new Child2()
console.log(s1.play) // (3) [1, 2, 3]
console.log(s2.play) // (3) [1, 2, 3]
s1.play.push(4)
console.log(s1.play) // (4) [1, 2, 3, 4]
console.log(s2.play) // (4) [1, 2, 3, 4]
```

子類實例都共享父類實例的屬性，一個實例修改屬性會影響另一個實例。

### 組合繼承：結合構造函數繼承和原型繼承

```js
function Parent3() {
  this.name = 'parent3'
  this.play = [1, 2, 3]
}
Parent3.prototype.say = function () { }
function Child3() {
  Parent3.call(this)
  this.type = 'child3'
}
Child3.prototype = new Parent3()
var s3 = new Child3()
var s4 = new Child3()
console.log(s3.play) // (3) [1, 2, 3]
console.log(s4.play) // (3) [1, 2, 3]
s3.play.push(4)
console.log(s3.play) // (4) [1, 2, 3, 4]
console.log(s4.play) // (3) [1, 2, 3]
```

真正實現繼承，構造函數中的屬性通過 call 每次賦值，原型中的屬性繼承自父類原型。缺點是實例一個對象是，父類 new 了兩次。

### 組合繼承優化（1）

```js
function Parent4() {
  this.name = 'parent4'
  this.play = [1, 2, 3]
}
Parent4.prototype.say = function () { }
function Child4() {
  Parent4.call(this)
  this.type = 'child4'
}
Child4.prototype = Parent4.prototype
var s5 = new Child4()
var s6 = new Child4()
console.log(s5 instanceof Child4) // true
console.log(s5 instanceof Parent4) // true
console.log(s5.constructor) // ƒ Parent4() { }
```

通過 call 就可以獲得構造函數中所有屬性，所以原型直接賦值就好。但是無法確定對象有什麼構造函數實例化，constructor 判斷可知，子類的構造器還是父類。這是因為 constructor 在 prototype 中，指向構造器。這裡直接賦值產生問題。

### 組合繼承優化（2）

```js
function Parent5() {
  this.name = 'parent5'
  this.play = [1, 2, 3]
}
Parent5.prototype.say = function () { }
function Child5() {
  Parent5.call(this)
  this.type = 'child5'
}
Child5.prototype = Object.create(Parent5.prototype)
Child5.prototype.constructor = Child5
var s5 = new Child5()
console.log(s5 instanceof Child5); // true
console.log(s5 instanceof Parent5); // true
console.log(s5.constructor); // ƒ Parent5() { }
```

這裡使用 `Object.create()`，它的作用是將對象繼承到 proto 屬性上，舉個例子：

```js
var test = Object.create({ x: 123, y: 345 });
console.log(test); // {}
console.log(test.x); // 123
console.log(test.__proto__.x); // 123
console.log(test.__proto__.x === test.x); // true
```

