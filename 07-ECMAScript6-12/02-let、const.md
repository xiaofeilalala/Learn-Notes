# let、const



## 1. let 关键字

* `let` 关键字拥有块级作用域
* `let` 关键字不会存在变量提升
* 暂时性死区：`let` 关键字声明时会存在暂时性死区
* `let` 关键字不允许重复声明

```js
// let 关键字拥有块级作用域
{
	let str = 'jsx';
	var num = 123;
	// 在同一作用域下可以访问
	console.log(str)
}
console.log(str); // 访问不到str
console.log(num);

// 不存在变量提升
console.log(str1); // undefined
var str1 = 'jsx';

console.log(num1); // 访问不到num1
let num1 = 123;

// 暂时性死区
if (true) {
	str2 = 'jsx'
	let str2;
	console.log(str2); // 暂时性死区 无法访问
}

typeof num2; // 暂时性死区 无法访问
let num2 = 123;

// 不允许重复声明
let str3 = 'jsx'; // 报错
let str3 = 'jsx'; // 报错
```



## 2. 暂时性死区 TDZ

如果区块中存在 `let` 和 `const` 命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域，凡是在声明之前就使用这些变量，就会报错

使用 `let` 和 `const` 关键字要先声明变量再使用

```js
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}

typeof x; // ReferenceError
let x;
```



## 3. 块级作用域

`ES5` 只有全局作用域和函数作用域，没有块级作用域

`let` 和 `const` 为 `JavaScript` 新增了块级作用域

* 允许块级作用域的任意嵌套
* 内层作用域可以定义外层作用域的同名变量
* 块级作用域替代了匿名立即执行函数表达式 `IIFE`
* 允许在块级作用域内声明函数，函数声明会提升到所在的块级作用域的头部
* `ES6` 的块级作用域必须有大括号，如果没有大括号，`JavaScript` 引擎就认为不存在块级作用域

```js
// 不同块级作用域可以声明相同变量
function f1() {
	let n = 5;
	if (true) {
		let n = 10;
	}
	console.log(n); // 5
}

// IIFE 写法
(function() {
	var tmp = 'jsx';
	console.log(tmp); // jsx
}());
console.log(tmp) // ReferenceError

// 块级作用域写法
{
	let tmp = 'jsx';
	console.log(tmp) // jsx
}
console.log(tmp) // ReferenceError

// 块级作用域声明函数
{
	let a = 'secret';
	let f = function() {
		return a;
	};
	console.log(f()); // secret
}

// 块级作用域必须有大括号
if (true) {
	let x = 1;
	console.log(x)
}
```



## 4. const 关键字

* `const` 关键字拥有块级作用域
* `const` 声明一个只读的常量，一旦声明，常量的值就不能改变
* `const` 命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用
* `const`声明的常量，也与`let`一样不可重复声明

```js
// const声明常量,常量变量名通常全部大写
const STR = 'jsx'

// 不能修改
STR = 'ljj';
console.log(STR); // Uncaught TypeError

// 拥有块级作用域
{
	const NUM = 123;
	console.log(NUM); // 123
}
console.log(NUM); // ReferenceError

// 暂时性死区
function sayHi() {
	STR1 = 'jsx';
	const SRT1; // SyntaxError
}

// 不会变量提升
console.log(NUM1); //  ReferenceError
const NUM1 = 123;

// 不可重复声明
const STR2 = 'jsx'; //  SyntaxError
const STR2 = 'jsx'; //  SyntaxError
```



## 5. const 值改变

`const` 并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动

* 对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量
* 对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，`const`只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了

```js
// const 声明变量值修改问题
// 简单类型无法修改
const STRING = 'string'
STRING = 'str'; // Uncaught TypeError

// 引用类型可以修改,保证变量指向的内存地址
const OBJECT = {
	name: 'jsx',
	age: 22
}
console.log(OBJECT); // {name: 'jsx', age: 22}
OBJECT.name = 'ljj'
console.log(OBJECT); // {name: 'ljj', age: 22}
```



## 6. 声明方式与全局

`ES5` 只有两种声明变量的方法：`var`命令和`function`命令。ES6 除了添加`let`和`const`命令，后面章节还会提到，另外两种声明变量的方法：`import`命令和`class`命令。所以，ES6 一共有 6 种声明变量的方法

```js
// 6中声明方式
var str = 'jsx'
let str1 = 'jsx'
const STR2 = 'jsx'

function str3() {
	console.log('hello')
}
class str4 {

};
import {
	sayHi
} from './index.js';
sayHi(); // sayHi
```

```js
// index.js
export function sayHi() {
    console.log('sayHi')
}
```



`ES2020` 标准引入`globalThis`作为顶层对象任何环境下，`globalThis`都是存在的，都可以从它拿到顶层对象，指向全局环境下的`this`

```js
// 全局对象 globalThis
console.log(globalThis)
```

