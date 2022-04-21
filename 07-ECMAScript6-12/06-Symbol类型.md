# Symbol 类型

## 1. Symbol 声明

根据规范，对象的属性键只能是字符串类型或者 `Symbol` 类型

`Symbol` 是一种基本数据类型，`Symbol()`函数会返回 `symbol` 类型的值，`Symbol` 值是唯一的标识符，不会与其他属性名产生冲突

> **Tips：**`Symbol` 函数前不能使用 `new` 命令，会报错。这是因为生成的 `Symbol` 是一个原始类型的值，不是对象类型

```js
let symbol1 = Symbol();
let symbol2 = Symbol();
console.log(symbol1 === symbol2); // false
```



## 2. Symbol 描述与转换

### 2-1 description 描述

`Symbol` 函数可以接受一个字符串作为参数，表示对 `Symbol` 实例的描述

`ES2019` 提供了一个实例属性 `description`，直接返回 `Symbol` 的描述

```js
// Symbol接受一个字符串作为参数
let sym = Symbol(1);
// description来获取Symbol描述
console.log(typeof sym.description); // [object String]
```



如果 `Symbol` 的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个 `Symbol` 值

```js
let obj = {
    toString() {
        return 'symbol';
    }
}
let sym1 = Symbol(obj);
console.log(sym1.description); // symbol
```



### 2-2 类型转换

* `Symbol` 值不会发生隐式转换，不会被自动转换为字符串
* 如果我们真的想显示一个 `Symbol` 值，可以通过 `toString()` 显式转为字符串

* `Symbol` 值也可以转为布尔值，但是不能转为数值

```js
// symbol值不会发生隐式转换
let symbol = Symbol();
// console.log(+symbol); Uncaught TypeError
// 通过toString()转换
let str = symbol.toString();
console.log(str, typeof str); // Symbol() string

// symbol可以转换为布尔类型
let bool = Boolean(symbol);
console.log(bool); // true

// 不能转为数字类型
let num = Number(symbol);
console.log(num);
// Cannot convert a Symbol value to a number
```



## 3. 作为属性名

由于每一个 `Symbol` 值都是不相等的，这意味着 `Symbol` 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性

* `Symbol` 作为对象属性的三种写法

```js
// symbol作为对象属性三种写法
let sym = Symbol();
let obj = {};
// 添加属性方式
name[sym] = 'hello';

let obj1 = {
	// 直接设置属性
	[sym]: 'hello'
}

let obj2 = {};
// defineProperty添加属性
Object.defineProperty(obj2, sym, {
	value: 'hello'
})
```



* `Symbol` 声明和访问使用 `[]`  方括号形式操作，不能使用 `.` 点语法因为 `.` 点语法是操作字符串属性的

下面写法是错误的，会将 `Symbol` 属性当成字符串处理

```js
let sym1 = Symbol();
let obj3 = {};
obj3.sym1 = 'hello'
console.log(obj3[sym1]); // undefined
console.log(obj3['sym1']); // hello
```



* 在对象的内部，使用 `Symbol` 值定义属性时，`Symbol` 值必须放在方括号之中

```js
let symbol = Symbol();
let objover = {};
objover[symbol] = 'hello';
console.log(objover[symbol]); // hello
```



## 4. 属性遍历与复制



### 4-1 Symbol 属性遍历

`Symbol` 作为属性名，遍历对象时的情况：

* 不会出现在`for...in`、`for...of` 循环中
*  `Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()` 也会忽略 `Symbol` 属性
* `Object.assign` 会同时复制字符串和 `Symbol` 属性

```js
for (let item in obj) {
	console.log(item); // name
}
for (let item of Object.keys(obj)) {
	console.log(item); // name
}

console.log(Object.getOwnPropertyNames(obj)); // ['name']
console.log(JSON.stringify(obj)); //{"name":"jsx"}

// object.assign会复制symbol属性
let newobj = {...obj};
console.log(newobj); 
// {name: 'jsx', Symbol(): 'hello'}
```



### 4-2 获取 Symbol 属性

`Object.getOwnPropertySymbols()` 

可以获取指定对象的所有 `Symbol` 属性名，该方法返回一个数组，数组元素是当前对象的所有用作属性名的 `Symbol` 值

```js
let sym = Symbol('学习');
let sym1 = Symbol('努力');
let sym2 = Symbol('加油');
let obj = {
	[sym]: 'hello',
	[sym1]: 'hello1',
	[sym2]: 'hello2',
}
console.log(obj[sym]); // hello
let symbolAll = Object.getOwnPropertySymbols(obj)
console.log(symbolAll);
// [Symbol(学习), Symbol(努力), Symbol(加油)]
```



## 5. Symbol.for()、Symbol.keyFor() 全局

### 5-1 Symbol.for()

通常所有的 `Symbol` 都是不同的，即使它们有相同的名字，如果需要使用同一个 `Symbol` 值，可以通过 `Symbol.for()` 方法来实现

`Symbol.for()` 接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 `Symbol` 值

> **Tips：** `Symbol.for()`、`Symbol.keyFor()` 都只获取全局的 `Symbol`

* 如果有，就返回这个 `Symbol` 值
* 否则就新建一个以该字符串为名称的  `Symbol` 值，并将其注册到全局

```js
let sym = Symbol('hello');
let sym1 = Symbol('hello')
console.log(sym === sym1); // false

// 未找到注册到全局
let sym2 = Symbol.for('hello');
console.log(sym === sym2); // false

// 找到相同的 Symbol 使用
let sym3 = Symbol.for('hello');
console.log(sym3 === sym2); // true
```



### 5-2 Symbol.for() 与 Symbol() 区别

* `Symbol.for()` —— 会被登记在全局环境中供搜索，不会每次调用就返回一个新的 `Symbol` 类型的值，而是会先检查给定的 `key` 是否已经存在，如果不存在才会新建一个值
* `Symbol()` —— 不会被登记在全局环境中供搜索，每次调用都会返回一个不同的值

```js
// Symbol() 创建的symbol不会注册在全局中
let symbol = Symbol('描述');
let sym = Symbol.for('描述');
console.log(sym); // Symbol(描述) 全局下的
console.log(symbol === sym); // false

// Symbol.for() 创建的symbol会注册在全局中
let sym1 = Symbol.for('描述');
console.log(sym1 === sym); // true

// Symbol()每次使用都是不同的值
let sym2 = Symbol();
let sym3 = Symbol();
console.log(sym2 === sym3); // false

// Symbol.for()会先寻找全局下是否有相同描述的symbol供使用
// 有则使用，没有则新创建全局symbol
```



### 5-3 Symbol.keyFor()

`Symbol.keyFor(sym)` 通过全局 `Symbol` 返回一个 `Symbol` 描述或者名字

如果 `Symbol` 不是全局的，它将无法找到它并返回  `undefined`

```js
let sym = Symbol.for('hello');
let symkeyfor = Symbol.keyFor(sym);
console.log(symkeyfor); // hello
        
// 不是全局Symbol返回undefined
let sym1 = Symbol('嘻嘻');
let symkeyfor1 = Symbol.keyFor(sym1);
console.log(symkeyfor1); // undefined
```



### 5-4 Reflect.ownKeys()

`Reflect.ownKeys()`方法可以返回所有类型的键名，包括常规键名和 `Symbol` 键名

```js
let str = 'haha';
let sym = Symbol('symbol')
let obj = {
	name: 'jsx',
	[str]: 'haha',
	[sym]: 'symbol'
}
let allKeys = Reflect.ownKeys(obj)
console.log(allKeys); // ['name', 'haha', Symbol(symbol)]
```



## 6. Symbol 使用场景

### 6-1 解决对象同名属性冲突

作为对象属性 当一个复杂对象中含有多个属性的时候，很容易将某个属性名覆盖掉，利用 `Symbol` 值作为属性名可以很好的避免这一现象

```js
 // let user1 = 'jsx';
 // let user2 = 'jsx';

 let user1 = Symbol('jsx');
 let user2 = Symbol('jsx');
 let obj = {
 	[user1]: ['html', 'css', 'js'],
 	[user2]: ['html', 'css', 'js', 'vue']
 }
 // 后面同名属性覆盖前面属性
 // console.log(obj[user1]); // ['html', 'css', 'js', 'vue'] 
 // console.log(obj[user2]); // ['html', 'css', 'js', 'vue']

 console.log(obj[user1]); // ['html', 'css', 'js'] 
 console.log(obj[user2]); // ['html', 'css', 'js', 'vue']
```



### 6-2 消除魔术字符串

当在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值时，可以通过 `Symbol` 消除魔术字符串，改由含义清晰的变量代替

```js
const TYPE_AUDIO = Symbol('audio')
const TYPE_VIDEO = Symbol('video')
const TYPE_IMAGE = Symbol('image')

function handleFileResource(resource) {
	switch (resource.type) {
		case TYPE_AUDIO:
			console.log(resource)
			break
		case TYPE_VIDEO:
			console.log(resource)
			break
		case TYPE_IMAGE:
			preconsole.log(resource)
			break
		default:
			console.log('错误')
	}
}
handleFileResource({
	type: TYPE_AUDIO
})
// {type: Symbol(audio)}
```



### 6-3 对象属性私有化

`Symbol` 作为对象属性会隐藏自身属性，`Symbol` 属性不会被 `for...in`, `Object.keys()` 等等遍历方法遍历

```js
let name = Symbol('jsx')

function User() {
	this.lesson = ['jsx', 'ljj'];
	this.title = 'hello';
	this[name] = 'jsx';
}
let user = new User();
console.log(user);
// User {lesson: Array(2), title: 'hello', Symbol(jsx): 'jsx'}
for (let item in user) {
	console.log(item); // lesson title
}
```

