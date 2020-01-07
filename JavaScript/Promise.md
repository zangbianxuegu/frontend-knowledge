## Promise

參考：[前端基础进阶（十三）：透彻掌握Promise的使用，读这篇就够了](https://www.jianshu.com/p/fe5f173276bd)

為了解決回調地獄的問題

利用回調函數：

```js
// 一个简单的封装
function want() {
  console.log('这是你想要执行的代码');
}
function fn(want) {
  console.log('这里表示执行了一大堆各种代码');
  // 其他代码执行完毕，最后执行回调函数
  want && want();
}
fn(want);
```

利用隊列機制：

```js
function want() {
  console.log('这是你想要执行的代码');
}
function fn(want) {
  // 将想要执行的代码放入队列中，根据事件循环的机制，我们就不用非得将它放到最后面了，由你自由选择
  want && setTimeout(want, 0);
  console.log('这里表示执行了一大堆各种代码');
}
fn(want);
```

利用 Promise：

```js
function want() {
  console.log('这是你想要执行的代码');
}
function fn(want) {
  console.log('这里表示执行了一大堆各种代码');
  // 返回Promise对象
  return new Promise(function (resolve, reject) {
    if (typeof want == 'function') {
      resolve(want);
    } else {
      reject('TypeError: ' + want + '不是一个函数')
    }
  })
}
fn(want).then(function (want) {
  want();
})
fn('1234').catch(function (err) {
  console.log(err);
})
// 这里表示执行了一大堆各种代码
// 这里表示执行了一大堆各种代码
// 这是你想要执行的代码
// TypeError: 1234不是一个函数
```

一、Promise 對象有三種狀態：

- pending: 等待中，或者進行中，表示還沒有得到結果
- resolved(Fulfilled): 已經完成，表示得到了我們想要的結果，可以繼續往下執行。
- rejected: 也表示得到結果，但是由於結果並非我們所願，因此拒絕執行。

在 Promise 對象的構造函數中，將第一個函數作為第一個參數，這個函數，就是用來處理 Promise 的狀態變化。

```js
new Promise(function(resolve, reject) {
  if(true) { resolve() };
  if(false) { reject() };
})
```

resolve 和 reject 都為一個函數，他們的作用分別是將狀態修改為 resolved 和 rejected。

二、Promise 對象中的 then 方法，可以接收構造函數中處理的狀態變化，并分別對應執行。then 方法有兩個參數，第一個函數接收 resolved 狀態的執行，第二個函數接收 rejected 狀態的執行。

```js
function fn(num) {
  return new Promise(function (resolve, reject) {
    if (typeof num == 'number') {
      resolve();
    } else {
      reject();
    }
  }).then(function () {
    console.log('参数是一个number值');
  }, function () {
    console.log('参数不是一个number值');
  })
}
fn('hahha');
fn(1234);
// 参数不是一个number值
// 参数是一个number值
```

then 方法的執行結果也會返回一個 Promise 對象，因此可以進行 then 的鏈式執行，`then(null, function() {})` 就等同于 `catch(function() {})`

```js
function fn(num) {
  return new Promise(function (resolve, reject) {
    if (typeof num == 'number') {
      resolve();
    } else {
      reject();
    }
  })
    .then(function () {
      console.log('参数是一个number值');
    })
    .then(null, function () {
      console.log('参数不是一个number值');
    })
}
fn('hahha');
fn(1234);
// 参数是一个number值
// 参数不是一个number值
```

三、Promise 中的數據傳遞


```js
var fn = function (num) {
  return new Promise(function (resolve, reject) {
    if (typeof num == 'number') {
      resolve(num);
    } else {
      reject('TypeError');
    }
  })
}

fn(2).then(function (num) {
  console.log('first: ' + num);
  return num + 1;
})
  .then(function (num) {
    console.log('second: ' + num);
    return num + 1;
  })
  .then(function (num) {
    console.log('third: ' + num);
    return num + 1;
  });
// first: 2
// second: 3
// third: 4
```

Promise 封裝 ajax：

```js
var url = 'https://hq.tigerbrokers.com/fundamental/finance_calendar/getType/2017-02-26/2017-06-10';
// 封装一个 get 请求的方法
function getJSON(url) {
  return new Promise(function (resolve, reject) {
    var XHR = new XMLHttpRequest();
    XHR.open('GET', url, true);
    XHR.send();
    XHR.onreadystatechange = function () {
      if (XHR.readyState == 4) {
        if (XHR.status == 200) {
          try {
            var response = JSON.parse(XHR.responseText);
            resolve(response);
          } catch (e) {
            reject(e);
          }
        } else {
          reject(new Error(XHR.statusText));
        }
      }
    }
  })
}
getJSON(url).then(resp => console.log(resp));
// {ret: 0, serverTime: 1564995908593, items: {…}}
```

四、Promise all

`Promise.all` 接收一個 Promise 對象組成的數組作為參數。當這個數組中所有的 Promise 對象狀態都變成 resolved 或者 rejected 的時候，它才會去調用 then 方法。

```js
var url = 'https://hq.tigerbrokers.com/fundamental/finance_calendar/getType/2017-02-26/2017-06-10';
var url1 = 'https://hq.tigerbrokers.com/fundamental/finance_calendar/getType/2017-03-26/2017-06-10';
function renderAll() {
  return Promise.all([getJSON(url), getJSON(url1)]);
}
renderAll().then(function (value) {
  console.log(value);
})
// (2) [{…}, {…}]
// 0: {ret: 0, serverTime: 1564995908951, items: {…}}
// 1: {ret: 0, serverTime: 1564995908565, items: {…}}
// length: 2
// __proto__: Array(0) 
```

==========================================================

## 待补充

- [MDN：使用 Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)
- [MDN：Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- 《你不知道的 JavaScript 中》

