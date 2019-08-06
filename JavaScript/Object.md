# 对象

## Object.create()

`Object.create()` 方法創建一個新對象，使用現有的對象作為新創建對象的 prototype。

Lodash Hash 中的 clear：

```js
clear() {
  this.__data__ = Object.create(null)
  this.size = 0
}
```