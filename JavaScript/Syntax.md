# 語法


## 指令式編程



## 宣告式編程



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

let 被創建在包含該聲明的（塊）作用域頂部，一般被稱為 “提升”。與通過  var 聲明的有初始化值 undefined 的變量不同，通過 let 聲明的變量直到它們的定義被執行時才初始化。在變量初始化前訪問該變量會導致 ReferenceError。該變量處在一個自塊頂部到初始化處理的“暫存死區” 中。

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

### 迭代器

#### `do...while`

do...while 語句創建一個執行指定語句的循環，直到 condition 值為 false。在執行 statement 後檢測 condition，所以指定的 statement 至少執行一次。

> 語法：
> do
   statement
while (condition);

#### `while`

```js
while (length--) {
  if (eq(array[length][0], key)) {
    return length
  }
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



## 運算符和表達式



## 參考

- https://zh.wikipedia.org/wiki/%E5%AE%A3%E5%91%8A%E5%BC%8F%E7%B7%A8%E7%A8%8B
- https://wiwiwiki.kfd.me/wiki/%E6%8C%87%E4%BB%A4%E5%BC%8F%E7%B7%A8%E7%A8%8B
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Expressions_and_Operators
