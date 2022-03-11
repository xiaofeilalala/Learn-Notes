# Function 函数



## 1. 函数简介

函数是 `JavaScript` 中的基本组件之一

 一个函数是 `JavaScript` 过程， 一组执行任务或计算值的语句，要使用一个函数，你必须将其定义在你希望调用它的作用域内

一个 `JavaScript` 函数用 `function` 关键字定义，后面跟着函数名和圆括号

```js
function fn () {
    ...
}
```



## 2. 函数使用

### 2-1 函数声明

**在函数声明被定义之前，它就可以被调用**

一个函数定义（也称为**函数声明**，或**函数语句**）由一系列的 `function `关键字组成

* 函数的名称
* 函数参数列表，包围在括号中并由逗号分隔
* 定义函数的 `JavaScript` 语句，用大括号`{}`括起来

```js
function name(parameter1, parameter2, ... parameterN) {
  ...body...
}
```



### 2-2 函数调用

函数可以通过其函数名来调用，后面还要加上一对圆括号和参数

定义的函数不会自动执行，需要通过函数名来调用函数才会执行代码

```js
function getName (name) {
    console.log('名字：' + name);
}
console.log('jsx'); // jsx
```



### 2-3 函数返回值

`return expression;`

`return`语句终止函数的执行，并返回一个指定的值给函数调用者

表达式的值会被返回，空值的 `return` 或没有 `return` 的函数返回值为 `undefined`

```js
function getName(nameArr) {
	let name;
	for (let i of nameArr) {
		console.log(i);
		if (i == 'jsx') {
			name = i;
            return name;
		}
	}
	
};
console.log('返回值：' + getName(['ljj', 'jsx', 'ddc'])); // jsx

// 空值的return或没有return的函数返回值为undefined
function fn() {
	return;
}
console.log(fn()); // undefined
```



不要在 `return` 与返回值之间添加新行，会影响 `return` 语句

返回的表达式写成跨多行的形式:

* 在 `return` 的同一行开始写此表达式
* 至少按照如下的方式放上左括号

```js
function scaleNum(a, b) {
	return (
	a + b;)
}
console.log(scaleNum(1, 2)); // 2
```



### 2-4 函数命名

函数就是行为，所以它们的名字通常是动词。它应该简短且尽可能准确地描述函数的作用。这样读代码的人就能清楚地知道这个函数的功能

- `get…` —— 返回一个值
- `calc…` —— 计算某些内容
- `create…` —— 创建某些内容
- `check…` —— 检查某些内容并返回 boolean 值

```js
showMessage(..)     // 显示信息
getAge(..)          // 返回 age（gets it somehow）
calcSum(..)         // 计算求和并返回结果
createForm(..)      // 创建表单（通常会返回它）
checkPermission(..) // 检查权限并返回 true/false
```



## 3. 参数

`JavaScript` 允许传入任意参数，即使函数内部不需要这些参数，也不影响调用

从 `ECMAScript 6` 开始，有两个新的类型的参数：

* 默认参数
* 剩余参数



### 3-1 形参与实参

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220311190121.png)

形参是在函数声明时设置的参数，实参指在调用函数时传递的值

* 实参数量小于形参时，形参值为默认参数，剩余参数则为空数组
* 实参数量大于形参时，超出的实参会被忽略并不会报错

```js
// 形参大于实参,超出的形参为默认参数
function fn(num1, num2, ...num3) {
	console.log(num1, num2, num3);
}
console.log(fn(1)); // 1 undefined []


// 实参数量大于形参时，超出的实参会被忽略并不会报错
function fn1(num1, num2) {
	console.log(num1, num2);
}
console.log(fn1(1, 2, 3, 4)); // 1 2
```



### 3-2 默认参数

在 `JavaScript` 中，在没有值或`undefined`被传入时使用默认形参

有参数（argument）未被提供，那么相应的值就会变成 `undefined`

```js
function fn(a, b) {
	console.log(a);
	console.log(b);
}

// 未传入参数b值返回默认值undefined
console.log(fn(1)); // undefined
```



后备默认参数，通过给默认形参指定假值，当传入 `undefined` 时函数会使用默认形参

```js
// 通过 || 运算符
function fn(a, b) {
	b = b || 2;
	return a + b;
}
console.log(fn(1)); // 3

// 给定默认形参
function fn1(a, b = 4) {
	return a + b;
}
console.log(fn1(2)); // 6
console.log(fn1(2, undefined)); // 6
```



在函数被调用时，参数默认值会被解析，默认参数可用于后面的默认参数

```js
function fn2(a, b, c = a + b) {
	return a, b, c;
}
console.log(fn2(1, 7)); // 8
```



### 3-3 剩余参数

剩余参数语法允许将不确定数量的参数集合成为数组

剩余参数可以通过使用三个点 `...` 并在后面跟着包含剩余参数的数组名称，来将它们包含在函数定义中，将剩余参数收集到一个数组中

* 可以把部分参数放到剩余参数中
* 可以把所有的参数都放到剩余参数中

```js
function fn (num1, num2, ...numN) {
    console.log(num1, num2, numN); // 1 2 [3, 4, 5, 6]
}
console.log(fn(1, 2, 3, 4, 5, 6));

function scalNum(...num) {
	// 返回数组为true
	console.log(Array.isArray(num), num); // true (3) [1, 2, 3]
	for (let i of num) {
		console.log(i)
	}

}
console.log(scalNum(1, 2, 3));
```



**Rest 参数必须放到参数列表的末尾**

Rest 参数会收集剩余的所有参数，因此下面这种用法没有意义，并且会导致错误

```js
function fn1(num1, ...numN, num2) {
	console.log(num1, numN, num2);
}
console.log(fn1(1, 2, 3, 4));
// error:Rest parameter must be last formal parameter
```



### 3-3 函数展开语法

```js
myFunction(...iterableObj)
```

可以在函数调用时, 将数组表达式或者 `string` 在语法层面展开

展开语法只适用于可迭代对象

```js
function fn(x, y, z) {
	return x + y + z;
}
let arr = [1, 2, 3];
console.log(fn(...arr)); // 6
console.log(...arr); // 1, 2, 3

function myFunction(v, w, x, y, z) {
	console.log(v, w, x, y, z)
	return v + w + x + y + z;
}
var args = [0, 1];
console.log(myFunction(-1, ...args, 2, ...[3]));
```



## 4. `arguments` 对象

函数的所有实际参数会被保存在一个类似数组的 `arguments` 对象中

`arguments` 只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数

 `arguments` 是一个类数组，也是可迭代对象，但它不是数组，它不支持数组方法

```js
function fn(a, ...b) {
	let result = 0;
	for (let i of b) {
		result += i
	}
	console.log(arguments)
	console.log(arguments[0] + arguments[1] + arguments[2]); // 6
	// 获取参数长度
	console.log(arguments.length); // 3
	return a + result;

}
console.log(fn(1, 2, 3)); // 6
```



**箭头函数是没有** `arguments` 对象的

访问到的 `arguments` 并不属于箭头函数，而是属于箭头函数外部的函数

```js
function fn1() {
	console.log(arguments);
	let showArg = () => console.log(arguments);
	showArg();
}

console.log(fn1(1, 2, 3));
```



## 5. 函数表达式（匿名函数）

**函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用**

可以通过函数表达式来声明函数

函数主体被视为一个表达式，并且该表达式被分配给一个变量。使用这种语法定义的函数可以是命名函数或匿名函数

```js
let fn = function () {};
```



函数是一个值，可以将函数赋值给其它变量

```js
let fn = function() {
	console.log('jsx');
}
let newFn = fn;
fn(); // jsx
newFn(); // jsx
```



未命名的函数被称为匿名函数

```js
let anonymous = function (a) {
    console.log(a);
}
console.log(anonymous('vue')); // vue
```



命名函数表达式，函数名称将只作为函数作用域内有效

```js
let fn = function fn1(a, b) {
	console.log(typeof fn1); // function
	return a + b;
}
console.log(typeof fn1); // undefined
console.log(fn(3, 5)); // 8
```



## 6. 立即执行函数

立即调用函数表达式是在定义时就会立即执行的 `JavaScript` 函数

通过圆括号加运算符里的一个匿名函数，再通过`()` 创建一个立即执行函数表达式

```js
(function sum () {
    console.log('执行')
})();

(function sum () {
    console.log('执行')
}());
```



当函数变成立即执行的函数表达式时，表达式中的变量不能从外部访问

```js
(function sum() {
	let res = 'jsx';
	console.log('执行')
})();
// 外部无法访问，变量私有
console.log(res1); // res1 is not defined

let b = (function() {
	var res2 = 'ljj'
})();
console.log(res2); // res2 is not defined
```



## 7. 回调函数

被作为实参传入另一函数，并在另一函数内被调用，用以来完成某些任务的函数，称为回调函数

```js
function getName(name) {
	console.log(name)
}

function editName(callback) {
	let result = prompt('你的名字？')
	callback(result);
}
editName(getName);
```

