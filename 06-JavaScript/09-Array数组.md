# Array 数组



## 1. 数组简介

`JavaScript` 的 `Array` 对象是用于构造数组的全局对象，数组是类似于列表的高阶对象

`JavaScript` 数组是一种类列表对象，它的原型中提供了遍历和修改元素的相关操作，用于在单一变量中存储多个值

```js
let arr = [item1, item2, ...];
```



## 2. 创建

创建一个空数组有两种语法：

绝大多数情况下使用的都是第二种语法

```js
let arr = new Array();
let arr = [];
```



### 2-1 字面量创建

* 我们可以在方括号中添加初始元素
* 空格和折行并不重要，声明可横跨多行
* 不要在数组最后一个元素后写逗号

```js
let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
let arr = ['jsx',
          'ljj',
          'zdj',
          'ddc'
];
```



### 2-2 构造函数创建

* 如果参数为空，则表示创建一个空数组

* 使用单个参数（即数字）调用 `new Array`，那么它会创建一个指定了长度，却没有任何项的数组

* 如果有多个参数时，表示数组中的元素

```js
let arr = new Array();
console.log(arr); // []

let arr1 = new Array(4);
console.log(arr1); // (4) [空属性 × 4]

let arr2 = new Array('jsx', 'ljj', 'zdj', 'ddc');
console.log(arr2); // ['jsx', 'ljj', 'zdj', 'ddc']
```



## 3. 数组基础

### 3-1 数组访问

通过使用索引来引用数组中的元素

通过方括号中的索引获取元素，0 是数组中的第一个元素，1 是第二个，数组索引从 0 开始

```js
let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
console.log(arr[0]); // jsx
console.log(arr[1]); // ljj
console.log(arr[2]); // zdj
```



通过使用索引来替换数组中的元素

```js
let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
arr[1] = '520ljj';
console.log(arr); // ['jsx', '520ljj', 'zdj', 'ddc']
```



通过使用索引来添加数组中的元素

```js
let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
arr[4] = '520ljj';
console.log(arr); // ['jsx', 'ljj' , 'zdj', 'ddc', '520ljj']
```



### 3-2 数组特性

`length` 属性的值是数组中元素的总个数

通过length属性，可以在数组末尾增加删除元素

* 如果指定的索引值小于数组的元素数，就会返回存储在相应位置的元素，那么数组就会被截断，多出的元素会被删除
* 如果设置的索引值大于数组的长度，那么就会将数组长度扩充至该索引值加一

```js
let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
console.log(arr.length); // 4

arr.length = 2;
console.log(arr); // ['jsx', 'ljj']

arr.length = 10;
console.log(arr); // (10) ['jsx', 'ljj', 空属性 × 8]
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220307172924.png)



数组可以储存任何类型的元素

```js
let arr = ['jsx', 22, true, {name: 'ljj'}, 123n, [1, 2, 3], function (){console.log('hello function')}];
console.log(arr);
console.log(arr[3].name); // ljj
```



多维数组，数组里的元素也可以是数组

```js
let arr = ['jsx', ['ljj', ['ddc']]]
console.log(arr);
```



## 4. 数组操作

`push/pop` 方法运行的比较快，而 `shift/unshift` 比较慢

`push` 和 `unshift` 方法都可以一次添加多个元素

```js
// push 和 unshift 方法都可以一次添加多个元素
let arr = ['jsx', 'ljj'];
arr.push('a', 'b', 'c');
arr.unshift(1, 2, 3);
console.log(arr); // (8) [1, 2, 3, 'jsx', 'ljj', 'a', 'b', 'c']
```



### 4-1 `push()`

在数组末端添加元素，**该方法会改变原数组**

可以接收任意数量的参数，并将它们添加了数组末尾，并返回数组新的长度

```js
let arr = ['jsx', 'ljj'];
console.log(arr.push('ddc')); // ['jsx', 'ljj', 'ddc']
```



### 4-2 `pop()`

取出并返回数组的最后一个元素，**该方法会改变原数组**

用于删除并返回数组的最后一个元素，它没有参数

```js
let arr = ['jsx', 'ljj'];
console.log(arr.pop()); // ['ljj']
```



### 4-3 `shift()`

取出数组的第一个元素并返回该元素，**该方法会改变原数组**

`shift` 方法需要先删除索引为 0 的元素，然后所有元素向左移动重新定义索引，最后更新 `length` 属性

```js
let arr = ['jsx', 'ljj'];
console.log(arr.shift()); // ['jsx']
```



### 4-4 `unshift()`

向数组的开头添加一个或更多元素，并返回新的长度，**该方法会改变原数组**

`unshift` 方法在数组的首端添加元素，首先需要将现有的元素向右移动，增加它们的索引值

```js
let arr = ['jsx', 'ljj'];
console.log(arr.unshift('ddc')); // ['ddc', 'jsx', 'ljj']
```



## 5. 数组遍历

### 5-1 for 循环

遍历数组最古老的方式就是 `for` 循环，由三个表达式组成，分别是声明循环变量、判断循环条件、更新循环变量

`for` 循环可以用来遍历数组，字符串，类数组，DOM节点等，可以改变原数组

运行得最快，可兼容旧版本浏览器

```js
let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i])
}
```



### 5-2 for..of

`for...of` 语句创建一个循环来迭代可迭代的对象

在 ES6 中引入的 `for...of` 循环，以替代 `for..in` 和 `forEach()` ，并支持新的迭代协议

* `for..of` 方法只会遍历当前对象的属性，不会遍历其原型链上的属性
* `for..of` 方法适用遍历 **数组/ 类数组/字符串/`map`/`set`** 等拥有迭代器对象的集合
* `for..of` 方法不支持遍历普通对象，因为其没有迭代器对象，如果想要遍历一个对象的属性，可以用 `for..in` 方法
* 可以使用`break`、`continue`、`return` 来中断循环遍历

```js
// variable：每个迭代的属性值被分配给该变量
// iterable：一个具有可枚举属性并且可以迭代的对象
for (variable of iterable) {
    执行的代码块
}

let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
for (let arr of arr) {
    console.log(arr[arr])
}

let obj = {
	name: 'jsx',
	age: 22,
	grilfriend: {
		name: 'ljj',
		age: 23
	}
}

for (let objs of obj) {
	console.log(objs);
}
// 普通对象没有迭代器对象无法使用for..of
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220307200807.png)



### 5-3 for..in

数组也是对象，可以使用 `for..in` 方法

遍历数组时为数组的下标，遍历对象时为对象的 `key` 值

`for…in` 主要用于循环对象属性，循环中的代码每执行一次，就会对对象的属性进行一次操作

`for in` 方法不仅会遍历当前的对象所有的可枚举属性，还会遍历其原型链上的属性

```js
// key 必须,指定的变量可以是数组元素，也可以是对象的属性
// obj 必须,指定迭代的的对象
for (let key in object) {
 执行的代码块
}

let arr = ['jsx', 'ljj', 'zdj', 'ddc'];
for (let key in arr) {
    console.log(arr[key]);
}
```



## 6. 迭代器和可迭代对象

### 6-1 迭代器

迭代器 `iteration` 它是一种接口，为各种不同的数据结构提供统一的访问机制，任何数据结构只要部署 `Iterator` 接口，就可以完成遍历操作

`Iterator` 的作用有三个：

* 为各种数据结构，提供一个统一的、简便的访问接口
* 使得数据结构的成员能够按某种次序排列
*  `Iterator` 接口主要供 `for...of` 消费

`Iterator` 迭代过程：

* 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象
* 这个对象里面有一个`next`方法，然后调用该 `next` 方法，移动指针使得指针指向数据结构中的第一个元素

* 每调用一次 `next` 方法，指针就指向数据结构里的下一个元素，这样不断的调用`next` 方法就可以实现遍历元素的效果了
* 每一次调用 `next` 方法，都会返回数据结构的当前成员的信息，返回一个包含`value`和`done`两个属性的对象
  * `value`属性是当前成员的值
  * `done`属性是一个布尔值，表示遍历是否结束，没有结束返回 `false`，结束为 `true`

```js
function myiteration(arr) {
	let index = 0;
	return {
		next: function() {
			return index < arr.length ? 
                {value: arr[index++], done: false} : 
                {value: undefined, done: true}
		}
	}
}
let result = myiteration([3, 4, 5]);
console.log(result.next()); // {value: 3, done: false}
console.log(result.next()); // {value: 4, done: false}
console.log(result.next()); // {value: 5, done: false}
console.log(result.next()); // {value: undefined, done: true}
```



### 6-2 可迭代对象

ES6 规定，默认的 `Iterator` 接口部署在数据结构的 `Symbol.iterator` 属性

一个数据结构只要具有 `Symbol.iterator` 属性，就可以认为是可迭代的（iterable）

`Symbol.iterator` 属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器

```js
// 普通对象转换为可迭代对象
let obj = {
	name: 'jsx',
	age: 22
}
let arr1 = [];
for (let i in obj) {
	let obj1 = {};
	obj1[i] = obj[i]
	arr1.push(obj1);
}
obj.date = arr1;

obj[Symbol.iterator] = function() {
	let self = this;
	let index = 0;
	return {
		next: function() {
			if (index < self.date.length) {
				return {
					value: self.date[index++],
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

for (let i of obj) {
	console.log(i);
}
// {name: 'jsx'}
// {age: 22}
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220310174756.png)



`String`、`Array`、`TypedArray`、函数的 `arguments` 对象、`NodeList` 对象、`Map` 和 `Set` 都是内置可迭代对象，因为它们的原型对象都拥有一个 `Symbol.iterator `方法

```js
let str = 'jsx';
let result = str[Symbol.iterator]();
console.log(result); // StringIterator {}
console.log(result.next());
```



### 6-3 数组迭代方法

在ES6中，Array的原型上暴露了3个用于检索数组内容的方法：`keys()`、`values()`、`entries()`

* `arr.keys()` 返回数组索引的迭代器
* `arr.values()` 方法返回数组元素的迭代器
* `arr.entries()` 方法返回索引值对的迭代器

这些方法返回的都是迭代器，所以可以将他们的内容通过 `Array.from` 直接转化为数组实例

```js
let arr = ['html', 'css', 'js', 'vue']
console.log(arr);
console.log(Array.from(arr.keys())); // [0, 1, 2, 3]
console.log(Array.from(arr.values())); // ['html', 'css', 'js', 'vue']
console.log(Array.from(arr.entries())); // [[0, 'html'], [1, 'css'], [2, 'js'], [3, 'vue']]
```



## 7. 类数组

### 7-1 什么是类数组?

类数组对象，就是指可以**通过索引属性访问元素**并且**拥有 length** 属性的对象

```js
var arrLike = {
  0: 'name',
  1: 'age',
  2: 'job',
  length: 3
}
```



在访问、赋值、获取长度时，对类数组对象的操作与对数组是一致的

```js
let obj = {
	0: 'jsx',
	1: 22,
	length: 2
}
console.log(obj) // {0: 'jsx', 1: 22, length: 2}
// 可以访问元素
console.log(obj[1]); // 22

// 可以给元素赋值
obj[0] = 'jsx-ljj';
console.log(obj);

// 可以获取长度
console.log(obj.length); // 2
```



类数组对象与数组的区别是**类数组对象不能直接使用数组的方法**

类数组对象是对象不是 `Array`,用 `isArray` 判断会返回 `false`

```js
// 类数组不能用数组的方法
let obj = {
	0: 'jsx',
	1: 22,
	length: 2
}

let arr = ['jsx', 'ljj'];
// 用isArray判断会返回 false
console.log(Array.isArray(arr)); // true
console.log(Array.isArray(obj)); // false

obj.push('ljj'); // obj.push is not a function
```



### 7-2 类数组场景

- 函数里面的参数对象 `arguments`
- 用 `getElementsByTagName`/`ClassName/Name` 获得的 `HTMLCollection` `DOM` 元素集合
- 用 `querySelector`、`querySelectorAll` 获得的 `NodeList` 节点的集合

```html
<ul>
    <li class="list">类数组场景</li>
    <li class="list">类数组场景</li>
    <li class="list">类数组场景</li>
    <li class="list">类数组场景</li>
    <li class="list">类数组场景</li>
</ul>
```

```js
// arguments 函数对象 类数组
function foo(name, age) {
	console.log(arguments);
	console.log(typeof arguments); // object
	console.log(arguments.length); // 2
}
foo('jsx', 22); // ['jsx', 22, callee: ƒ, Symbol(Symbol.iterator): ƒ]

// dom 元素集合 类数组
let list = document.getElementsByClassName("list");
console.log(list);
console.log(typeof list); // object

// nodeList 节点的集合 类数组
let list1 = document.querySelectorAll("ul > li");
console.log(list1)
for (let item of list1) {
	console.log(item)
	item.style.color = 'red';
}
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220310202430.png)



### 7-3 `Array.from()`

`Array.from(arrayLike, mapFn, thisArg)`

全局方法 `Array.from` 可以接受一个可迭代或类数组的值，并从中获取一个真正的数组，可以对其调用数组方法（该方法会返回一个的数组，不会改变原对象）

伪数组对象（拥有一个 `length` 属性和若干索引属性的任意对象）

可迭代对象（可以获取对象中的元素,如 `Map`和 `Set` 等）

如果不确定返回值，则会返回 `undefined`，最终生成的是一个包含若干个 `undefined` 元素的空数组

* `arrayLike`：想要转换成数组的类数组对象或可迭代对象
* `mapFn`：如果指定了该参数，新数组中的每个元素会执行该回调函数
* `thisArg`：可选参数，执行回调函数 `mapFn` 时 `this` 对象

```js
let obj = {
	0: 'jsx',
	1: 22,
	length: 2
}
let newArr = Array.from(obj, function(value, index) {
	console.table(value, index)
	return value;
})
console.log(newArr); // ['jsx', 22]
console.log(Array.isArray(newArr)); // true
```

拥有迭代器的对象还包括 `String`、`Set`、`Map` 等，`Array.from` 都可以进行处理

```javascript
// String
Array.from('abc');                             // ["a", "b", "c"]
// Set
Array.from(new Set(['abc', 'def']));           // ["abc", "def"]
// Map
Array.from(new Map([[1, 'ab'], [2, 'de']]));   // [[1, 'ab'], [2, 'de']]
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220310192321.png)



### 7-4 其它转换方法

* `Array.prototype.slice.call()` 能将具有`length`属性的对象转成数组
*  `Array.from(arguments)` 类数组对象和可遍历（`iterable`）对象转成数组
* ES6 中的扩展运算符`...`
