# 對象

## Object.create()

`Object.create()` 方法創建一個新對象，使用現有的對象作為新創建對象的 prototype。

Lodash Hash 中的 clear：

```js
clear() {
  this.__data__ = Object.create(null)
  this.size = 0
}
```

## 類型判斷

### typeof


## 方法

### `toString`

每个对象都有一个 toString() 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，toString() 方法被每个 Object 对象继承。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中 type 是对象的类型。

检测对象类型：

```js
var toString = Object.prototype.toString;

toString.call(new Date); // [object Date]
toString.call(new String); // [object String]
toString.call(Math); // [object Math]

//Since JavaScript 1.8.5
toString.call(undefined); // [object Undefined]
toString.call(null); // [object Null]
```

