# 語法


## 指令式編程

In computer science, imperative programming is a programming paradigm that uses statements that change a program's state. In much the same way that the imperative mood in natural languages expresses commands, an imperative program consists of commands for the computer to perform. Imperative programming focuses on describing how a program operates.

The term is often used in contrast to declarative programming, which focuses on what the program should accomplish without specifying how the program should achieve the result.

## 宣告式編程

In computer science, declarative programming is a programming paradigm—a style of building the structure and elements of computer programs—that expresses the logic of a computation without describing its control flow.[1]

Many languages that apply this style attempt to minimize or eliminate side effects by describing what the program must accomplish in terms of the problem domain, rather than describe how to accomplish it as a sequence of the programming language primitives[2] (the how being left up to the language's implementation). This is in contrast with imperative programming, which implements algorithms in explicit steps.

## 函數式編程

In computer science, functional programming is a programming paradigm—a style of building the structure and elements of computer programs—that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data. It is a declarative programming paradigm in that programming is done with expressions or declarations[1] instead of statements. Functional code is idempotent: a function's return value depends only on its arguments, so calling a function with the same value for an argument always produces the same result. This is in contrast to imperative programming where, in addition to a function's arguments, global program state can affect a function's resulting value. Eliminating side effects, that is, changes in state that do not depend on the function inputs, can make understanding a program easier, which is one of the key motivations for the development of functional programming.


## 語句和聲明

JavaScript 應用程序是由許多語法正確的語句組成的。單個語句可以跨多行。如果每個語句用分號隔開，那麼多個語句可以在一行中出現。

### 控制流程

#### `block`

一個塊語句可以用來管理零個或多個語句。該區塊是由一對大括號分隔。

通過 var 聲明的變量沒有塊級作用域。

使用 let 和 const 聲明的變量是有塊級作用域的。

函數聲明同樣被限制在聲明他的語句塊內：

```js
foo('outside');  // TypeError: foo is not a function
{
  function foo(location) {
   console.log('foo is called' + location);
  }
  foo('inside'); // 正常工作並且打印'foo is called inside'
}
```

#### `break`

break 語句中止當前循環，switch 語句或 label 語句，並把程序控制流轉到緊接著被中止語句後面的語句。

break 語句包含一個可選的標籤，可允許程序擺脫一個被標記的語句。break 語句需要內嵌在引用的標籤中。被標記的語句可以是任何 塊語句；不一定是循環語句。

> 語法：`break [label];`

- `label`：可選。與語句標籤相關聯的標識符。如果 break 語句不在一個循環或 switch 語句中，則該項是必須的。

下面的代碼中一起使用 break 語句和被標記的塊語句。一個 break 語句必須內嵌在它引用的標記中。注意，inner_block 內嵌在 outer_block 中。

```js
outer_block:{

  inner_block:{
    console.log ('1');
    break outer_block;      // breaks out of both inner_block and outer_block
    console.log (':-(');    // skipped
  }

  console.log ('2');        // skipped
}
```

#### `switch`

> 語法：
> switch (expression) {
    case value1:
      // 當 expression 的結果與 value1 匹配時，執行此處語句
      [break;]
    case value2:
      // 當 expression 的結果與 value2 匹配時，執行此處語句
      [break;]
    ...
    case valueN:
      // 當 expression 的結果與 valueN 匹配時，執行此處語句
      [break;]
    [default:
      // 如果 expression 與上面的 value 值都不匹配時，執行此處語句
    [break;]]
  }

一個 switch 語句首先會計算其 expression 。然後，它將尋找到一個與其表達式值相等的子句，執行相關語句。如果沒有 case 子句相匹配，程序會執行 default 子句。若沒有 default 子句，程序將繼續執行直到 switch 結束。

可選的 break 語句確保程序立即從相關的 case 子句中跳出 switch 並接著執行 switch 之後的語句。若 break 被省略，程序會繼續執行 switch 語句中的下一條語句，不論 case 是否匹配。

```js
var foo = 1;
var output = 'Output:';
switch (foo) {
  case 10:
    output += 'So';
  case 1:
    output += 'What';
    output += 'Is';
  case 2:
    output += 'Your';
  case 3:
    output += 'Name';
  case 4:
    output += '?';
    console.log(output);
    break;
  case 5:
    output += '!';
    console.log(output);
    break;
  default:
    console.log('Please pick a number from 0 to 6!');
}
```

輸出：


| Value	                                | Log text |
| ------------------------------------- | -------- |
| foo is NaN or not 1, 2, 3, 4, 5 or 10	| Please pick a number from 0 to 6! |
| 10	                                  | Output: So What Is Your Name? |
| 1	                                    | Output: What Is Your Name? |
| 2                                     | Output: Your Name? |
| 3	                                    | Output: Name? |
| 4	                                    | Output: ? |
| 5	                                    | Output: ! |

#### `continue`

continue 語句結束當前（或標籤）的循環語句的本次迭代，並繼續執行循環的下一次迭代。

> 語法：`continue [label]`;

與 break 語句的區別在於， continue 並不會終止循環的迭代，而是：

- 在 while 循環中，控制流跳轉回條件判斷；
- 在 for 循環中，控制流跳轉到更新語句。

continue 語句可以包含一個可選的標號以控制程序跳轉到指定循環的下一次迭代，而非當前循環。此時要求 continue 語句在對應的循環內部。

下述例子展示了 while 循環中 continue 語句的使用。當循環到 i 的值為 3 時，執行 continue 。 n 的值在幾次迭代後分別為 1, 3, 7 和 12 ．

```js
i = 0;
n = 0;
while (i < 5) {
   i++;
   if (i === 3) {
      continue;
   }
   n += i;
}
```

#### 空語句

> 語法：;

空語句是一個分號（;），表示不會執行任何語句，即使 JavaScript 語法需要一個語句。

空語句有時與循環語句一起使用。以下示例使用空循環體：

```js
var arr = [1, 2, 3];

// Assign all array values to 0
for (let i = 0; i < arr.length; arr[i++] = 0) /* empty statement */ ;

console.log(arr)
// [0, 0, 0]
```

#### `if...else`

要在一個從句中執行多條語句，可使用語句塊（{...}）。通常情況下，一直使用語句塊是個好習慣，特別是在涉及嵌套 if 語句的代碼中：

```js
if (condition) {
   statements1
} else {
   statements2
}
```

#### `throw`

throw 語句用來拋出一個用戶自定義的異常。當前函數的執行將被停止（throw 之後的語句將不會執行），並且控制將被傳遞到調用堆棧中的第一個 catch 塊。如果調用者函數中沒有 catch 塊，程序將會終止。

```js
function getRectArea(width, height) {
  if (isNaN(width) || isNaN(height)) {
    throw "Parameter is not a number!";
  }
}

try {
  getRectArea(3, 'A');
}
catch(e) {
  console.log(e);
  // expected output: "Parameter is not a number!"
}
```

#### `try...catch`

try...catch 語句將能引發錯誤的代碼放在 try 塊中，並且對應一個響應，然後有異常被拋出。

### 聲明

#### `var`

var 聲明語句聲明一個變量，並可選地將其初始化為一個值。

```js
var x = 0;

function f(){
  var x = y = 1; // x 在函數內部聲明，y 不是！
}
f();

console.log(x, y); // 0, 1
// x 是全局變量。
// y 是隱式聲明的全局變量。
```

```js
var x = 0;  // x 是全局變量，並且賦值為 0。

console.log(typeof z); // undefined，因為 z 還不存在。

function a() { // 當 a 被調用時，
  var y = 2;   // y 被聲明成函數 a 作用域的變量，然後賦值成 2。

  console.log(x, y);   // 0 2

  function b() {       // 當 b 被調用時，
    x = 3;  // 全局變量 x 被賦值為 3，不生成全局變量。
    y = 4;  // 已存在的外部函數的 y 變量被賦值為 4，不生成新的全局變量。
    z = 5;  // 創建新的全局變量 z，並且給 z 賦值為 5。
  }         // (在嚴格模式下（strict mode）拋出 ReferenceError)

  b();     // 調用 b 時創建了全局變量 z。
  console.log(x, y, z);  // 3 4 5
}

a();                   // 調用 a 時同時調用了 b。
console.log(x, z);     // 3 5
console.log(typeof y); // undefined，因為 y 是 a 函數的本地（local）變量。
```

#### let

let 語句聲明一個塊級作用域的本地變量，並且可選的將其初始化為一個值。

模擬私有接口：

```js
var SomeConstructor;

{
    let privateScope = {};

    SomeConstructor = function SomeConstructor () {
        this.someProperty = "foo";
        privateScope.hiddenProperty = "bar";
    }

    SomeConstructor.prototype.showPublic = function () {
        console.log(this.someProperty); // foo
    }

    SomeConstructor.prototype.showPrivate = function () {
        console.log(privateScope.hiddenProperty); // bar
    }

}

var myInstance = new SomeConstructor();

myInstance.showPublic();
myInstance.showPrivate();

console.log(privateScope.hiddenProperty); // error
```

在同一個函數或塊作用域中重複聲明同一個變量會引起 SyntaxError。

在 switch 語句中只有一個塊，你可能因此而遇到錯誤：

```js
let x = 1;
switch(x) {
  case 0:
    let foo;
    break;

  case 1:
    let foo; // SyntaxError for redeclaration.
    break;
}
```

let 被創建在包含該聲明的（塊）作用域頂部，一般被稱為 “提升”。與通過  var 聲明的有初始化值 undefined 的變量不同，通過 let 聲明的變量直到它們的定義被執行時才初始化。在變量初始化前訪問該變量會導致 ReferenceError。該變量處在一個自塊頂部到初始化處理的 “暫存死區” 中。

```js
function do_something() {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError
  var bar = 1;
  let foo = 2;
}
```

#### `const`

常量是塊級作用域，很像使用 let 語句定義的變量。常量的值不能通過重新賦值來改變，並且不能重新聲明。

### 函數和類

#### `function`

函數聲明定義一個具有指定參數的函數。

> 語法：
> function name([param,[, param,[..., param]]]) {
   [statements]
}

#### `async function`

async function 用來定義一個返回 AsyncFunction 對象的異步函數。異步函數是指通過事件循環異步執行的函數，它會通過一個隱式的 Promise 返回其結果。如果你在代碼中使用了異步函數，就會發現它的語法和結構會更像是標準的同步函數。

```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('resolved');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  var result = await resolveAfter2Seconds();
  console.log(result);
  // expected output: 'resolved'
}

asyncCall();

> "calling"
> "resolved"
```

#### `class`

class 聲明創建一個基於原型繼承的具有給定名稱的新類。

> 語法：
> class name [extends] {
  // class body
}

在下面的例子中，我們首先定義一個名為 Polygon 的類，然後繼承它來創建一個名為 Square 的類。注意，構造函數中使用的 super() 只能在構造函數中使用，並且必須在使用 this 關鍵字前調用。

```js
class Polygon {
  function Object() { [native code] }(height, width) {
    this.name = 'Polygon';
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  function Object() { [native code] }(length) {
    super(length, length);
    this.name = 'Square';
  }
}
```

#### `return`

return 語句終止函數的執行，並返回一個指定的值給函數調用者。

> 語法：`return [[expression]]; `

自動插入分號（ASI）規則會影響 return 語句。在 return 關鍵字和被返回的表達式之間不允許使用行終止符。

### 迭代器

#### `do...while`

do...while 語句創建一個執行指定語句的循環，直到 condition 值為 false。在執行 statement 後檢測 condition，所以指定的 statement 至少執行一次。

> 語法：
> do
   statement
while (condition);

#### `while`

while 語句可以在某個條件表達式為真的前提下，循環執行指定的一段代碼，直到那個表達式不為真時結束循環。

```js
while (length--) {
  if (eq(array[length][0], key)) {
    return length
  }
}
```

#### `for`

for 語句用於創建一個循環，它包含了三個可選的表達式，三個可選的表達式包圍在圓括號中並由分號分隔，後跟一個在循環中執行的語句（通常是一個塊語句）。

#### `for...in`

for...in 語句以任意順序遍歷一個對象的除 Symbol 以外的可枚舉屬性。

如果你只要考慮對象本身的屬性，而不是它的原型，那麼使用 getOwnPropertyNames() 或執行 function hasOwnProperty() { [native code] }() 來確定某屬性是否是對象本身的屬性（也能使用 propertyIsEnumerable ）。或者，如果你知道不會有任何外部代碼干擾，您可以使用檢查方法擴展內置原型。

```js
var triangle = {a: 1, b: 2, c: 3};

function ColoredTriangle() {
  this.color = 'red';
}

ColoredTriangle.prototype = triangle;

var obj = new ColoredTriangle();

for (var prop in obj) {
  if (obj.function hasOwnProperty() { [native code] }(prop)) {
    console.log(`obj.${prop} = ${obj[prop]}`);
  }
}

// Output:
// "obj.color = red"
```

#### `for..of`

for...of 語句在可迭代對象（包括 Array，Map，Set，String，TypedArray，arguments 對象等等）上創建一個迭代循環，調用自定義迭代鉤子，併為每個不同屬性的值執行語句

`for...of` 與 `for...in` 的區別：

```js
Object.prototype.objCustom = function() {};
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
  if (iterable.function hasOwnProperty() { [native code] }(i)) {
    console.log(i); // logs 0, 1, 2, "foo"
  }
}

for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
```

### 其他

#### `label`

標記語句可以和 break 或 continue 語句一起使用。標記就是在一條語句前面加個可以引用的標識符（identifier）。

```js
var str = "";

loop1:
for (var i = 0; i < 5; i++) {
  if (i === 1) {
    continue loop1;
  }
  str = str + i;
}

console.log(str);
// expected output: "0234"
```

在 for 循環中使用帶標記的 break：

```js
var i, j;

loop1:
for (i = 0; i < 3; i++) {      //The first for statement is labeled "loop1"
   loop2:
   for (j = 0; j < 3; j++) {   //The second for statement is labeled "loop2"
      if (i == 1 && j == 1) {
         break loop1;
      }
      console.log("i =" + i + ", j =" + j);
   }
}

// Output is:
//   "i = 0, j = 0"
//   "i = 0, j = 1"
//   "i = 0, j = 2"
//   "i = 1, j = 0"
// Notice the difference with the previous continue example
```

#### `debugger`

#### `export`

在創建 JavaScript 模塊時，export 語句用於從模塊中導出函數、對象或原始值，以便其他程序可以通過 import 語句使用它們。

無論您是否聲明，導出的模塊都處於嚴格模式。 export 語句不能用在嵌入式腳本中。

```js
// 導出單個特性
export let name1, name2, …, nameN; // also var, const
export let name1 = …, name2 = …, …, nameN; // also var, const
export function FunctionName(){...}
export class ClassName {...}

// 導出列表
export {name1, name2, …, nameN};

// 重命名導出
export {variable1 as name1, variable2 as name2, …, nameN};

// 默認導出
export default expression;
export default function (…) { … } // also class, function*
export default function name1(…) { … } // also class, function*
export {name1 as default, …};

// Aggregating modules
export * from …;
export {name1, name2, …, nameN} from …;
export {import1 as name1, import2 as name2, …, nameN} from …;
export {default} from …;
```

#### `import`

## 表達式和運算符

### 主要表達式

#### `this`

#### `function`

函數表達式

#### `class`

類表達式

#### `function*`

function* 關鍵字定義了一個 generator 函數表達式。

#### `yield`

暫停和恢復 generator 函數。

#### `await`

暫停或恢復執行異步函數，並等待 promise 的 resolve/reject 回調。

#### `[]`

數組初始化 / 字面量語法。

#### `{}`

對象初始化 / 字面量語法。

#### `/ab+c/i`

正則表達式字面量語法。

#### `()`

分組操作符。

圓括號運算符 ( ) 用於控制表達式中的運算優先級。

### 左表達式

左邊的值是賦值的目標。

#### 屬性訪問符

成員運算符提供了對對象的屬性或方法的訪問

(object.property 和 object["property"]).

#### `new`

new 運算符創建了構造函數實例。

#### `new.target`

在構造器中，new.target 指向 new 調用的構造器。

#### `super`

super 關鍵字調用父類的構造器.

#### `...obj`

展開運算符可以將一個可迭代的對象在函數調用的位置展開成為多個參數, 或者在數組字面量中展開成多個數組元素。

### 自增和自減節

前置 / 後置自增運算符和前置 / 後置自減運算符.

#### `A++`

後置自增運算符.

#### `A--`

後置自減運算符.

#### `++A`

前置自增運算符.

#### `--A`

前置自減運算符.

### 一元運算符節

一元運算符只有一個操作數.

#### `delete`

delete 運算符用來刪除對象的屬性.

#### `void`

void 運算符表示表達式放棄返回值.

#### `typeof`

typeof 運算符用來判斷給定對象的類型.

#### `+`

一元加運算符將操作轉換為 Number 類型.

#### `-`

一元減運算符將操作轉換為 Number 類型並取反.

#### `~`

按位非運算符.

#### `!`

邏輯非運算符.

### 算術運算符節

算術運算符以二個數值（字面量或變量）作為操作數，並返回單個數值。

#### `+`

加法運算符.

#### `-`

減法運算符.

#### `/`

除法運算符.

#### `*`

乘法運算符.

#### `%`

取模運算符.

### 關係運算符節

比較運算符比較二個操作數並返回基於比較結果的 Boolean 值。

#### `in`

in 運算符用來判斷對象是否擁有給定屬性.

#### `instanceof`

instanceof 運算符判斷一個對象是否是另一個對象的實例.

#### `<`

小於運算符

#### `>`

大於運算符.

#### `<=`

小於等於運算符.

#### `>=`

大於等於運算符。

### 相等運算符節

如果相等，操作符返回的是 Boolean(布爾) 類型的 true，否則是 false。

#### `==`

相等 運算符.

#### `!=`

不等 運算符.

#### `===`

全等 運算符.

#### `!==`

非全等 運算符.

### 位移運算符節

在二進制的基礎上對數字進行移動操作

#### `<<`

按位左移運算符。

#### `>>`

按位右移運算符。

#### `>>>`

按位無符號右移運算符。

### 二進制位運算符節

二進制運算符將它們的操作數作為 32 個二進制位（0 或 1）的集合，並返回標準的 JavaScript 數值。

#### `&`

二進制位與（AND）。

#### `|`

二進制位或（OR）。

#### `^`

二進制位異或（XOR）。

### 二元邏輯運算符節

邏輯運算符典型的用法是用於 boolean(邏輯) 值運算, 它們返回 boolean 值。

#### `&&`

邏輯與.

#### `||`

邏輯或.

### 條件 (三元) 運算符節

#### `(condition ? ifTrue : ifFalse)`

條件元素運算符把兩個結果中其中一個符合運算邏輯的值返回。

### 賦值運算符節
賦值元素符會將右邊的操作數的值分配給左邊的操作數，並將其值修改為右邊操作數相等的值。

#### `=`

賦值運算符。

#### `*=`
賦值乘積。

#### `/=`

賦值商。

#### `%=`

賦值求餘。

#### `+=`

賦值求和。

#### `-=`

賦值求差。

#### `<<=`

左位移。

#### `>>=`

右位移。

#### `>>>=`

無符號右位移。

#### `&=`

賦值與。

#### `^=`

賦值按位異或。

#### `|=`

賦值或。

#### `[a, b] = [1, 2]`
#### `{a, b} = {a:1, b:2}`

解構賦值允許你分配數組或者對象變量的屬性通過使用規定的語法，其看起來和數組和對象字面量很相似。

### 逗號操作符節

#### `,`

逗號操作符允許在一個判斷狀態中有多個表達式去進行運算並且最後返回最後一個表達式的值。

## 參考

- https://zh.wikipedia.org/wiki/%E5%AE%A3%E5%91%8A%E5%BC%8F%E7%B7%A8%E7%A8%8B
- https://wiwiwiki.kfd.me/wiki/%E6%8C%87%E4%BB%A4%E5%BC%8F%E7%B7%A8%E7%A8%8B
- https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Expressions_and_Operators
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators