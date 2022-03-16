# 箭头函数和this指针

## 1. 箭头函数

### 1-1 什么是箭头函数

箭头函数时 `ES6` 新增的特性， 通过使用 `=>` 定义函数的新语法

```js
let func = (arg1, arg2, ..., argN) => expression;
```

* `arg1..argN` 函数参数
* `expression` 函数返回值

`ES5` vs `ES6` 书写对比：

```js
// es5
function fn(a) {
    return a;
};

// 箭头函数
let fn = (a) => a ;
```



### 1-2 箭头函数使用

* 只有一个参数，还可以省略掉参数外的圆括号，使代码更短
* 如果没有参数，括号将是空的，括号必须保留
* 当有多个参数时，在圆括号内定义多个参数用逗号分隔
* 如果箭头函数的代码块部分有多条语句，就要使用大括号将它们括起来，并且使用 `return` 语句返回
* 箭头函数不支持重命名函数参数
* 箭头函数在参数和箭头之间不能换行
* 大括号 `{}` 被解释为代码块，箭头函数返回一个对象，必须在对象外面加上括号，否则会报错

```js
 // 无参数时括号必须保留
 let fn = () => 1;

 // 只有一个参数可省略参数括号
 let fn1 = a => a;

 // 有多个参数用, 隔开
 let fn2 = (a, b, c) => a + b + c;

 // 箭头函数返回有多条语句时。用大括号抱起来，然后return
 let fn3 = (a, b) => {
 	console.log(a + b);
 	return a * b;
 }
 console.log(fn3(5, 5)); // 25
 // 不支持重名函数参数
 let fn4 = (a, a) => a + a;
 cosnole.log(fn4(1, 3)); // Uncaught SyntaxError: Duplicate parameter name not allowed in this context

 // 参数和箭头之间不能换行
 let fn5 = (x, y) => x + y;
 console.log(fn5(7, 8)); //  Unexpected token '=>'

// {} 大括号包裹为代码块
let fn6 = (a, b) => {a: b};
console.log(fn6(name, 'jsx')); // undefined

let fn7 = (a, b) => ({name: b});
console.log(fn7('name', 'jsx')); // {name: 'jsx'}
```



### 1-3 箭头函数特性

* 不可以当作构造函数，使用 `new` 调用箭头函数会报错
* 没有 `arguments` 对象，可以用 `rest` 参数代替
* 不可以使用 `yield` 命令，因此箭头函数不能用作`Generator` 函数
* 箭头函数没有自己的 `this` 对象

```js

```



### 1-4 箭头函数的 this

* 箭头函数没有 `prototype` 原型，所以箭头函数本身没有 `this`
* 箭头函数的 `this` 指向在定义的时候继承自外层第一个普通函数的 `this`
* 不能直接修改箭头函数的 `this` 指向
* 箭头函数外层没有普通函数，`this` 都会指向`window`全局对象
* call 、 apply 、 bind 无法改变箭头函数中 `this` 的指向

```
```





## 2. this 指向

### 2-1 什么是 this 指针



### 2-2 this 指针规则

