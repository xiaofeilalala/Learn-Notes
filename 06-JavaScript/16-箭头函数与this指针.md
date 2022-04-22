# 箭头函数和this指针

## 1. 箭头函数

### 1-1 什么是箭头函数

箭头函数是 `ES6` 新增的特性， 通过使用 `=>` 定义函数的新语法

箭头函数表达式的语法比函数表达式更简洁，并且没有自己的`this`，`arguments`，`super` 或 `new.target`

箭头函数表达式更适用于那些本来需要匿名函数的地方，并且它不能用作构造函数

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



### 1-2 箭头函数参数

* 如果没有参数，括号将是空的，括号必须保留
* 只有一个参数，还可以省略掉参数外的圆括号，使代码更短
* 当有多个参数时，在圆括号内定义多个参数用逗号分隔
* 箭头函数不允许重复的参数名

```js
// 无参数时括号必须保留
let fn = () => 1;

// 只有一个参数可省略参数括号
let fn1 = a => a;

// 有多个参数用, 隔开
let fn2 = (a, b, c) => a + b + c;
console.log(fn3(5, 5)); // 25

// 不允许重复的参数名
let fn4 = (a, a) => a + a;
cosnole.log(fn4(1, 3)); // Uncaught SyntaxError: Duplicate parameter name not allowed in this context
```



### 1-3 箭头函数函数体

* 如果箭头函数的代码块部分有多条语句，就要使用大括号将它们括起来，并且使用 `return` 语句返回
* 花括号 `{}` 里面的代码会被解析为一系列语句，箭头函数返回一个对象，必须在对象外面加上括号 `()`，否则会报错
* 箭头函数在参数和箭头之间不能换行，可以通过`()`、`{ }`来实现换行

```js
// 箭头函数返回有多条语句时。用大括号抱起来，然后return
let fn3 = (a, b) => {
    console.log(a + b);
 	return a * b;
}

// 参数和箭头之间不能换行
let fn5 = (x, y) 
=> x + y;
console.log(fn5(7, 8)); //  Unexpected token '=>'
// 通过()、{}来实现换行
let fn5 = (
    x,
    y) => x + y;
console.log(fn5(7, 8));

// {} 大括号包裹为代码块
let fn6 = (a, b) => {a: b};
console.log(fn6(name, 'jsx')); // undefined

// 返回对象必须加上大括号
let fn7 = (a, b) => ({name: b});
console.log(fn7('name', 'jsx')); // {name: 'jsx'}
```



### 1-3 箭头函数特性

* 箭头函数没有 `prototype` 原型属性

```js
// 没有prototype原型
let arrow1 = (a) => a;
console.log(arrow1.prototype); // undefined
```



* 箭头函数不能用作构造器，和 `new`一起用会抛出错误

```js
// 不可当做构造函数
let arrow2 = (a) => a;
let arrow3 = new arrow2(2); // arrow2 is not a constructor
```



* 没有 `arguments` 对象，可以用 `rest` 参数代替

```js
// 箭头函数没有 arguments对象
// 可以通过rest语法获取参数
let arrow = (...all) => {
	// console.log(arguments); // arguments is not defined
	console.log(all); // [2, 8]
}
console.log(arrow(2, 8))
```



* 不可以使用 `yield` 关键字，因此箭头函数不能用作 `Generator` 函数生成器



* 箭头函数不支持`new.target`
  * `new.target`是 `ES6` 新引入的属性，普通函数如果通过`new`调用，`new.target`会返回该函数的引用
  * 箭头函数的 `this` 指向普通函数，它的 `new.target` 就是指向该普通函数的引用

```js
// 箭头函数不支持new.target
let a = () => {
	console.log(new.target); // 报错：new.target 不允许在这里使用
};
a(); // new.target expression is not allowed here

function func() {
	let a = () => {
		console.log(new.target);
	}
	a();
}
new func(); // f func(){}
```



* 箭头函数没有自己的 `this` 指针

```js
// 没有自己的this,会获取定义时候上下文的this
let obj = {
	fun: function() {
		console.log(this); // this 普通函数拥有自己的this
		var obj1 = {
			fun1: () => {
				console.log(this);
			}
		}
		obj1.fun1();
	}
}
obj.fun(); // this 指向fun 函数

function global() {
	console.log(this) // this 指向window
	setTimeout(() => {
		console.log(this); // this 指向window 上下文
	})
}
global()
```



### 1-4 箭头函数的 this

* 箭头函数的 `this` 指向在定义的时候继承自外层第一个普通函数的 `this`
  * 箭头函数的 `this` 指向定义时所在的外层第一个普通函数，跟使用位置没有关系
  * 被继承的普通函数的 `this` 指向改变，箭头函数的 `this` 指向会跟着改变

```js
let jsx = {
	name: 'jsx'
}
let ljj = {
	name: 'ljj'
}

function fn1() {
	console.log(this)
	let a = () => {
		console.log(this);
	}
	a();
}

function fn2() {
	console.log(this);
}
fn1(); // this 为windows
fn1.call(ljj); // this指向ljj 箭头函数跟随外层普通函数
fn2();
fn2.apply(jsx); // this 指向jsx
fn2.bind(jsx); // this 指向jsx
```



* 箭头函数外层没有普通函数，严格模式和非严格模式下 `this` 都会指向`window`全局对象

```js
'user strict'
let nofun = () => {
    console.log(this); // window
}
nofun()
```



* `call` 、 `apply` 、 `bind` 无法改变箭头函数中 `this` 的指向

```js
let html = {
	name: 'html'
}
let vue = {
	name: 'vue'
}

function method() {
	console.log(this);
	let a = () => {
		console.log(this);
	}
	a.call(html); // 不会改变，外层改变获取外层this跟着改变
	a.apply(html);
	a.bind(html);
}
method.call(vue)
```



* 箭头函数的 `this` 指向普通函数时,它的 `argumens` 继承于该普通函数

```js
function args(a, b, c) {
	console.log(arguments);
	let x = () => {
		console.log(arguments); // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
	}
	x();
}
args(1, 2, 3);
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220317210857.png)



## 2. this 指针

### 2-1 什么是 this 指针?

当前执行上下文（`global`、`function` 或 `eval`）的一个属性，在非严格模式下，总是指向一个对象，在严格模式下可以是任意值

`this` 就是一个指针，指向的当前执行环境，可以对当前执行环境进行一些操作

**this 永远指向最后调用它的那个对象**

```js
function thisArg() {
	this.name = 'jsx'; //
	console.log(this);
}
console.log(thisArg()); // undefined
console.log(window.name); // jsx

// 调用了thisArg()，发现this指向的是window，name属性添加到了window上
```



### 2-2 全局中的 this

* 无论是否在严格模式下，在全局执行环境中（在任何函数体外部）`this` 都指向全局对象，函数内部的 `this` 都指向 `undefined`

```js
function global() {
	console.log(this); // window
}
global();

// 严格模式下
"use strict"

function strictFn() {
	console.log(this);
}
strictFn(); // undefined
```



### 2-3 普通函数内的 this

* 在函数内部，`this` 的值取决于函数被调用的方式
* 没有明确调用者时也就是立即执行函数，`this` 指向 `window`
* 在严格模式下，函数体内部的 `this` 为 `undefined`

```js
// 在没有明确调用者时，this指向window
function func() {
	function func1() {
		function func2() {
			console.log(this)
		}
		func2()
	}
	func1();
}
func();

// 函数内部，严格模式下this为undefined
function strictFn() {
	'use strict';
	console.log(this);
}
strictFn(); // undefined
```



### 2-4 作为对象方法中的 this

* 当函数作为对象里的方法被调用时，`this` 被设置为调用该函数的对象

```js
let method = {
	age: 22,
	fn: function() {
		console.log(this)
	}
}
method.fn(); // {age: 22, fn: ƒ}
```



### 2-5 箭头函数里的 this

* 箭头函数本身并不具有 `this` 
* 箭头函数会将当前作用域中的 `this` 作为自己的 `this`
* `call`、`bind`、或者`apply` 无法改变箭头函数的 `this`

```js
let str = {
	age: 22
}

function arrowFn() {
	let fn = () => {
		console.log(this)
	}

	fn.call(str); // 不改变
	fn();
}
arrowFn(); // window

let obj = {
	name: 'jsx'
}

arrowFn.call(obj); // {name: 'jsx'}
```



### 2-6 计时器中的 this

立即执行函数的 `this` 指向 `window`

`setTimeout`、`setInterval` 定时器 `this `指向 `window`

```js
 var name = 'ljj';
 let obj = {
 	name: 'jsx',
 	fn: function() {
 		let time = setTimeout(function() {
 			console.log(this.name); // ljj 指向window
 		}, 0)

 	}
 }
 obj.fn()
```



### 2-6 构造函数里的 this

当一个函数用作构造函数时（使用 `new` 关键字），它的 `this` 指向构造函数创建的实例对象

```js
function fn() {
	this.name = 'jsx'
	console.log(this); // fn {name: 'jsx'}
}
let method = new fn();
console.log(method.name); // jsx
```



### 2-7 DOM事件中的 this

- 事件中 `this `指向事件源对象，`currentTarget`指的是绑定事件的 `DOM` 对象，`target`指的是触发事件的对象
- `DOM` 事件回调里面 `this` 总是指向`currentTarget`，如果触发事件的对象刚好是绑定事件的对象，即 `target === currentTarget`，`this` 也会顺便指向 `target`

```js
function fn(e) {
	console.log(this); //  指向事件源对象
	console.log(e.currentTarget); // 绑定dom对象
	console.log(e.target) // 触发事件的对象
	console.log(this === e.currentTarget); // 总是true
	console.log(this === e.target);
}
let ele = document.getElementsByClassName('box');
for (let i = 0; i < ele.length; i++) {
	ele[i].addEventListener('click', fn)
}
```

