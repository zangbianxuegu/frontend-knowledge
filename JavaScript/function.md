# 函数


## 函数参数

函数声明一个参数，但是调用函数时可以传入任意数量的参数，也可以什么都不传。
function f1 (param) {}，f1 (p1, p2, p3)，param 只能获取 p1。

### 函数重载

面向对象语言里，方法重载通常是通过在同名方法（但不同参数）里声明不同的实现来达到目的。但在 JavaScript 中，通过传入参数的特性和个数进行相应修改来达到目的。

merge 函数：

```js
function merge (root) {
  for (var i = 1; i < arguments.length; i++) {
    for (var key in arguments[i]) {
      root[key] = arguments[i][key]
    }
  } 
  return root
} 
var merged = merge(
  // 第一个参数
  {
    name: 'Batou'
  },
  // 第二个参数
  {
    city: 'Niihama'
  }
)
```

root 是第一个参数，作为返回，arguments 可以获取其他参数，但是从第二个开始。





