# Event Loop



## 1. 定时器

- `setTimeout` 允许我们将函数推迟到一段时间间隔之后再执行
- `setInterval` 允许我们重复运行一个函数，从一段时间间隔之后开始运行，之后以该时间间隔连续重复运行该函数
- 定时器 `setTimeout/setinterval` 中的 `this` 都指向 `window`



### 1-1 setTimeout

`setTimeout(func|code, delay, arg1, arg2, ...)`

在指定毫秒数后执行代码或者函数

* `dunc|code` 想要执行的函数或代码
* `delay` 执行前的延时，以毫秒为单位（1000 毫秒 = 1 秒），默认值是 0
* `arg1, arg2, ...` 要传入被执行函数或代码的参数列表

```js
let timeId = setTimeout(func|code, delay, arg1, arg2, ...)
```

```js
// setTimeout 在指定毫秒数后执行
let timeId = setTimeout((...name) => {
  // 可以使用箭头函数
  console.log('执行')
  // 传入参数
  console.log(name); // ['jsx', 'ljj']
}, 1000, 'jsx', 'ljj')
```



### 1-2 取消setTimeout

`setTimeout` 在调用时会返回一个定时器标识符，可以使用 `clearTimeout` 来取消定时器执行调用

>  **Tips：**如果在创建定时器时没有名字，则定时器无法清除

```js
let timerId = setTimeout(...);
```

```js
let timeId = setTimeout(function() {
  console.log('执行')
}, 1000)
// 定时器不会执行
clearTimeout(timeId)
```



### 1-3 嵌套setTimeout

嵌套的 `setTimeout` 可以实现与 `setInterval` 相同的循环执行调用

```js
let timeId = setTimeout(function run() {
  console.log('执行')
  timeId = setTimeout(run, 1000)
}, 1000)
```



### 1-4 setInterval

`setInterval(func|code, delay, arg1, arg2, ...)`

每间隔指定的时间周期性执行代码或者函数，定时器不被清除，代码或者函数会无限执行

* `dunc|code` 想要执行的函数或代码
* `delay` 执行前的延时，以毫秒为单位（1000 毫秒 = 1 秒），默认值是 0
* `arg1, arg2, ...` 要传入被执行函数或代码的参数列表

```js
let timerId = setInterval(func|code, delay, arg1, arg2, ...)
```



### 1-5 取消setInterval

`setInterval` 在调用时会返回一个定时器标识符，可以使用 `clearInterval` 来取消定时器执行调用

```js
let interval = setInterval(function() {
  console.log('执行')
}, 1000)
// 定时器标识符清除定时器
clearInterval(interval)
```



### 1-5 定时器垃圾回收

当一个函数传入 `setInterval/setTimeout` 时，将为其创建一个内部引用，并保存在调度程序中，即使这个函数没有其他引用，也能防止垃圾回收器（GC）将其回收

可以通过定时器标识符取消定时器，来使得定时器传入的函数被垃圾回收

```js
let timeId = setTimeout(time, 1000)

function time() {
  console.log('执行')
}
// 取消定时器
clearTimeout(timeId)

let interval = setInterval(time, 1000)
// 取消定时器
clearInterval(interval)
```



## 2. 线程

### 2-1 单线程

`JavaScript` 是单线程的，任务需要排队执行

`JavaScript` 的主要用途是与用户互动，以及操作 `DOM` 如果是多线程的，则会带来很复杂的同步问题， `DOM` 节点内容两个线程不同操作，浏览器执行问题，所以为了避免复杂性，`JavaScript` 核心特征就是单线程



### 2-2 多线程

浏览器是多进程的，`GUI` 渲染线程和 `JS` 引擎线程是不能同时进行的，渲染线程在执行任务的时候，`JS` 引擎线程会被挂起

* `GUI` 渲染线程 —— 负责渲染浏览器界面，解析 `HTML`，`CSS`
* `JS` 引擎线程 —— 解析和执行 `JS` 脚本
* 事件触发线程 —— 控制事件循环，管理事件任务队列
* 定时器触发线程 —— 处理 `setInterval` 与 `setTimeout` 定时器
* 异步 `http` 请求线程 —— 处理异步 `http` 请求



## 3. 任务队列

### 3-1 执行栈与主线程

* 执行栈 —— 当触发某个事件时，该事件触发就会进入任务队列等待主线程读取，执行任务队列中的某个任务时，这个被执行的任务就称为执行栈

* 主线程 —— 规定现在执行执行栈中的哪个事件，主线程会不停的从执行栈中读取事件，会执行完所有栈中的同步代码，当主线程将执行栈中所有的代码执行完之后，主线程将会去查看任务队列是否有任务，将可运行的异步任务添加到执行栈中，开始执行

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220503211538.png)



### 3-2 同步与异步任务

* 同步任务：在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务
* 异步任务 —— 不进入主线程、而进入任务队列的任务，只有任务队列通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220504090347.png)



### 3-3 任务队列

主线程之外，事件触发线程管理着一个任务队列，只要异步任务有了运行结果，就在任务队列之中放一个事件回调

执行栈中的所有同步任务执行完毕，浏览器就会读取任务队列，将可运行的异步任务添加到执行栈中，开始执行

* 首先，执行栈开始顺序执行
* 判断是否为同步，异步则进入异步线程，最终事件回调给事件触发线程的任务队列等待执行，同步继续执行
* 执行栈空，询问任务队列中是否有事件回调
* 任务队列中有事件回调则把回调加入执行栈末尾继续从第一步开始执行
* 任务队列中没有事件回调则不停发起询问

```js
let timeId = setTimeout(function() {
  console.log('定时器')
}, 1000)

console.log('console')

let result = new Promise(resolve => {
  console.log('Promise resolve')
  resolve()
})

// 异步请求
result.then(() => {
  console.log('Promise then')
})

// console
// Promise resolve
// Promise then
// 定时器
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220503222546.png)





## 4. 宏任务与微任务

异步任务 —— 异步任务分为宏任务（macrotask ）和微任务（microtask ）



### 4-1 宏任务

宏任务 —— 可以将每次执行栈执行的代码当做是一个宏任务， 每一个宏任务会从头到尾执行完毕，不会执行其它

* 每个宏任务之后，引擎会立即执行微任务队列中的所有任务，然后再执行其他的宏任务，或渲染，或进行其他任何操作
* 浏览器为了能够使得使 `宏任务` 和 `DOM任务` 能够有序的执行，会在一个宏任务执行结束后，在下一个宏任务执行开始前，对页面进行重新渲染
* 常见的宏任务 —— 主代码块 `script`、`setTimeout`、`setInterval`、`setImmediate() —— Node`、浏览器 `requestAnimationFrame()`

```js
// 执行顺序
宏任务 -> GUI渲染 -> 宏任务 -> ...
```



### 4-2 微任务

微任务 —— 在当前任务执行结束后立即执行的任务，在当前任务后，下一个任务之前，在渲染之前

* 微任务优先级会高于宏任务
* 在一个宏任务执行完后，会在渲染前将在执行期间产生的所有微任务都执行完毕
* 常见的微任务 —— `Promise.then()`、`Promise.catch`、`Promise.finally`、`process.nextTick() —— Node`、`Object.observe`、`MutationObserver`

```js
// 执行顺序
宏任务 -> 微任务 -> GUI渲染 -> 宏任务 -> ...
```



### 4-3 执行流程

* 整段脚本 `script` 作为宏任务开始执行
* 遇到微任务将其推入微任务队列，宏任务推入宏任务队列
* 宏任务执行完毕，检查有没有可执行的微任务
* 发现有可执行的微任务，将所有微任务执行完毕
* 当前宏任务执行完毕，开始检查渲染，然后 `GUI` 线程接管渲染
* 渲染完毕后，`JS` 线程继续接管，开始新的宏任务，反复如此直到所有任务执行完毕

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220503212546.png)



## 5. 事件循环

### 5-1 事件循环流程

* 首先，整体的 `script`(作为第一个宏任务)开始执行的时候，会把所有代码分为`同步任务`、`异步任务`两部分
* 同步任务会直接进入主线程依次执行
* 异步任务会再分为宏任务和微任务
* 宏任务进入到 `Event Table` 事件列表中，并在里面注册回调函数，每当指定的事件完成时，事件列表会将这个函数移到 `Event Queue` 宏任务队列中
* 微任务也会进入到另一个 `Event Table` 事件列表中，并在里面注册回调函数，每当指定的事件完成时，事件列表会将这个函数移到 `Event Queue` 微任务队列中
* 当主线程内的任务执行完毕，主线程为空时，会检查微任务的 `Event Queue` 事件队列，如果有任务，就全部执行，如果没有就执行下一个宏任务
* 上述过程会不断重复，这就是 `Event Loop`，比较完整的事件循环

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220504091805.png)



### 5-2 Promise和async/await

* `new Promise(() => {}).then()` ，前面的 `new Promise()` 这一部分是一个构造函数，这是一个同步任务，后面的 `.then()` 才是一个异步微任务
* `async/await` 本质上还是基于 `Promise` 的一些封装，`await` 以前的代码，相当于与 `new Promise` 的同步代码，`await` 以后的代码相当于 `Promise.then` 的异步微任务
* `await` 后面的表达式会先执行一遍，将 `await` 后面的代码加入到 `microtask` 微任务中，然后就会跳出整个 `async` 函数来执行后面的代码

```js
// promise代码是一个同步任务
// .then、.catch、.finally是微任务
console.log('a')
let promise = new Promise(function(resolve, reject) {
    console.log('b')
    resolve();
  })
  .then(function() {
    console.log('c')
  })
// a b c
```

```js
// async与await await前面的代码都是同步的 await后面则是微任务
async function func1() {
  console.log('a');
  await func2();
  console.log('b')
}

async function func2() {
  console.log('c')
}
console.log('d')

setTimeout(function() {
  console.log('setTimeout')
}, 0)

func1(); // dacb
```



## 6. 事件循环练习

- 同步任务进入主线程排队，异步任务进入事件队列排队等待被推入主线程执行
- 定时器的延迟时间为 `0` 并不是立刻执行，只是代表相比于其他定时器更早的被执行

### 6-1 练习1

* `setTimeout` 进入宏任务队列，
* `Promise` 创建立即执行，打印 `a b`
* 遇到 `Promise.then` 进入微任务队列
* 遇到 `console.log('d')` 打印 `d`
* 有可执行的微任务，打印 `c`，遇到 `setTimeout`（Promise中的） 将其推入宏任务队列中
* 定时器延迟时间相同，开始按照顺序执行宏任务，分别打印 `setTimeout` `then中的setTimeout`

```js
setTimeout(function(){
 console.log('setTimeout')
}, 0)

const p = new Promise(resolve => {
 console.log('a')
 resolve()
 console.log('b')
})

p.then(() => {
 console.log('c')
 setTimeout(function(){
  console.log('then中的setTimeout')
 }, 0)
})

console.log('d')
// a b d c setTimeout then中的setTimeout
```



### 6-2 练习2

* 打印 `a`
* `promise` 立即执行，打印 `b`
* `promise.then` 推入微任务队列
* `setTimeout` 推入宏任务队列
*  整段代码执行完毕，开始执行微任务，打印 `c` ，遇到 `setTimeout` 推入宏任务队列排队等待执行
* 没有可执行的微任务开始执行宏任务，定时器按照延迟时间排队执行
* 打印 `h j` ，`promise.then` 推入微任务队列
* 有可执行的微任务，打印 `i` ，继续执行宏任务，打印 `d`
* 执行延迟为100的宏任务，打印 `e f`，执行微任务打印 `g`，所有任务执行完毕

```js
console.log('a');

new Promise(resolve => {
    console.log('b')
    resolve()
}).then(() => {
    console.log('c')
    setTimeout(() => {
      console.log('d')
    }, 0)
})

setTimeout(() => {
    console.log('e')
    new Promise(resolve => {
        console.log('f')
        resolve()
    }).then(() => {
        console.log('g')
    })
}, 100)

setTimeout(() => {
    console.log('h')
    new Promise(resolve => {
        resolve()
    }).then(() => {
        console.log('i')
    })
    console.log('j')
}, 0)
// a b c h j i d e f g
```



### 6-3 练习3

* 执行 `console.log('script start')` ，输出 `script start`
* `setTimeout` 推入宏任务队列
* 执行了 `async1()` 函数，`await` 前面代码同步执行输出 `async1 start`，`await` 后面代码推入微任务立即执行输出 `async2` ，然后跳出 `async1` 函数	
* `Promise` 中的函数是立即执行的，而后续的 `.then` 则会被分发到微任务队列中，输出 `promise1`
* 向下继续执行输出 `script end`
* 查找清空微任务队列输出 `async1 end`、`promise2`
* 最后输出 `setTimeout` 定时器宏任务 `setTimeout`

```js
async function async1() {
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}
async function async2() {
  console.log('async2')
}

console.log('script start')

setTimeout(function () {
  console.log('setTimeout')
}, 0)

async1()

new Promise(function (resolve) {
  console.log('promise1')
  resolve()
}).then(function () {
  console.log('promise2')
})
console.log('script end')
// script start, async1 start, async2, promise1, script end, async1 end, promise2, setTimeout
```



