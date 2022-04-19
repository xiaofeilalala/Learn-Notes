# Object 对象



## 1. 对象基础使用

对象是一个包含相关数据和方法的集合

通常由一些变量和函数组成，我们称之为对象里面的属性和方法

- 对象是属性和方法的集合即封装
- 将复杂功能隐藏在内部，只开放给外部少量方法，更改对象内部的复杂逻辑不会对外部调用造成影响即抽象
- 继承是通过代码复用减少冗余代码
- 根据不同形态的对象产生不同结果即多态



### 1-1 对象的创建

* 字面量创建
* 构造函数创建

```js
let obj = {};
let obj = new object();
```



### 1-2 文本和属性

通过字面量创建对象时，可以直接将属性以键值对的形式放入 `{}` 中

属性名和属性值是一组一组的键值对结构，键和值之间使用 `:` 连接，多个值对之间使用 `,` 隔开，最后一个属性值后可以不加逗号 `,`

-   键：相当于属性名
    * 对象的所有键名都是字符串
    * ES6 又引入了 `Symbol` 值也可以作为键名
    * 如果键名是数值，会被自动转为字符串
-   值：相当于属性值，可以是任意类型的值（数字类型、字符串类型、布尔类型，函数类型等）

```js
let obj = {
	name: 'jsx',
	age: 22,
	study: true,
	fn: function() {
		console.log(this);
	}
}
obj.fn();
```



### 1-3 添加、删除、访问属性

* 使用点符号 `.` 访问属性值
  * 点符号要求 `key` 是有效的变量标识符
  * 不包含空格，不以数字开头，也不包含特殊字符（允许使用 `$` 和 `_`）
* 可以用 `delete` 操作符移除属性
* 对于多词属性，可以使用方括号 `[]`  访问属性值
  * 可用于任何字符串
  * 方括号中的字符串要放在引号中，单引号或双引号都可以

```js
// 通过.访问或者添加属性
// .点操作符访问不能有空格，数字开头，特殊字符
let obj = {
	message: 'hello'
}
console.log(obj.message);
obj.name = 'jsx';
console.log(obj.name)

// 多词属性设置通过[]方括号
let methods = {
	lesson: 'vue'
}
methods['hello world'] = 'javascript';
console.log(methods); // {lesson: 'vue', hello world: 'javascript'} 

// delete删除属性
let all = {
	name: 'ljj',
	age: 22
}
delete all.name
console.log(all); // {age: 22}
```



### 1-4 属性简写与限制

* 属性名跟变量名一样时，可以将对象属性简写
* 对象中的属性命名没有任何限制，属性名可以是任何字符串或者 `symbol`

```js
function makeUser(name, age) {
	return {
		name, // name: name
		age // age = age
	}
}
console.log(makeUser('jsx', 22)); // {name: 'jsx', age: 22}

var lesson = 'vue',
	time = 2022;
let user = {
	lesson,
	time
}
//  这里对象里的this向外层获取lesson
console.log(user); // {lesson: 'vue', time: 2022}
```



### 1-4 属性检测

`in` 操作符用来判断对象中一个属性是否存在，会获取原型上继承的属性，读取不存在的属性时返回 `undefined`

`in` 操作符左边的必须是属性名，通常是一个带引号的字符串

如果省略引号，左边解析为一个变量，它应该包含要判断的实际属性名

```js
"key" in object
let obj = {
	school: '三峡',
	study: '安全技术与管理'
}
// 当school不添加引号会去访问school变量
console.log('school' in obj); // true
```



### 1-5 for...in 遍历对象

`for...in`语句以任意顺序遍历一个对象的除 `Symbol` 以外的 可枚举属性，包括继承的可枚举属性

`for ... in`是为遍历对象属性而构建的，不建议与数组一起使用，数组可以用`Array.prototype.forEach()` 和 `for ... of`

`for...in` 循环有两个使用注意点：

- 遍历对象里可枚举属性
- 它不仅遍历对象自身的属性，还遍历继承的属性

```js
let obj = {
	user: 'jsx',
	age: '22',
	grilFriend: {
		name: 'ljj',
		age: 23
	}
}
for (let key in obj) {
	console.log(obj[key])
} // jsx 22 {name: 'ljj', age: 23}
```



## 2. 构造器与 new 操作符

### 2-1 工厂函数

所谓工厂函数，就是指这些内建函数都是类对象，当你调用他们时，实际上是创建了一个类实例

当调用这个函数，实际上是先利用类创建了一个对象，然后返回这个对象

工厂函数理解：

* 它是一个函数
* 它用来创建对象
* 它像工厂一样，生产出来的函数都是标准件（拥有同样的属性）

```js
function methods(name, grilfriend, favorite) {
	let arr = {};
	arr.name = name;
	arr.grilfriend = grilfriend;
	arr.favorite = favorite;
	return arr;
}
console.log(methods('jsx', 'ljj', ['code', '看剧']));
// {name: 'jsx', grilfriend: 'ljj', favorite: Array(2)}
```



### 2-2 构造函数

当任意一个普通函数用于创建一类对象时，它就被称作构造函数，或构造器

一个函数要作为一个真正意义上的构造函数，需要满足下列条件：

1. 在函数内部对新对象 `this` 的属性进行设置，通常是添加属性和方法
2. 构造函数可以包含返回语句（不推荐）
3. 它们的命名以大写字母开头，它们只能由 `new` 操作符来执行

```js
function Methods(name, mygril, message) {
	this.name = name;
	this.mygril = mygril;
	this.message = message;
	this.tellme = function() {
		console.log(this.message)
	}
}
let obj = new Methods('jsx', 'ljj', '520')
obj.tellme();
```



###  2-3 new 操作符

当一个函数被使用 `new` 操作符执行时，它按照以下步骤：

1. 一个新的空对象被创建并分配给 `this`
2. 函数体执行。通常它会修改 `this`，为其添加新的属性
3. 返回 `this` 的值

```js
function User(name) {
  // this = {};（隐式创建）

  // 添加属性到 this
  this.name = name;
  this.isAdmin = false;

  // return this;（隐式返回）
}

let user = {
  name: "Jack",
  isAdmin: false
};
```



### 2-4 new.target

使用 `new.target` 属性来检查它是否被使用 `new` 进行调用了

对于常规调用，它为 `undefined`，对于使用 `new` 的调用，则等于该函数本身

```js
function fn(name) {
	console.log(name)
	console.log(new.target)
}
fn('jsx'); // jsx undefined
let obj = new fn('jsx'); // jsx fn(name) {}
```



### 2-5 构造函数 return

通常，构造器没有 `return` 语句

- 如果 `return` 返回的是一个对象，则返回这个对象，而不是 `this`
- 如果 `return` 返回的是一个原始类型，则忽略

```js
// 当构造函数返回一个对象时，就返回这个对象
function Method(name) {
	this.name = name;
	return {
		name: 'ljj'
	};
}
let obj = new Method('jsx');
console.log(obj.name); // ljj

// 当return返回一个原始类型时，则会忽略
function fn(user) {
	this.user = user;
	return 'ljj'
}
let obj1 = new fn('jsx');
console.log(obj1.user); // jsx
```



## 3. 引用特性与 this

### 3-1 对象引用特性

对象和函数、数组一样是引用类型，即复制只会复制引用地址

赋值了对象的变量存储的不是对象本身，而是该对象在内存中的地址，也就是对该对象的引用

* 当一个对象变量被复制 —— 引用被复制，而该对象自身并没有被复制

```js
let obj = {
	name: 'jsx'
}
let obj1 = obj;
obj1.age = 22;
console.log(obj); // {name: 'jsx', age: 22}
console.log(obj1); // {name: 'jsx', age: 22}
```

* 对象做为函数参数使用时也不会产生完全赋值，内外共用一个对象

```js
// 对象最为函数参数也不会完全赋值，共用同一对象
let user = {
	name: 'jsx'
}

function getUser(value) {
	value.name = 'ljj'
}
getUser(user);
console.log(user); // {name: 'ljj'}
```



### 3-2 对象中的 this

`this` 指当前对象的引用，作为对象属性的函数被称为方法

* 方法中的 `this` 指向调用该方法的对象
* 在没有对象的情况下调用 `this` 等于 `window`，严格模式下为 `undefined`
* 箭头函数没有 `this` ，在箭头函数内部访问到的 `this` 都是从外部获取的

```js
// 方法中调用 this 指向该对象
let obj = {
	name: 'jsx',
	fn: function() {
		console.log(this); // {name: 'jsx', fn: ƒ}
		console.log(this.name) // jsx
	}
}
obj.fn();

// 在没有对象情况下 this 指向window
// 严格模式下为undefined
function global() {
	'use strict'
	console.log(this); // undefined
}
global(); // Window {window: Window, self: Window, document: document, name: '', location: Location, …}

// 对象中的箭头函数通过上下文获取this
let method = {
	num: 2,
	sum: function() {
		return () => {
			console.log(this) // {num: 2, sum: ƒ}
			console.log(this.num); //  2
		}
	}
}
method.sum()();
```

