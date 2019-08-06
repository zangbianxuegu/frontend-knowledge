# 數組

遍歷數組

```js
var fruits = ["Apple", "Banana"]
fruits.forEach(function (item, index, array) {
  console.log(item, index, array)
})
// Apple 0 (2) ["Apple", "Banana"]
// Banana 1 (2) ["Apple", "Banana"]
```

添加元素到數組的末尾

```js
var newLength = fruits.push('Orange');
// newLength:3; fruits: ["Apple", "Banana", "Orange"]
```

刪除數組末尾的元素

```js
var last = fruits.pop(); // remove Orange (from the end)
// last: "Orange"; fruits: ["Apple", "Banana"];
```

刪除數組最前面的元素

```js
var first = fruits.shift(); // remove Apple from the front
// first: "Apple"; fruits: ["Banana"];
```

添加元素到數組頭部

```js
var newLength = fruits.unshift('Strawberry') // add to the front
// ["Strawberry", "Banana"];
```

找出某個元素在數組中的索引

```js
fruits.push('Mango');
// ["Strawberry", "Banana", "Mango"]
var pos = fruits.indexOf('Banana');
// 1
```

通過索引刪除某個元素

```js
var removedItem = fruits.splice(pos, 1); // this is how to remove an item
// ["Strawberry", "Mango"]
```

從一個索引位置刪除多個元素

```js
var vegetables = ['Cabbage', 'Turnip', 'Radish', 'Carrot'];
console.log(vegetables); 
// ["Cabbage", "Turnip", "Radish", "Carrot"]
var pos = 1, n = 2;
var removedItems = vegetables.splice(pos, n);
// this is how to remove items, n defines the number of items to be removed,
// from that position(pos) onward to the end of array.
console.log(vegetables); 
// ["Cabbage", "Carrot"] (the original array is changed)
console.log(removedItems); 
// ["Turnip", "Radish"]
```

複製一個數組

```js
var shallowCopy = fruits.slice(); // this is how to make a copy 
// ["Strawberry", "Mango"]
```

## Array.from()

從一個類似數組或可迭代對象中創建一個新的，淺拷貝的數組實例。

```js
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]
console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]
```

### 語法

`Array.from(arrayLike[, mapFn[, thisArg]])`

Array.from() 方法有一个可选参数 mapFn，让你可以在最后生成的数组上再执行一次 map 方法后再返回。也就是说 Array.from(obj, mapFn, thisArg) 就相当于 Array.from(obj).map(mapFn, thisArg), 除非创建的不是可用的中间数组。

## Array.isArray()

确定传递的值是否是一个 Array。

```js
// 下面的函数调用都返回 true
Array.isArray([]);
Array.isArray([1]);
Array.isArray(new Array());
// 鲜为人知的事实：其实 Array.prototype 也是一个数组。
Array.isArray(Array.prototype); 

// 下面的函数调用都返回 false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(17);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({ __proto__: Array.prototype });
```

### `Array.isArray` 和 `instanceof`

当检测 Array 实例时，`Array.isArray` 优于 `instanceof`，因为 `Array.isArray` 能检测 iframes。

### Polyfill

```js
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```

## Array.of()

创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

```js
Array.of(1);         // [1]
Array.of(1, 2, 3);   // [1, 2, 3]
Array.of(undefined); // [undefined]
```

### Polyfill

```js
if (!Array.of) {
  Array.of = function() {
    return Array.prototype.slice.call(arguments);
  };
}
```

## Array.slice()

方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。

```js
var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
var citrus = fruits.slice(1, 3);
// fruits contains ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
// citrus contains ['Orange','Lemon']
```

slice 方法可以用来将一个类数组（Array-like）对象/集合转换成一个新数组。

```js
function list() {
  return Array.prototype.slice.call(arguments);
}
var list1 = list(1, 2, 3); // [1, 2, 3]
```

## 參考

- [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)