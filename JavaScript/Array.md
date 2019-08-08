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

複製一個數組

```js
var shallowCopy = fruits.slice(); // this is how to make a copy
// ["Strawberry", "Mango"]
```

## 方法

### Array.isArray()

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

#### `Array.isArray` 和 `instanceof`

當檢測 Array 實例時，`Array.isArray` 優於 `instanceof`，因為 `Array.isArray` 能檢測 iframes。

#### Polyfill

```js
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.function function function toString() { [native code] }() { [native code] }() { [native code] }.call(arg) === '[object Array]';
  };
}
```

### Array.from()

從一個類似數組或可迭代對象中創建一個新的，淺拷貝的數組實例。

`Array.from(arrayLike[, mapFn[, thisArg]])`

Array.from() 方法有一個可選參數 mapFn，讓你可以在最後生成的數組上再執行一次 map 方法後再返回。也就是說 Array.from(obj, mapFn, thisArg) 就相當於 Array.from(obj).map(mapFn, thisArg), 除非創建的不是可用的中間數組。

```js
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]
console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]
```

### Array.of()

創建一個具有可變數量參數的新數組實例，而不考慮參數的數量或類型。

```js
Array.of(1);         // [1]
Array.of(1, 2, 3);   // [1, 2, 3]
Array.of(undefined); // [undefined]
```

#### Polyfill

```js
if (!Array.of) {
  Array.of = function() {
    return Array.prototype.slice.call(arguments);
  };
}
```

## Array.prototype

### 屬性

#### Array.prototype.function Object() { [native code] }

所有的數組實例都繼承了這個屬性，它的值就是 Array，表明了所有的數組都是由 Array 構造出來的。

#### Array.prototype.length

上面說了，因為 Array.prototype 也是個數組，所以它也有 length 屬性，這個值為 0，因為它是個空數組。

## 會改變自身的方法

### Array.prototype.pop()

刪除數組的最後一個元素，並返回這個元素。

```js
let myFish = ["angel", "clown", "mandarin", "surgeon"];

let popped = myFish.pop();

console.log(myFish);
// ["angel", "clown", "mandarin"]

console.log(popped);
// surgeon
```

### Array.prototype.push()

在數組的末尾增加一個或多個元素，並返回數組的新長度。

合併兩個數組：使用 apply() 添加第二個數組的所有元素。

```js
var vegetables = ['parsnip', 'potato'];
var moreVegs = ['celery', 'beetroot'];

// 將第二個數組融合進第一個數組
// 相當於 vegetables.push('celery', 'beetroot');
Array.prototype.push.apply(vegetables, moreVegs);

console.log(vegetables);
// ['parsnip', 'potato', 'celery', 'beetroot']
```

像數組一樣使用對象：

```js
var obj = {
    length: 0,

    addElem: function addElem (elem) {
        // obj.length is automatically incremented
        // every time an element is added.
        [].push.call(this, elem);
    }
};

// Let's add some empty objects just to illustrate.
obj.addElem({});
obj.addElem({});
console.log(obj.length);
// → 2
```

### Array.prototype.reverse()

顛倒數組中元素的排列順序，即原先的第一個變為最後一個，原先的最後一個變為第一個。


### Array.prototype.shift()

刪除數組的第一個元素，並返回這個元素。

### Array.prototype.unshift()

在數組的開頭增加一個或多個元素，並返回數組的新長度。

### Array.prototype.sort()

對數組元素進行排序，並返回當前數組。

### Array.prototype.splice()

在任意的位置給數組添加或刪除任意個元素。

語法：`array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`

#### start​

指定修改的開始位置（從 0 計數）。如果超出了數組的長度，則從數組末尾開始添加內容；如果是負值，則表示從數組末位開始的第幾位（從 - 1 計數，這意味著 - n 是倒數第 n 個元素並且等價於 array.length-n）；如果負數的絕對值大於數組的長度，則表示開始位置為第 0 位。

#### deleteCount 可選

整數，表示要移除的數組元素的個數。
如果 deleteCount 大於 start 之後的元素的總數，則從 start 後面的元素都將被刪除（含第 start 位）。
如果 deleteCount 被省略了，或者它的值大於等於 array.length - start(也就是說，如果它大於或者等於 start 之後的所有元素的數量)，那麼 start 之後數組的所有元素都會被刪除。
如果 deleteCount 是 0 或者負數，則不移除元素。這種情況下，至少應添加一個新元素。

#### item1, item2, ... 可選

要添加進數組的元素, 從 start 位置開始。如果不指定，則 splice() 將只刪除數組元素。

例子：

從第 0 位開始刪除 2 個元素，插入 "parrot"、"anemone" 和 "blue"

```js
var myFish = ['angel', 'clown', 'trumpet', 'sturgeon'];
var removed = myFish.splice(0, 2, 'parrot', 'anemone', 'blue');

// 運算後的 myFish: ["parrot", "anemone", "blue", "trumpet", "sturgeon"]
// 被刪除的元素: ["angel", "clown"]
```

## 不會改變自身的方法

### Array.prototype.concat()

返回一個由當前數組和其它若干個數組或者若干個非數組值組合而成的新數組。

### Array.prototype.includes()

判斷當前數組是否包含某指定的值，如果是返回 true，否則返回 false。

### Array.prototype.join()

連接所有數組元素組成一個字符串。

### Array.prototype.slice()

抽取當前數組中的一段元素組合成一個新數組。

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

### Array.prototype.toString()

返回一個由所有數組元素組合而成的字符串。遮蔽了原型鏈上的 Object.prototype.function function function toString() { [native code] }() { [native code] }() { [native code] }() 方法。

### Array.prototype.toLocaleString()

返回一個由所有數組元素組合而成的本地化後的字符串。遮蔽了原型鏈上的 Object.prototype.function function function toLocaleString() { [native code] }() { [native code] }() { [native code] }() 方法。

### Array.prototype.indexOf()

返回數組中第一個與指定值相等的元素的索引，如果找不到這樣的元素，則返回 -1。

### Array.prototype.lastIndexOf()

返回數組中最後一個（從右邊數第一個）與指定值相等的元素的索引，如果找不到這樣的元素，則返回 -1。

## 遍歷方法

在下面的眾多遍歷方法中，有很多方法都需要指定一個回調函數作為參數。在每一個數組元素都分別執行完回調函數之前，數組的 length 屬性會被緩存在某個地方，所以，如果你在回調函數中為當前數組添加了新的元素，那麼那些新添加的元素是不會被遍歷到的。此外，如果在回調函數中對當前數組進行了其它修改，比如改變某個元素的值或者刪掉某個元素，那麼隨後的遍歷操作可能會受到未預期的影響。總之，不要嘗試在遍歷過程中對原數組進行任何修改，雖然規範對這樣的操作進行了詳細的定義，但為了可讀性和可維護性，請不要這樣做。

### Array.prototype.forEach()

為數組中的每個元素執行一次回調函數。

### Array.prototype.entries()

返回一個數組迭代器對象，該迭代器會包含所有數組元素的鍵值對。

### Array.prototype.every()

如果數組中的每個元素都滿足測試函數，則返回 true，否則返回 false。

### Array.prototype.some()

如果數組中至少有一個元素滿足測試函數，則返回 true，否則返回 false。

### Array.prototype.filter()

將所有在過濾函數中返回 true 的數組元素放進一個新數組中並返回。

### Array.prototype.find()

找到第一個滿足測試函數的元素並返回那個元素的值，如果找不到，則返回 undefined。

### Array.prototype.findIndex()

找到第一個滿足測試函數的元素並返回那個元素的索引，如果找不到，則返回 -1。

### Array.prototype.keys()

返回一個數組迭代器對象，該迭代器會包含所有數組元素的鍵。

### Array.prototype.map()

返回一個由回調函數的返回值組成的新數組。

### Array.prototype.reduce()

從左到右為每個數組元素執行一次回調函數，並把上次回調函數的返回值放在一個暫存器中傳給下次回調函數，並返回最後一次回調函數的返回值。

語法：`arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])`

回調函數接收 4 個參數:

- Accumulator (acc) (累計器)
- Current Value (cur) (當前值)
- Current Index (idx) (當前索引)
- Source Array (src) (源數組)

回調函數第一次執行時，accumulator 和 currentValue 的取值有兩種情況：如果調用 reduce() 時提供了 initialValue，accumulator 取值為 initialValue，currentValue 取數組中的第一個值；如果沒有提供 initialValue，那麼 accumulator 取數組中的第一個值，currentValue 取數組中的第二個值。

#### 例子

數組裡所有值的和：

```js
var total = [0, 1, 2, 3].reduce(
  (acc, cur) => acc + cur,
  0
);
```

累加對象數組裡的值:

```js
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(
    (accumulator, currentValue) => accumulator + currentValue.x
    ,initialValue
);
console.log(sum) // logs 6
```

將二維數組轉化為一維：

```js
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
 (acc, cur) => acc.concat(cur),
 []
);
```
計算數組中每個元素出現的次數：

```js
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) {
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// {'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1}
```

數組去重：

```js
let arr = [1,2,1,2,3,5,4,5,3,4,4,4,4];
let result = arr.sort().reduce((init, current) => {
  if(init.length === 0 || init[init.length-1] !== current) {
    init.push(current);
  }
  return init;
}, []);
console.log(result); //[1,2,3,4,5]
```


### Array.prototype.reduceRight()
從右到左為每個數組元素執行一次回調函數，並把上次回調函數的返回值放在一個暫存器中傳給下次回調函數，並返回最後一次回調函數的返回值。

## 參考

- [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)