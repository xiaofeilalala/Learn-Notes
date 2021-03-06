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

Rest 参数会收集剩余的所有参数，因此当剩余参数后面还有其它参数时会导致错误

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



### 4-1 `length` 属性

`arguments.length`

表示传入函数实参的个数

```js
function fn(a, ...b) {
	// 返回传入参数的数量
	console.log(arguments.length); // 3
    return arguments.length;
}
console.log(fn(1, 2, 3)); // 6
```



### 4-2 `callee` 属性

`arguments.callee `

`callee` 是 `arguments` 对象的一个属性，包含当前正在执行的函数

它可以用于引用该函数的函数体内当前正在执行的函数

```js
// callee属性
// 返回正在执行的函数
function newFn() {
	console.log('new');
	console.log(arguments.callee); // newFn()
}
newFn();
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220316223720.png)



### 4-3 `caller` 属性

返回调用指定函数的函数

* 如果一个函数 `f` 是在全局作用域内被调用的,则 `f.caller` 为`null`
* 如果一个函数是在另外一个函数作用域内被调用的,则`f.caller` 指向调用它的那个函数

该属性的常用形式 `arguments.callee.caller`替代了被废弃的 `arguments.caller`

```js
// callee属性
// 返回正在执行的函数
function newFn() {
	console.log('new');
	otherFn();
	console.log(arguments.callee); // newFn()
}
newFn();

function otherFn() {
	console.log('other');
	// caller 返回调用指定函数的函数
	console.log(arguments.callee.caller); // newFn()
}
```



### 4-2 箭头函数没有 `arguments` 

访问到的 `arguments` 并不属于箭头函数，而是属于箭头函数外部的函数，因此箭头函数没有 `arguments` 属性

```js
function fn1() {
	console.log(arguments);
	let showArg = () => console.log(arguments);
	showArg();
}

console.log(fn1(1, 2, 3));
```



## 5. 函数表达式（匿名函数）

### 5-1 创建函数

**函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用**

可以通过函数表达式来声明函数

函数主体被视为一个表达式，并且该表达式被分配给一个变量。使用这种语法定义的函数可以是命名函数或匿名函数

```js
let fn  = function([形参1, 形参2, ...形参N]){
	语句....
}
```



### 5-1 作为值得函数

函数不仅仅是一种语法，也可以是一个值，可以将函数赋值给其它变量

```js
let fn = function() {
	console.log('jsx');
}
let newFn = fn;
fn(); // jsx
newFn(); // jsx
```



### 5-2 匿名函数

匿名函数就是没有名字的函数

匿名函数通常与立即执行函数结合使用，因为匿名函数没有函数名不符合 `javascript` 语法，没办法调用，通过立即执行函数调用

```js
let anonymous = function (a) {
    console.log(a);
}
console.log(anonymous('vue')); // vue
```



### 5-3 命名函数表达式

命名函数表达式，指带有名字的函数表达式

* 它允许函数在内部引用自己
* 它在函数外是不可见的

```js
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("jsx"); // 使用 func 再次调用函数自身
  }
};

sayHi(); // Hello jsx

// 但这不工作
func(); // Error, func is not defined（在函数外不可见）
```



## 6. 立即执行函数

### 6-1 自执行函数 `IIFE`

立即调用函数表达式是在定义时就会立即执行的 `JavaScript` 函数

通过圆括号包裹一个匿名或者命名函数，再通过`()` 创建一个立即执行函数表达式

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



### 6-2 自执行函数参数

如果立即执行函数中需要全局变量，全局变量会被作为一个参数传递给立即执行函数

```js
(function(j){
//代码中可以使用j
})(i)
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220317005415.png)



## 7. 回调函数

`javascript` 中函数可以作为参数传递

一个函数被作为参数传递给另一个函数，传入的函数在另一个中被调用被称为回调函数



### 7-1 匿名回调函数

将匿名函数作为参数传递给其他函数

* 可以让我们不做命名的情况下传递函数（节省变量名的使用）
* 可以将函数调用操作委托给另一个函数（节省代码编写工作）
* 有助于提升性能

```js
// 匿名回调函数
function getName(name, callback) {
	// 函数传递一个name参数，一个callback函数
	callback(name);
	// callback参数为name值
}

// 调用外部定义函数
getName('jsx', function(name) {
	// 匿名函数回调,传入参数name 返回name
	console.log(name); // jsx
})
```



### 7-2 命名回调函数

命名函数作为参数传递给其他函数

```js
 // 命名回调函数
 function getAge(age, callback) {
 	callback(age);
 }

 function showAge(age) {
 	console.log(age);
 }
 getAge(22, showAge); // 22
```



## 8. 构造函数

### 8-1 创建函数

创建一个新的 `Function` 对象。直接调用此构造函数可以动态创建函数

* `functionName`表示函数名称
* `[arg1[, arg2[, ...argN]],]`表示可选的参数列表
* `functionBody`表示函数

```js
var functionName = new Function ([arg1[, arg2[, ...argN]],] functionBody)
```

- 函数的参数和函数体，都以字符串形式填写在括号中，以逗号分隔
- 在使用构造函数创建一个 `Function` 类型的对象时，会得到一个函数

```js
// 无参的函数
var fun = new Function('console.log("这是一个函数")')
fun() // 这是一个函数
// 带一个参数的函数
var fun = new Function('a', 'console.log("这个函数带一个参数：" + a)')
fun(100) // 这个函数带一个参数：100
// 带两个参数的函数
var fun = new Function(
  'a, b',
  'console.log("这是带两个参数的函数，分别是" + a + "和" + b);',
)
fun(100, 200) // 这是带两个参数的函数，分别是100和200
```



### 8-2 构造函数作用域

由 `Function` 构造函数创建的函数不会创建当前环境的闭包，它们总是被创建于全局环境

只能访问全局变量和自己的局部变量，不能访问它们被 `Function` 构造函数创建时所在的作用域的变量

```js
var x = 10;

function createFunction1() {
    var x = 20;
    return new Function('return x;'); // 这里的 x 指向最上面全局作用域内的 x
}

function createFunction2() {
    var x = 20;
    function f() {
        return x; // 这里的 x 指向上方本地作用域内的 x
    }
    return f;
}

var f1 = createFunction1();
console.log(f1());          // 10
var f2 = createFunction2();
console.log(f2());          // 20
```

