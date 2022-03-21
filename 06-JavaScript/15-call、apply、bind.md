# call、apply、bind

`call`、`apply` 和 `bind` 是挂在 `Function` 对象上的三个方法,只有函数才有这些方法

作用：改变函数执行时的 `this` 指向



## 1. call

`fun.call(thisArg, arg1, arg2, ...)`

* `thisArg`：在 *`function`* 函数运行时使用的 `this` 值
  * 非严格模式下：`thisArg` 指定为 `null`，`undefined`，`fun` 中的`this` 指向 `window` 对象
  * 严格模式下：`fun` 的 `this` 为 `undefined`
  * 值为原始值（数字，字符串，布尔值）的 `this` 会指向该原始值的自动包装对象，如 `String`、`Number`、`Boolean`
* `arg1, arg2, ...`：指定的参数列表
  * 如果 `arg` 不传或为 `null`/`undefined`，则表示不需要传入任何参数

`call()` 方法使用一个指定的 `this` 值和单独给出的一个或多个参数来调用一个函数

使用调用者提供的 `this` 值和参数调用该函数的返回值，若该方法没有返回值，则返回 `undefined`

```js
function getInfo(arg1, arg2) {
	console.log(arg1, arg2)
	console.log(this.name);
}
let jsx = {
	name: 'jsx'
}

let ljj = {
	name: 'ljj'
}
getInfo.call(jsx, '蒋壮壮', '刘草草'); // 蒋壮壮 刘草草
```



## 2. apply

`fun.apply(thisArg, argsArray)`

* `thisArg`：在 *`function`* 函数运行时使用的 `this` 值

  * 非严格模式下：`thisArg` 指定为 `null`，`undefined`，`fun` 中的`this` 指向 `window` 对象
  * 严格模式下：`fun` 的 `this` 为 `undefined`
  * 值为原始值（数字，字符串，布尔值）的 `this` 会指向该原始值的自动包装对象，如 `String`、`Number`、`Boolean`

* `argsArray`：可选的。一个数组或者类数组对象

  * 数组元素将作为单独的参数传给 `func` 函数

  * 如果 `argsArray` 不传或为 `null`/`undefined`，则表示不需要传入任何参数

`apply()` 方法调用一个具有给定`this`值的函数，以及以一个数组或类数组对象的形式提供的参数

调用有指定 `this` 值和参数的函数的结果

```js
function getInfo(...arg) {
	console.log(arg)
	console.log(this.name);
}
let jsx = {
	name: 'jsx'
}

let ljj = {
	name: 'ljj'
}
let arr = ['html', 'css', 'js']
getInfo.apply(ljj, arr); //ljj 'html', 'css', 'js'
```



## 3. bind

`fun.bind(thisArg, arg1, arg2, ...)`

`thisArg`：调用绑定函数时作为 `this` 参数传递给目标函数的值

`arg1, arg2, ...`：当目标函数被调用时，被预置入绑定函数的参数列表中的参数

`bind()` 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用

```js
function global(arg) {
	console.log(arg);
	console.log(this.name)
}
let html = {
	name: 'html'
}
let vue = {
	name: 'vue'
}
let newFn = global.bind(html, 'js'); // 不会立即执行
newFn(); // html js
```



### 4. call、apply、bind区别

`call`、`apply`、`bind` 区别：

- `call` / `apply` 改变了函数的 `this` 上下文后马上执行该函数
- `bind` 则是返回改变了上下文后的函数不执行该函数
- `call` / `apply` 返回`fun`的执行结果
- `bind` 返回 `fun` 的拷贝，并指定了 `fun` 的 `this` 指向，保存了 `fun` 的参数



`call`、`apply` 使用选择：

- 参数数量/顺序确定就用 `call`，参数数量/顺序不确定的话就用 `apply`
- 考虑可读性：参数数量不多就用 `call`，参数数量比较多的话，把参数整合成数组，使用 `apply`
- 参数集合已经是一个数组的情况，用 `apply`，比如上文的获取数组最大值/最小值

```js
const obj = {
	age: 24,
	name: 'OBKoro1',
}
const obj2 = {
	age: 777
}
callObj(obj, handle)
callObj(obj2, handle)
// 根据某些条件来决定要传递参数的数量、以及顺序
function callObj(thisAge, fn) {
	let params = [];
	if (thisAge.name) {
		params.push(thisAge.name)
	}
	if (thisAge.age) {
		params.push(thisAge.age)
	}
	fn.apply(thisAge, params) // 数量和顺序不确定 不能使用call
}

function handle(...params) {
	console.log('params', params) // do some thing
}
```



## 4. 改变 this 指向

### 4-1 类型判断

`Object.prototype.toString.call()`

对于对象型的数据类型，我们可以借助 `call` 来得知他的具体类型

```js
function isType(data, type) {
	const typeObj = {
		'[object String]': 'string',
		'[object Number]': 'number',
		'[object Boolean]': 'boolean',
		'[object Null]': 'null',
		'[object Undefined]': 'undefined',
		'[object Object]': 'object',
		'[object Array]': 'array',
		'[object Function]': 'function',
		'[object Date]': 'date', // Object.prototype.toString.call(new Date())
		'[object RegExp]': 'regExp',
		'[object Map]': 'map',
		'[object Set]': 'set',
		'[object HTMLDivElement]': 'dom', // document.querySelector('#app')
		'[object WeakMap]': 'weakMap',
		'[object Window]': 'window', // Object.prototype.toString.call(window)
		'[object Error]': 'error', // new Error('1')
		'[object Arguments]': 'arguments',
	}
	let name = Object.prototype.toString.call(data) // 借用Object.prototype.toString()获取数据类型
	let typeName = typeObj[name] || '未知类型' // 匹配数据类型
	return typeName === type // 判断该数据类型是否为传入的类型
}
console.log(
	isType({}, 'object'), // true
	isType([], 'array'), // true
	isType(new Date(), 'object'), // false
	isType(new Date(), 'date'), // true
)
```



### 4-2 数组获取最大值和最小值

`apply()` 直接传递数组调用方法的参数，使用`Math.max`、`Math.min`来获取数组的最大值/最小值

```js
let arr = [2, 29, 0, 38, 91];
console.log(Math.max.apply(Math, arr)) // 91
console.log(Math.min.apply(Math, arr)) // 0
```



### 4-3 伪数组转换

` Array.prototype.slice.call()`

类数组不是真正的数组所有没有数组内置对象方法，可以利用`call`、`apply` 来将其转化为真正的数组

```js
let likeArr = {
	0: 'html',
	1: 'css',
	2: 'js',
	length: 3
}
let arr = Array.prototype.slice.call(likeArr);
console.log(arr); //  ['html', 'css', 'js']
arr.push('vue');
console.log(arr); // ['html', 'css', 'js', 'vue']
```



### 4-4 数组元素添加

在 `js` 中要往数组中添加元素，可以直接用 `push` 方法

```js
let arr1 = ['jsx', 'ljj'];
let arr2 = ['ddc', 'zdj'];
[].push.apply(arr1, arr2);
console.log(arr1); // ['jsx', 'ljj', 'ddc', 'zdj']
```



### 4-5 构造函数继承

通过借用父类的构造方法来实现父类方法/属性的继承

```js
let global = function(name, age) {
	this.name = name;
	this.age = age;
}
let fn1 = function(name) {
	global.call(this, name);
}
let fn2 = function(name, age) {
	global.apply(this, arguments);
}
let method1 = new fn1('jsx', 22)
console.log(method1) // fn1 {name: 'jsx', age: undefined}
let method2 = new fn2('ljj', 23)
console.log(method2) // fn2 {name: 'jsx', age: 23}
```



### 4-6  this赋值给闭包

当使用匿名函数或者立即执行函数时，`this` 指向的`window` 对象，可以通过在函数内部创建私有变量，形成闭包，使得 `this` 指向函数调用本身

```js
var name = 'ljj'; 
let obj = {
	name: 'jsx',
	fn: function() {
		let _this = this
		return function() {
			return _this.name
		}
	}

}
console.log(obj.fn()()) // jsx
```

