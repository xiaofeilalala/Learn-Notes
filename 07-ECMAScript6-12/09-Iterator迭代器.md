# Iterator 迭代器

## 1. 迭代器

### 1-1 迭代器概念

`Javascript` 四种集合数据结构：`Array`、`Object`、`Map`、`Set`

迭代器它是一种接口，为各种不同的数据结构提供统一的访问机制，任何数据结构只要部署 `Iterator` 接口，就可以完成遍历操作

* 为各种数据结构，提供一个统一的、简便的访问接口
* 使数据结构的成员能够按某种次序排列
* `Iterator` 接口主要供 `for...of` 消费



### 1-2 迭代器原理

* 创建一个指针对象，指向当前数据结构的起始位置，也就是说，遍历器对象本质上，就是一个指针对象

* 第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员

* 第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员

* 不断调用指针对象的`next`方法，直到它指向数据结构的结束位置

每一次调用`next`方法，都会返回数据结构的当前成员的信息，返回一个包含`value`和`done`两个属性的对象

* `value` —— 属性是当前成员的值
* `done` —— 属性是一个布尔值，表示遍历是否结束

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220425095952.png)

```js
// 迭代器接口 [Symbol.iterator] 会返回 next() 方法
// next() 方法返回一个对象，包含done 和 value
// done 为false 则继续执行直到 true, value 则是元素的值
let arr = [1, 2, 3];

function makeIterator(arr) {
	let index = 0;
	return {
		next() {
			return index < arr.length ? {
				value: arr[index++],
				done: false
			} : {
				value: arr[index],
				done: true
			}
		}
	}
}
let arrIterator = makeIterator(arr);
console.log(arrIterator.next()); // {value: 1, done: false}
console.log(arrIterator.next()); // {value: 2, done: false}
console.log(arrIterator.next()); // {value: 3, done: false}
console.log(arrIterator.next()); // {value: undefined, done: true}
```



## 2. 迭代器接口

### 2-1 Symbol.iterator

`ES6` 规定，默认的 `Iterator` 接口部署在数据结构的 `Symbol.iterator` 属性

一个数据结构只要具有 `Symbol.iterator` 属性，就可以认为是可遍历的

`Symbol.iterator` 属性本身是一个函数，就是当前数据结构默认的遍历器生成函数，执行这个属性，会返回一个遍历器对象。该对象的根本特征就是具有`next`方法，`next` 方法则会返回一个包含 `value`和`done`两个属性的对象

```js
// 迭代器接口 Symbol.iterator
let arr = ['html', 'css', 'js'];
// 通过Symbol.iterator属性返回next()函数
let arrNext = arr[Symbol.iterator]()
console.log(arrNext.next()); // {value: 'html', done: false}
console.log(arrNext.next()); // {value: 'css', done: false}
console.log(arrNext.next()); // {value: 'js', done: false}
console.log(arrNext.next()); // {value: undefined, done: true}
```



### 2-2 return 和 throw

遍历器对象除了具有 `next()` 方法，还可以具有 `return()`、`throw()` 方法

`return` 方法在循环提前退出，循环有错误，`break` 语句时就会调用 `return` 方法

`throw()`方法主要是配合 `Generator` 函数使用，一般的遍历器对象用不到这个方法

```js
// return方法会在提前退出迭代，迭代中出错，berak语句时触发
let arr = [1, 2, 3];
for (let i of arr) {
	console.log(i); // 1
	break
}

// throw方法搭配Generator函数使用
for (let i of arr) {
	console.log(i); // 1
	throw new Error();
}
```



## 3. 可迭代对象

原生具备 `Iterator` 接口的数据结构：

* `Array`
* `Map`
* `Set`
* `String`
* `TypedArray`
* 函数的 `arguments` 对象
* `NodeList` 对象



### 3-1 迭代对象

对象是没有默认部署 `Iterator` 接口的，因为对象的哪个属性先遍历，哪个属性后遍历是不确定的

如果一个对象需要被 `for...of` 迭代，首先的在对象上部署 `Symbol.iterator` 属性，然后在 `Symbol.iterator` 的属性上部署遍历器生成方法（原型链上的对象具有该方法也可以）

```js
// 由于对象没有默认迭代接口，需要自己去添加
let obj = {
	name: 'jsx',
	age: 22
}
let arr = [];
// 遍历对象的键值对
for (let item in obj) {
	let newobj = {};
	newobj[item] = obj[item];
	arr.push(newobj)
}
obj.arr = arr;
console.log(obj)
// 添加迭代器
obj[Symbol.iterator] = function() {
	let index = 0;
	let that = this;
	return {
		next() {
			if (index < that.arr.length) {
				return {
					value: that.arr[index++],
					done: false
				}
			}
			return {
				value: undefined,
				done: true
			}
		}
	}
}
for (let list of obj) {
	console.log(list)
};
// {name: 'jsx'}
// {age: 22}
```



### 3-2 添加 Symbol.iterator 接口

对于类似数组的对象（存在数值键名和 `length` 属性），部署 `Iterator` 接口，可以将`Symbol.iterator` 方法直接引用数组的 `Iterator` 接口

* 普通对象部署数组的`Symbol.iterator`方法，并无效果
* 如果 `Symbol.iterator` 方法对应的不是遍历器生成函数 `next`，解释引擎将会报错

```js
NodeList.prototype[Symbol.iterator] = Array.prototype[Symbol.iterator];
// 或者
NodeList.prototype[Symbol.iterator] = [][Symbol.iterator];
```

```js
// 类数组添加迭代接口
let likeArr = {
	0: 'html',
	1: 'css',
	2: 'js',
	length: 3
}
// likeArr不是可迭代对象
// for(let item of likeArr) {
//   console.log(item)
// }
// 将类数组对象转换为可迭代对象
likeArr[Symbol.iterator] = Array.prototype[Symbol.iterator];
for (let item of likeArr) {
	console.log(item); // html css js
}
```

```js
// 普通对象使用数组的迭代器接口并无效果
let obj = {
	name: 'jsx'
};
obj[Symbol.iterator] = [][Symbol.iterator]
for (let item of obj) {
	console.log(item)
}

// 如果迭代接口不是返回指定函数，会报错
let arr1 = [1, 2, 3];
arr1[Symbol.iterator] = function() {
	let that = this
	let index = 0;
	return {
		// 必须迭代器生成函数next
		go() {
			if (index < that.length) {
				return {
					value: that[index++],
					done: false
				}
			}
			return {
				value: undefined,
				done: true
			}
		}
	}
}
console.log([...arr1]); //  [1, 2, 3]
```



### 3-3 原生可迭代对象

* `String` 字符串 —— 字符串是一个类似数组的对象，也原生具有 `Iterator` 接口

* 数组原生具备 `iterator` 接口
* `Set` 和 `Map` 结构也原生具有 `Iterator` 接口，可以直接使用 `for...of` 循环
* 类似数组的对象，`arguments`、文档节点`NodeList`

```js
// 字符串类型 字符串是一个类数组的对象具有迭代接口
let str = 'hello';
console.log([...str])

// 数组也具备迭代接口
let arr = [1, 2, 3];
console.log(...arr); //  1 2 3

// Set 和 Map 类型都具有迭代接口
let set = new Set();
set.add(true)
	.add('1')
console.log(...set) // true '1'

let map = new Map();
map.set('user', 'jsx');
map.set('grilfriend', 'ljj');
for (let [key, value] of map) {
	console.log(key + ' ' + value);
}
// user jsx
// grilfriend ljj

// 类数组对象 函数参数
function getName(...arg) {
	console.log(...arg); // html css js
}
getName('html', 'css', 'js')

// 文档节点
window.onload = function() {
	let box = document.querySelectorAll('.box');
	console.log(...box);
	// <div class="box">1</div>
	// <div class="box">2</div>
	// <div class="box">3</div>
}
```



### 3-4 调用迭代接口

* 数组与 `Set` 解构进行解构赋值时，会默认调用`Symbol.iterator`方法
* 扩展运算符 `...` 也会调用默认的 `Iterator` 接口
* `yield*`后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口
* `for...of`、`Array.form()`、`Map()`、`Set()`、`WeakSet()`、`WeakMap()`、`Promise.all()`、`Promise.race`

```js
// 解构赋值
let [a, b] = [1, 2];
console.log(a, b); // 1 2

// 扩展运算符
let arr = ['html', 'css', 'js']
console.log(...arr); // html css js

// for of
function getNames(...arg) {
	for (let item of arg) {
		console.log(item) // html css js
	}
}
getNames('html', 'css', 'js')
```



### 3-4 for...in 和 for...of 区别

`forEach` 循环

* 使用 `forEach` 遍历数组的话，使用 `break` 不能中断循环，使用 `return` 也不能返回到外层函数

```js
// forEach break和return 不能停止迭代
let arr = [1, 2, 3];
let newarr = arr.forEach(item => {
	console.log(item)
})
```



`for...in` 循环

遍历对象通常用 `for...in` 来遍历对象的键名

- 数组的键名是数字，但是 `for...in` 循环是以字符串作为键名
- 遍历顺序有可能不是按照实际数组的内部顺序
- 使用 `for...in`会遍历数组所有的可枚举属性，包括原型

```js
// for in 一般用来遍历对象
let obj = {
	name: 'jsx',
	age: 22
};
for (let item in obj) {
	console.log(item); // name age
}

// 遍历数组，字符串作为键名
let arr1 = [1, 2, 3];
for (let list in arr1) {
	console.log(list); // '0', '1', '2'
}
```



`ES6` 新增的 `for...of` 循环

`for..of` 适用遍历数/数组对象/字符串/`Map`/`set`等拥有迭代器对象的集合，但是不能遍历对象，因为没有迭代器对象

* `for...of`遍历的只是数组内的元素，而不包括数组的原型属性 `method` 和索引 `name`

- 不同于`forEach`方法，它可以与`break`、`continue` 和 `return` 配合使用
- 提供了遍历所有数据结构的统一操作接口

```js
let arr2 = [1, 2, 3];
for (let list of arr2) {
	console.log(list); // 1 2 3
}
for (let list of 'hello') {
	console.log(list); // h e l l o
}
let set = new Set(['1', true, {
	name: 'jsx'
}]);
for (let list of set) {
	console.log(list); // 1 true {name: 'jsx'}
}
```
