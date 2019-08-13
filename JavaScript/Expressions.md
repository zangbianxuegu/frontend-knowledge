# 表达式

任何可以被求值的代码单元。既然表达式产生值，所以任何期望值的地方都可以使用表达式，比如函数的参数。

there are 3 key parts to that sentence: (1) validity, (2) tokens, and (3) value.

First, validity is not only in syntax, but by its context. For example:

```js
var a = 10;
a;          // this is a valid expression because 
            // `a` has been declared already
b;          // this is an invalid expression because
            // `b` has NOT been declared
```

Second, tokens (see here) describe a single “word” of code. This can be a string/number literal (e.g., "hello", 6), an identifier (a), a punctuator (=), etc. When you combine these tokens in a way that results in a value, you have an expression!

Lastly, value means that the expression results in a specific value. This implies that a valid expression can be substituted by its resulting value without breaking the code.

```js
10;     // results in 10, therefore an expression
var;    // NOT an expression but a keyword (a type of token),
        // results in a SyntaxError!
a;      // An expression, but an invalid one
a = 10; // finally a valid expression, results in 10;
function foo(x) {
  console.log(x);
}
                // because `a = 10` is a valid expression,
                // it can be substituted for its value.
foo(a = 10);    // 10, invoking the function in this manner is
                // permissible, though not that common
```



## 算术表达式

```js
10; // Here 10 is an expression that is evaluated to the numeric value 10 by the JS interpreter
10 + 13; // This is another expression that is evaluated to produce the numeric value 23
```

## 字符串表达式

```js
'hello';
'hello' + 'world'; // evaluates to the string 'hello world'
```

## 逻辑表达式

```js
10 > 9; // evaluates to boolean value true
10 < 20; // evaluates to boolean value false
true; //evaluates to boolean value true
a === 20 && b === 30; // evaluates to true or false based on the values of a and b
```

## 基本表达式

```js
'hello world'; // A string literal
23;            // A numeric literal
true;          // Boolean value true
sum;           // Value of variable sum
this;          // A keyword that evaluates to the current object
```

## Left-hand-side Expressions

```js
// variables such as i and total
i = 10;
total = 0;
// properties of objects
var obj = {}; // an empty object with no properties
obj.x = 10; // an assignment expression
// elements of arrays
array[0] = 20;
array[1] = 'hello';
// Invalid left-hand-side errors
++(a+1); // SyntaxError. Attempting to increment or decrement an expression that is not an lvalue will lead to errors.
```

## 赋值表达式

```js
average = 55;
var b = (a = 1); // here the assignment expression (a = 1) evaluates to a value that is assigned to the variable b. b = (a = 1) is another assignment expression. var is not part of the expression.
```

## 表达式的副作用

```js
sum = 20; // here sum is assigned the value of 20
sum++; // increments the value of sum by 1
function modify(){
  a *= 10;
}
var a = 10;
modify(); // modifies the value of a to 100.
```

# 语句

A statement in JavaScript is the collection of one or more expressions and keywords that completes a valid JavaScript thought.
语句是表达式和关键字组成

statements are the sentences of JavaScript. However, just as I cannot just say “the slowly man here running” and expect people to understand, we cannot just chain some JavaScript expressions together and expect the computer to execute the code as you had intended.

## 声明语句

```js
var sum;
var average;
// In the following example, var total is the statement and total = 0 is an assignment expression
var total = 0;
// A function declaration statement 
function greet(message) {
  console.log(message);
}
```

## 表达式语句

```js
var a = var b; // leads to an error cause you cannot use a statement in the place of an expression
var a = (b = 1); // since (b = 1) is an assignment expression and not a statement, this is a perfectly acceptable line of code
console.log(var a); // results in error as you can pass only expressions as a function argument
```

## 条件语句

## 循环语句


## 函数表达式和函数声明





## 参考

- https://medium.com/launch-school/javascript-expressions-and-statements-4d32ac9c0e74
- https://medium.com/@danparkk/javascript-basics-lexical-grammar-expressions-operators-and-statements-d9a61c7e71a8
- https://dev.to/promhize/javascript-in-depth-all-you-need-to-know-about-expressions-statements-and-expression-statements-5k2
- https://dev.to/promhize/what-you-need-to-know-about-javascripts-automatic-semi-colon-insertion-78a