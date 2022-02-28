# Number 数字类型详解



## 1. Number类型

在现代 JavaScript 中，数字 `Number` 有两种类型：

* `JavaScript` 中的常规数字以 64 位的格式 `IEEE-754` 存储，也被称为双精度浮点数。

* `BigInt` 数字，用于表示任意长度的整数。有时会需要它们，因为常规数字不能安全地超过 2<sup>53</sup> 或小于 -2<sup>53</sup>，仅在少数特殊领域才会用到 `BigInt`。

```js
let num = 22; // Number
let num1 = 23426456456456n; // BigInt
```

| 属性                     | 描述                                                   |
| ------------------------ | ------------------------------------------------------ |
| Number.MAX_VALUE         | JavaScript 中所能表示的最大值                          |
| Number.MIN_VALUE         | JavaScript 中所能表示的最小值                          |
| Number.NaN               | 非数字                                                 |
| Number.NEGATIVE_INFINITY | 负无穷，在溢出时返回                                   |
| Number.POSITIVE_INFINITY | 正无穷，在溢出时返回                                   |
| Number.EPSILON           | 表示 1 与 Number 所能表示的大于 1 的最小浮点数之间的差 |
| Number.MIN_SAFE_INTEGER  | 最小安全整数，即 -9007199254740991                     |
| Number.MAX_SAFE_INTEGER  | 最大安全整数，即 9007199254740991                      |

**整数** 就是整数,例如 10， 400，或者 -5

**浮点数** (浮点) 有小数点或小数位，例如 12.5，和 56.7786543，浮点数值的最高精度是 17 位小数 

**双精度** 双精度是一种特定类型的浮点数，它们具有比标准浮点数更高的精度



## 2. 数字编写方式

`ES2021`，允许`JavaScript` 的数值使用下划线（`_`）作为分隔符

这个数值分隔符没有指定间隔的位数，也就是说，可以每三位添加一个分隔符，也可以每一位、每两位、每四位添加一个，小数和科学计数法也可以使用数值分隔符

```js
let num = 1_000_000_000;// 1000000000
// 与1000000000相等

let num1 = 0.00_00_7; // 0.00007
// 与0.00007相等
```

数值分隔符有几个使用注意点

- 不能放在数值的最前面或最后面
- 不能两个或两个以上的分隔符连在一起
- 小数点的前后不能有分隔符
- 科学计数法里面，表示指数的 `e` 或 `E` 前后不能有分隔符



在 `JavaScript` 中，我们可以通过在数字后面附加字母 `"e"` 并指定零的个数来缩短数字

`e` 把数字乘以 `1` 后面跟着给定数量的 `0` 的数字

`e` 后面的负数表示除以 1 后面跟着给定数量的 0 的数字

```js
let num = 1e5; // 100000

// 0.34的左边有两个0
let num1 = 0.34e-2; // 0.34/100 = 0.0034
```



## 3. 进制转换

`num.toString(base)`

 返回在给定 `base` 进制数字系统中 `num` 的字符串表示形式

`base` 的范围可以从 `2` 到 `36`。默认情况下是 `10`


- **base=2** 计算机的最基础语言，主要用于调试按位操作，数字可以是 `0` 或 `1`
- **base=8** 数字可以是 `0..7`
- **base=10** 全世界通用的十进制，数字可以是 `0..9` ，进行算数计算时，八进制和十六进制表示的数值最终都将被转换成十进制数值
- **base=16** 用于十六进制颜色，字符编码等，数字可以是 `0..9` 或 `A..F`
- **base=36** 是最大进制，数字可以是 `0..9` 或 `A..Z`。所有拉丁字母都被用于了表示数字

```js
// num.toString(base) base范围为2-36 默认为10
let num = 255;
console.log(num.toString(10)); // 255
console.log(num.toString(2)); // 11111111
console.log(num.toString(8)); // 377
console.log(num.toString(16)); // ff
console.log(num.toString(36)); // 73


// 将m进制的num 转换为n进制
function main(num, m, n) {
	var s = num + '';
	var result = parseInt(s, m)
		.toString(n);
	return result;
}
main('ff', 16, 2);
console.log(main('ff', 16, 2));// 11111111
```



## 4. 浮点数计算问题

### 4-1 浮点数精度

> **Tips：**PHP，Java，C，Perl，Ruby 给出的也是完全相同的结果，因为它们基于的是相同的数字格式

在 `JavaScript` 开发中，都有遇到过浮点数运算精度误差的问题

```js
console.log(0.1 + 0.2 == 0.3); // false
console.log(0.1 + 0.2); // 0.30000000000000004
```

 `0.1` 和 `0.2` 的总和是否为 `0.3`，我们会得到 `false`

一个数字以其二进制的形式存储在内存中，在十进制数字系统中看起来很简单的 `0.1`，`0.2` 这样的小数，实际上在二进制形式中是无限循环小数

`IEEE-754` 数字格式通过将数字舍入到最接近的可能数字来解决此问题。这些舍入规则通常不允许我们看到极小的精度损失，但是它确实存在，当我们对两个数字进行求和时，它们的精度损失会叠加起来



### 4-2 `toFixed()`

`num.toFixed(n)` 把数字转换为字符串，结果的小数点后有指定位数的数字（四舍五入）

```js
let num = 3.1415926;
console.log(num.toFiexd(2)); // 3.14
console.log(typeof num.toFixed(2)); // string
```



### 4-3 解决浮点数计算问题

在小于 `Number.MAXSAFEINTEGER` 范围的 安全整数是可以被精确表示出来的，所以可以先把

小数转化为整数，运算得到结果后再转化为对应的小数

```js
function addPointNum(arg1, arg2) {
	// 获取arg1参数 小数点后数字长度
	let lastNumLength1 = arg1.toString().split('.')[1].length;
	console.log(lastNumLength1);

	// 获取arg2参数 小数点后数字长度
	let lastNumLength2 = arg2.toString().split('.')[1].length;
	console.log(lastNumLength2);

	// 通过小数点长度来乘以10的倍数化整
	let baseNum = Math.pow(10, Math.max(lastNumLength1,lastNumLength2));
	console.log(baseNum);
	let result = ((arg1 * baseNum) + (arg2 * baseNum)) / baseNum;
	console.log(result);
	return result;
}

addPointNum(0.1, 0.2); // 0.3
```





## 5. Number 对象方法

### 5-1 `toPrecision()`

`num.toPrecision(n)`

以指定的精度返回该数值对象的字符串表示，四舍五入到参数 `n` 指定的显示数字位数

* 该参数是 1 ~ 100 之间（且包括 1 和 100 ）的值
* 如果省略了该参数，则调用方法 `toString()`，返回原始数字的字符串形式
* 如果参数 `n` 不在 1 和 100 （包括）之间，将会抛出一个 `RangeError` 

```js
// num.toprecision() 返回指定长度的数值字符串
let num = 1.37234;
// 值会四舍五入
console.log(num.toPrecision(2)); // 1.4

// 指定的值必须在1-100内，如果不在则会抛出一个错误
console.log(num.toPrecision(101)); // error

// 省略参数时，则会调用toString()方法，返回原始数字的字符串形式
console.log(num.toPrecision()); // 1.37234
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220225220114.png)



### 5-2 `toExponential()`

`num.toExponential(n)`

返回以指数表示法（计数法）表示数字的字符串

```js
let num = 2.59345;

// 小数部分会四舍五入
console.log(num.toExponential(1)); // 2.6e+0
```



### 5-2 `valueOf()`

`num.valueOf()`

返回一个 `Number` 对象的基本数字值

```js
let num = 15;
console.log(num.valueOf()); // 15
console.log(typeof num.valueOf()); // number
```



### 5-3 `isFinite()`

`Number.isFinite(value)` 或 `isFinite(value)`

> **Tips：**ES6 在`Number`对象上，新提供了 `Number.isFinite()` 方法

用来检查一个数值是否为有限的，即不是`Infinity`

如果 `number` 是 `NaN`（非数字），或者是正、负无穷大的数，则返回 `false`

* 全局的 `isFinite()` 会先把检测值转换为 `Number` 类型 ，然后再检测值是否为有限的
* `Number` 对象方法：`Number.isFinite()` 不会将检测值转换为 `Number` 对象，如果检测值不是 `Number` 类型，则返回 `false`

```js
console.log(isFinite(NaN)); // false
console.log(isFinite(Infinity)); // false
console.log(isFinite(-Infinity)); // false

// 全局isFinity()方法 会先将值转换为Number类型再检测值是否有限
console.log(isFinite('2')); // 转化为数字2 在检测

// Number对象方法 Number.isFinity() 不会发生类型转换而是直接判断是否为数字类型再检测
console.log(Number.isFinite('2')); // false
```



### 5-4 `isNaN()`

`Number.isNaN(value)`

> **Tips：**ES6 在`Number`对象上，新提供了 `Number.isNaN()` 方法

用来检查一个值是否为`NaN`

* 全局的 `isNaN()` 会先把检测值转换为 `Number` 类型 ，然后再检测值是否为 `NaN`

* `Number` 对象方法：`Number.isNaN()` 不会自行将参数转换成数字，只有在参数是值为 `NaN` 时，才会返回 `true`

```js
console.log(isNaN(NaN)); // true
console.log(isNaN(Infinity)); // false
console.log(isNaN(-Infinity)); // false

// 全局isNaN() 会将检测值转换为数字类型再检测
console.log(isNaN('34a')); // true 

// Number对象方法 Number.isNaN() 不会自行将检测值转换为字符类型，只有在参数为NaN时才返回true
console.log(Number.isNaN('34a')); // false
```



### 5-5 `isInteger()`

`Number.isInterger(value)`

确定传递的值类型是 `Number` 类型，且是整数

```js
let num = 123;
console.log(Number.isInteger(num)); // true

let num2 = 3.14;
console.log(Number.isInteger(num2)); // false

let str = 'jsx';
console.log(Number.isInteger(str)); // false
```



### 5-6 `isSafeInteger()`

`Number.isSafeInteger(Value)`

确定传递的值是否为安全整数 - (2<sup>53</sup> - 1) 至 2<sup>53</sup> - 1 之间，包含  - (2<sup>53</sup> - 1) 和 2<sup>53</sup> - 1

* `Number.MIN_SAFE_INTEGER`：最小安全整数 - 2<sup>53</sup> - 1  
* `Number.MAX_SAFE_INTEGER`：最大安全整数 2<sup>53</sup> - 1  

```js
let num = 10;
console.log(Number.isSafeInteger(num)); // true

// 超出范围2的53次幂 返回false
console.log(Number.isSafeInteger(9007199254740992)); // false 

// 最大安全整数
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991

// 最小安全整数
console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
```



### 5-7 `parselnt()`

`parseInt(string, radix)` 或 `Number.parseInt(string, radix)`

> **Tips：**ES6 将全局方法`parseInt()`，移植到`Number`对象上面，行为完全保持不变

可解析一个字符串，并返回一个整数

* 允许开头和结尾的空格

* 从字符串中读取数字，直到无法读取为止。如果发生 `error`，则返回收集到的数字
* 如果第一个字符串就是非数字，返回 `NaN`
* 如果是有小数的数字，小数点后的数字会被去除
* `parseInt` 解析 `BigInt` 为 `Number` , 丢失精度。因为末位 `n` 字符被丢弃
* `parseInt` 不可以解析无穷大或负无穷大，会返回 `NaN`

```js
// 解析字符串规则：
// 从左到右解析，当第一个字符无法解析则返回NaN;
// 解析到小数点或者无法解析的字符时会返回小数点前或无法解析字符前的数字
console.log(parseInt(2.62)); // 2
console.log(parseInt('asfas')); // NaN

// 全局方法 parseInt()
console.log(parseInt('24a1')); // 24
// Number对象方法number.parseint();
console.log(Number.parseInt('23.1')); // 23

// 解析BigInt类型问题 末位n字符被丢弃
console.log(Number.parseInt(123123241n)); // 123123241
console.log(typeof Number.parseInt(123123241n)); // number

// 不可以返回Infinity
console.log(Number.parseInt(Infinity)); // NaN
```



`radix `表示要解析的数字的基数。该值介于 `2 ~ 36` 之间

如果 `radix` 是 `undefined`、`0`或未指定的，`JavaScript` 会假定以下情况：

* 如果输入的 `string` 以 `0x` 或 `0x`（一个0，后面是小写或大写的 `X`）开头，那么 `radix` 被假定为16，字符串的其余部分被当做十六进制数去解析

* 如果输入的 `string`以 `0`开头， `radix`被假定为 `8`（八进制）或 `10`（十进制）。具体选择哪一个 `radix` 取决于实现。`ECMAScript 5` 澄清了应该使用十进制，但不是所有的浏览器都支持。**因此，在使用 `parseInt` 时，一定要指定一个 `radix`**

* 如果输入的 `string` 字符串以任何其他值开头， `radix` 是 `10` (十进制)

```js
// parseInt() 第二个参数radix 表示解析的数字的基数 范围2-36
// 字符串以0X开头默认16进制解析
console.log(Number.parseInt('0X56')) // 86

// 字符串以0开头，es5以10进制解析，使用时建议指定第二个参数值
console.log(Number.parseInt('0123')); // 123

// 字符串以其它值开头默认10进制解析
console.log(Number.parseInt('102')); // 102
```



### 5-8 `parseFloat()`

`parseFloat(string)` 或 `Number.parseFloat(string)`

> **Tips：**ES6 将全局方法 `parseFloat()`，移植到`Number`对象上面，行为完全保持不变

可解析一个字符串，并返回一个浮点数

* 允许开头和结尾的空格
* 第二个小数点的出现也会使解析停止（在这之前的字符都会被解析）
* 如果参数字符串的第一个字符不能被解析成为数字,`则` `parseFloat` 返回 `NaN`
* 如果 `parseFloat` 在解析过程中遇到了正号（`+`）、负号（`-`）、数字（`0`-`9`）、小数点（`.`）、或者科学记数法中的指数（e 或 E）以外的字符，则它会忽略该字符以及之后的所有字符，返回当前已经解析到的浮点数
* `parseFloat` 也可以解析无穷大或负无穷大并返回 `Infinity`
* `parseFloat`解析 `BigInt` 为 `Number` , 丢失精度。因为末位 `n` 字符被丢弃

```js
// 字符串中出现第二个浮点数会停止解析
console.log(Number.parseFloat('2.6.8')); // 2.6

// 参数无法解析返回NaN
console.log(Number.parseFloat('asdas')); // NaN

// 可以返回Infinity
console.log(Number.parseFloat(1 / 0)); // Infinity

// 解析BigInt类型问题 末位n字符被丢弃
console.log(Number.parseFloat(123123241n)); // 123123241
console.log(typeof Number.parseFloat(123123241n)); // number
```



