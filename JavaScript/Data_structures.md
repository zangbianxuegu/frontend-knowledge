# 數據類型和數據結構

## 數據類型

最新的 ECMAScript 標準定義了 8 種數據類型:

- 7 種原始類型:
  - Boolean
  - Null
  - Undefined
  - Number
  - BigInt
  - String
  - Symbol
- 和 Object

## 基本類型（primitive values 原始值）

基本類型（基本數值、基本數據類型）是一種既非對象也無方法的數據。在 JavaScript 中，共有 7 種基本類型：string，number，bigint，boolean，null，undefined，symbol (ECMAScript 2016 新增)。

多數情況下，基本類型直接代表了最底層的語言實現。

所有基本類型的值都是不可改變的。但需要注意的是，基本類型本身和一個賦值為基本類型的變量的區別。變量會被賦予一個新值，而原值不能像數組、對象以及函數那樣被改變。

除 Object 以外的所有類型都是不可變的（值本身無法被改變）。例如，與 C 語言不同，JavaScript 中字符串是不可變的（譯註：如，JavaScript 中對字符串的操作一定返回了一個新字符串，原始字符串並沒有被改變）。我們稱這些類型的值為 “原始值”。

### 示例 1

```js
// 使用字符串方法不會改變一個字符串
var bar = "baz";
console.log(bar);               // baz
bar.toUpperCase();
console.log(bar);               // baz

// 使用數組方法可以改變一個數組
var foo = [];
console.log(foo);               // []
foo.push("plugh");
console.log(foo);               // ["plugh"]

// 賦值行為可以給基本類型一個新值，而不是改變它
bar = bar.toUpperCase();       // BAZ
```

下面的示例將讓你體會到 JavaScript 是如何處理基本類型的。

### 示例 2

```js
// 基本類型
let foo = 5;

// 定義一個貌似可以改變基本類型值的函數
function addTwo(num) {
   num += 2;
}
// 和前面的函數一樣
function addTwo_v2(foo) {
   foo += 2;
}

// 調用第一個函數，並傳入基本類型值作為參數
addTwo(foo);
// Getting the current Primitive value
console.log(foo);   // 5

// 嘗試調用第二個函數...
addTwo_v2(foo);
console.log(foo);   // 5
```

你是否認為會得到 7，而不是 5？如果是，請看看代碼是如何運行的：

- addTwo 和 addTwo_v2 函數調用時，JavaScript 會檢查標識符 foo 的值，從而準確無誤的找到第一行實例化變量的聲明語句。
- 找到以後，JavaScript 將其作為參數傳遞給函數的形參。
- 在執行函數體內語句之前，**JavaScript 會將傳遞進來的參數（基本類型的值）複製一份**，創建一個本地副本。這個副本只存在於該函數的作用域中，我們能夠通過指定在函數中的標識符訪問到它（addTwo 中的 num，addTwo_v2 中的 foo）。
- 接下來，函數體中的語句開始執行：
  - 第一個函數中，創建了本地 num 參數，num 的值加 2，但這個值並不是原來的 foo 的值。
  - 第二個函數中，創建了本地參數 foo，並將它的值加 2，這個值不是外部 foo 的值。在這種情況下，外部的 foo 變量不能以任何方式被訪問到。這是因為 JavaScript 的詞法作用域（lexical scoping）所導致的變量覆蓋，本地的變量 foo 覆蓋了外部的變量 foo。欲知詳情，請參閱閉包。
- 綜上所述，函數中的任何操作都不會影響到最初的 foo，我們操作的只不過是它的副本。

這就是為什麼說所有基本類型的值都是無法改變的。

### 布爾類型

布爾表示一個邏輯實體，可以有兩個值：true 和 false。

### Null 類型

Null 類型只有一個值： null。

### Undefined 類型

一個沒有被賦值的變量會有個默認值 undefined。

### 數字類型

根據 ECMAScript 標準，JavaScript 中只有一種數字類型：基於 IEEE 754 標準的雙精度 64 位二進制格式的值（-(263 -1) 到 263 -1）。**它並沒有為整數給出一種特定的類型**。除了能夠表示浮點數外，還有一些帶符號的值：+Infinity，-Infinity 和 NaN (非數值，Not-a-Number)。

要檢查值是否大於或小於 +/-Infinity，你可以使用常量 Number.MAX_VALUE 和 Number.MIN_VALUE。另外在 ECMAScript 6 中，你也可以通過 Number.isSafeInteger() 方法還有 Number.MAX_SAFE_INTEGER 和 Number.MIN_SAFE_INTEGER 來檢查值是否在雙精度浮點數的取值範圍內。 超出這個範圍，JavaScript 中的數字不再安全了，也就是隻有 second mathematical interger 可以在 JavaScript 數字類型中正確表現。

數字類型中只有一個整數有兩種表示方法： 0 可表示為 -0 和 +0（"0" 是 +0 的簡寫）。 在實踐中，這也幾乎沒有影響。 例如 +0 === -0 為真。 但是，你可能要注意除以 0 的時候：

```js
42 / +0; // Infinity
42 / -0; // -Infinity
```

### BigInt 類型

### 字符串類型

JavaScript 的字符串類型用於表示文本數據。它是一組 16 位的無符號整數值的 “元素”。在字符串中的每個元素佔據了字符串的位置。第一個元素的索引為 0，下一個是索引 1，依此類推。字符串的長度是它的元素的數量。

不同於類 C 語言，JavaScript 字符串是不可更改的。這意味著字符串一旦被創建，就不能被修改。但是，可以基於對原始字符串的操作來創建新的字符串。例如：

- 獲取一個字符串的子串可通過選擇個別字母或者使用 String.substr()。
- 兩個字符串的連接使用連接操作符 (+) 或者 String.concat()。

### 符號類型

符號 (Symbols) 是 ECMAScript 第 6 版新定義的。符號類型是唯一的並且是不可修改的，並且也可以用來作為 Object 的 key 的值 (如下)。在某些語言當中也有類似的原子類型 (Atoms)。你也可以認為為它們是 C 裡面的枚舉類型。

### JavaScript 中的基本類型包裝對象

除了 null 和 undefined 之外，所有基本類型都有其對應的包裝對象：

- String 為字符串基本類型。
- Number 為數值基本類型。
- Boolean 為布爾基本類型。
- Symbol 為字面量基本類型。

這個包裹對象的 function valueOf() { [native code] }() 方法返回基本類型值。

## 對象

在計算機科學中，對象是指內存中的可以被標識符引用的值。

### 屬性

在 Javascript 裡，對象可以被看作是一組屬性的集合。用對象字面量語法來定義一個對象時，會自動初始化一組屬性。（也就是說，你定義一個 var a = "Hello"，那麼 a 本身就會有 a.substring 這個方法，以及 a.length 這個屬性，以及其它；如果你定義了一個對象，var a = {}，那麼 a 就會自動有 a.function hasOwnProperty() { [native code] } 及 a.function Object() { [native code] } 等屬性和方法。）而後，這些屬性還可以被增減。屬性的值可以是任意類型，包括具有複雜數據結構的對象。屬性使用鍵來標識，它的鍵值可以是一個字符串或者符號值（Symbol）。

### "標準的" 對象, 和函數

一個 Javascript 對象就是鍵和值之間的映射。鍵是一個字符串（或者 Symbol），值可以是任意類型的值。這使得對象非常符合哈希表。

函數是一個附帶可被調用功能的常規對象。

### 有序集：數組和類型數組

數組是一種使用整數作為鍵 (integer-key-ed) 屬性和長度 (length) 屬性之間關聯的常規對象。此外，數組對象還繼承了 Array.prototype 的一些操作數組的便捷方法。例如, indexOf (搜索數組中的一個值) or push (向數組中添加一個元素)，等等。 這使得數組是表示列表或集合的最優選擇。

類型數組 (Typed Arrays) 是 ECMAScript Edition 6 中新定義的 JavaScript 內建對象，提供了一個基本的二進制數據緩衝區的類數組視圖。

### 鍵控集: Maps, Sets, WeakMaps, WeakSets

### 結構化數據: JSON

JSON (JavaScript Object Notation) 是一種輕量級的數據交換格式，來源於 JavaScript 同時也被多種語言所使用。 JSON 用於構建通用的數據結構。

### 標準庫中更多的對象

JavaScript 有一個內置對象的標準庫。

#### 標準內置對象的分類

##### 值屬性

這些全局屬性返回一個簡單值，這些值沒有自己的屬性和方法。

- Infinity
- NaN
- undefined
- null 字面量
- globalThis

##### 函數屬性

全局函數可以直接調用，不需要在調用時指定所屬對象，執行結束後會將結果直接返回給調用者。

- eval()
- uneval()
- isFinite()
- isNaN()
- parseFloat()
- parseInt()
- decodeURI()
- decodeURIComponent()
- encodeURI()
- encodeURIComponent()
- escape()
- unescape()

##### 基本對象

顧名思義，基本對象是定義或使用其他對象的基礎。基本對象包括一般對象、函數對象和錯誤對象。

- Object
- Function
- Boolean
- Symbol
- Error
- EvalError
- InternalError
- RangeError
- ReferenceError
- SyntaxError
- TypeError
- URIError

##### 數字和日期對象

用來表示數字、日期和執行數學計算的對象。

- Number
- BigInt
- Math
- Date

##### 字符串

用來表示和操作字符串的對象。

- String
- RegExp

##### 可索引的集合對象

這些對象表示按照索引值來排序的數據集合，包括數組和類型數組，以及類數組結構的對象。

- Array
- Int8Array
- Uint8Array
- Uint8ClampedArray
- Int16Array
- Uint16Array
- Int32Array
- Uint32Array
- Float32Array
- Float64Array
- BigInt64Array
- BigUint64Array

##### 使用鍵的集合對象

這些集合對象在存儲數據時會使用到鍵，支持按照插入順序來迭代元素。

- Map
- Set
- WeakMap
- WeakSet

##### 結構化數據

這些對象用來表示和操作結構化的緩衝區數據，或使用 JSON （JavaScript Object Notation）編碼的數據。

- ArrayBuffer
- SharedArrayBuffer
- Atomics
- DataView
- JSON

##### 控制抽象對象

- Promise
- Generator
- GeneratorFunction
- AsyncFunction

##### 反射

- Reflect
- Proxy

##### 國際化

為了支持多語言處理而加入 ECMAScript 的對象。

- Intl
- Intl.Collator
- Intl.DateTimeFormat
- Intl.ListFormat
- Intl.NumberFormat
- Intl.PluralRules
- Intl.RelativeTimeFormat
- Intl.Locale

##### WebAssembly

- WebAssembly
- WebAssembly.Module
- WebAssembly.Instance
- WebAssembly.Memory
- WebAssembly.Table
- WebAssembly.CompileError
- WebAssembly.LinkError
- WebAssembly.RuntimeError

##### 其他

- arguments

## 参考

- https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects
