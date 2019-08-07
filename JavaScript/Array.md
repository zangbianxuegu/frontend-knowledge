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

Array.from() 方法有一個可選參數 mapFn，讓你可以在最後生成的數組上再執行一次 map 方法後再返回。也就是說 Array.from(obj, mapFn, thisArg) 就相當於 Array.from(obj).map(mapFn, thisArg), 除非創建的不是可用的中間數組。

## Array.isArray()

確定傳遞的值是否是一個 Array。

```js
// 下面的函數調用都返回 true
Array.isArray([]);
Array.isArray([1]);
Array.isArray(new Array());
// 鮮為人知的事實：其實 Array.prototype 也是一個數組。
Array.isArray(Array.prototype);

// 下面的函數調用都返回 false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(17);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({[object Object]: Array.prototype});
```

### `Array.isArray` 和 `instanceof`

當檢測 Array 實例時，`Array.isArray` 優於 `instanceof`，因為 `Array.isArray` 能檢測 iframes。

### Polyfill

```js
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.function toString() { [native code] }.call(arg) === '[object Array]';
  };
}
```

## Array.of()

創建一個具有可變數量參數的新數組實例，而不考慮參數的數量或類型。

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

方法返回一個新的數組對象，這一對象是一個由 begin 和 end 決定的原數組的淺拷貝（包括 begin，不包括 end）。原始數組不會被改變。

```js
var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
var citrus = fruits.slice(1, 3);
// fruits contains ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
// citrus contains ['Orange','Lemon']
```

slice 方法可以用來將一個類數組（Array-like）對象 / 集合轉換成一個新數組。

```js
function list() {
  return Array.prototype.slice.call(arguments);
}
var list1 = list(1, 2, 3); // [1, 2, 3]
```

## Array.prototype

### 屬性

#### Array.prototype.function Object() { [native code] }

所有的數組實例都繼承了這個屬性，它的值就是 Array，表明了所有的數組都是由 Array 構造出來的。

#### Array.prototype.length

上面說了，因為 Array.prototype 也是個數組，所以它也有 length 屬性，這個值為 0，因為它是個空數組。

### 方法

#### 會改變自身的方法

##### Array.prototype.pop()

刪除數組的最後一個元素，並返回這個元素。

##### Array.prototype.push()

在數組的末尾增加一個或多個元素，並返回數組的新長度。

##### Array.prototype.reverse()

顛倒數組中元素的排列順序，即原先的第一個變為最後一個，原先的最後一個變為第一個。

##### Array.prototype.shift()

刪除數組的第一個元素，並返回這個元素。

##### Array.prototype.sort()

對數組元素進行排序，並返回當前數組。

##### Array.prototype.splice()

在任意的位置給數組添加或刪除任意個元素。

##### Array.prototype.unshift()

在數組的開頭增加一個或多個元素，並返回數組的新長度。

#### 不會改變自身的方法

##### Array.prototype.concat()

返回一個由當前數組和其它若干個數組或者若干個非數組值組合而成的新數組。

##### Array.prototype.includes()

判斷當前數組是否包含某指定的值，如果是返回 true，否則返回 false。

##### Array.prototype.join()

連接所有數組元素組成一個字符串。

##### Array.prototype.slice()

抽取當前數組中的一段元素組合成一個新數組。

##### Array.prototype.function toString() { [native code] }()

返回一個由所有數組元素組合而成的字符串。遮蔽了原型鏈上的 Object.prototype.function toString() { [native code] }() 方法。

##### Array.prototype.function toLocaleString() { [native code] }()

返回一個由所有數組元素組合而成的本地化後的字符串。遮蔽了原型鏈上的 Object.prototype.function toLocaleString() { [native code] }() 方法。

##### Array.prototype.indexOf()

返回數組中第一個與指定值相等的元素的索引，如果找不到這樣的元素，則返回 -1。

##### Array.prototype.lastIndexOf()

返回數組中最後一個（從右邊數第一個）與指定值相等的元素的索引，如果找不到這樣的元素，則返回 -1。

#### 遍歷方法

在下面的眾多遍歷方法中，有很多方法都需要指定一個回調函數作為參數。在每一個數組元素都分別執行完回調函數之前，數組的 length 屬性會被緩存在某個地方，所以，如果你在回調函數中為當前數組添加了新的元素，那麼那些新添加的元素是不會被遍歷到的。此外，如果在回調函數中對當前數組進行了其它修改，比如改變某個元素的值或者刪掉某個元素，那麼隨後的遍歷操作可能會受到未預期的影響。總之，不要嘗試在遍歷過程中對原數組進行任何修改，雖然規範對這樣的操作進行了詳細的定義，但為了可讀性和可維護性，請不要這樣做。

##### Array.prototype.forEach()

為數組中的每個元素執行一次回調函數。

##### Array.prototype.entries()

返回一個數組迭代器對象，該迭代器會包含所有數組元素的鍵值對。

##### Array.prototype.every()

如果數組中的每個元素都滿足測試函數，則返回 true，否則返回 false。

##### Array.prototype.some()

如果數組中至少有一個元素滿足測試函數，則返回 true，否則返回 false。

##### Array.prototype.filter()

將所有在過濾函數中返回 true 的數組元素放進一個新數組中並返回。

##### Array.prototype.find()

找到第一個滿足測試函數的元素並返回那個元素的值，如果找不到，則返回 undefined。

##### Array.prototype.findIndex()

找到第一個滿足測試函數的元素並返回那個元素的索引，如果找不到，則返回 -1。

##### Array.prototype.keys()

返回一個數組迭代器對象，該迭代器會包含所有數組元素的鍵。

##### Array.prototype.map()

返回一個由回調函數的返回值組成的新數組。

##### Array.prototype.reduce()

從左到右為每個數組元素執行一次回調函數，並把上次回調函數的返回值放在一個暫存器中傳給下次回調函數，並返回最後一次回調函數的返回值。

##### Array.prototype.reduceRight()
從右到左為每個數組元素執行一次回調函數，並把上次回調函數的返回值放在一個暫存器中傳給下次回調函數，並返回最後一次回調函數的返回值。

## 參考

- [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)