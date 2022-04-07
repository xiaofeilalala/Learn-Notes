# JSON 对象



## 1. 什么是 JSON ？

`JSON` 全称是 `JavaScript Object Notation`，即 `JavaScript` 对象标记法

`JSON` 是一种轻量级（Light-Meight)、基于文本的(Text-Based)、可读的(Human-Readable)格式

`JSON` 是存储和交换文本信息的语法，类似 `XML`，`JSON` 比 `XML` 更小、更快，更易解析

`JavaScript` 能够使用内建的 `eval()` 函数，用 ` JSON`  数据来生成原生的 `JavaScript` 对象

```json
{
    "sites": [
    { "name":"林俊杰" , "url":"她说" }, 
    { "name":"周杰伦" , "url":"晴天" }, 
    { "name":"邓紫棋" , "url":"泡沫" }
    ]
}
```



## 2. JSON 语法

- 数组（Array）用方括号 `"[]"` 表示
- 对象（0bject）用大括号 `"{}"` 表示
- 名称 / 值对(`name/value`）组合成数组和对象
- 名称( `name` ）置于双引号中，值（`value`）有字符串、数值、布尔值、`null`、对象和数组
- 并列的数据之间用逗号 `","` 分隔
- 名称/值对包括字段名称（在双引号中），后面写一个冒号，然后是值

`JSON` 不支持注释。向 `JSON` 添加注释无效

`JSON` 文件的文件类型是 `.json`

`JSON` 文本的 `MIME` 类型是 `application/json`

```html
<script type="application/json"></script>
```

```json
let options = {
	"age": 30,
	"name": "JSON学习",
	"url": "www.runoob.com",
	"sites": [
        { "name":"林俊杰" , "url":"她说" }, 
        { "name":"周杰伦" , "url":"晴天" }, 
        { "name":"邓紫棋" , "url":"泡沫" }
	],
	"flag": true,
	"runoob": null
}
// JSON 可以直接使用现有的 JavaScript 对象解析
console.log(options.name);
```



## 3. JSON 和 XML

* `JSON` 和 `XML` 都用于接收 `web` 服务端的数据，`JSON` 和 `XML` 在写法上有所不同

* `JSON` 没有结束标签，长度更短，读写更快，`JSON` 可以直接使用现有的 `JavaScript `对象解析，可以使用数组

```json
// json
{
	"name": "jsx",
	"age": 22,
	"fruits": ["apple", "pear", "grape"]
}
```

```xml
// xml
<root>
	<name>jsx</name>
	<age>22</age>
	<fruits>apple</fruits>
	<fruits>pear</fruits>
	<fruits>grape</fruits>
</root>
```



## 4. JSON 解析与序列化

- `JSON.stringify` 将对象转换为 `JSON`
- `JSON.parse` 将 `JSON` 转换回对象



### 4-1 JSON.parse

`JSON.parse(str, reviver);`

用来解析 `JSON` 字符串，构造由字符串描述的 `JavaScript` 值或对象，传入的字符串不符合 `JSON` 规范会报错

* `str`：要解析的 `JSON` 字符串
* `reviver`：可选的函数 `function(key,value)`，该函数将为每个 `(key, value)` 对调用，并可以对值进行转换

```js
// JSON.parse() 解析JSON字符串， 将JSON转换为对象
let json = '{"name": ["js", "webpack"], "age": 22, "gridFriend": "ljj"}'
console.log(JSON.parse(json)) // {name: Array(2), age: 'jsx'}

// 第二个参数是一个可选参数用来调用key和value
let result = JSON.parse(json, (key, value) => {
	if (key == "age") {
		return `年龄：${value}`
	}
	return value
})
console.log(result)
// {name: Array(2), age: '年龄：22', gridFriend: 'ljj'}
```



### 4-2 JSON.stringify

`JSON.stringify(value, replacer, space)`

将一个 `JavaScript` 对象或值转换为 `JSON` 字符串

如果指定了一个 `replacer` 函数，则可以选择性地替换值，或者指定的 `replacer` 是数组，则可选择性地仅包含数组指定的属性

* `value`：将要序列化成 一个 `JSON` 字符串的值
* `replacer`
  * 如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理
  * 如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 `JSON` 字符串中
  * 如果该参数为 `null` 或者未提供，则对象所有的属性都会被序列化
* `space`：指定缩进用的空白字符串，用于美化输出
  * 如果参数是个数字，它代表有多少的空格；上限为10。该值若小于1，则意味着没有空格
  * 如果该参数为字符串（当字符串长度超过10个字母，取其前10个字母），该字符串将被作为空格
  * 如果该参数没有提供（或者为 null），将没有空格

```js
// JSON.stringify(value, replacer, space)
// 将js对象转换为JSON字符串
let obj = {
	name: 'jsx',
	age: 22,
	lesson: ['html', 'css', 'js']
}
let json = JSON.stringify(obj)
console.log(json) // {"name":"jsx","age":22,"lesson":["html","css","js"]}

// 第二个参数replacer 为函数时，被序列化的值得属性都会经过该函数转换处理
let obj1 = {
	lesson: 'vue',
	date: 2022,
	flag: true
}

function replacer(key, value) {
	console.log(key, value)
	if (typeof value === "string") {
		return undefined;
	}
	return value;
}
let result = JSON.stringify(obj1, replacer)
console.log(result); // {"date":2022,"flag":true}

// 当replacer参数为数组，数组的值代表将被序列化成 JSON 字符串的属性名
let result1 = JSON.stringify(obj1, ['lesson', 'date'])
// 只保留 “lesson” 和 “date” 属性值
console.log(result1); // {"lesson":"vue","date":2022}

// 第三个参数spcae,用来控制结果字符串里面的间距
let obj2 = {
	name: 'jsx',
	age: 22,
	lesson: ['jsx', 'css', 'vue']
}
let result2 = JSON.stringify(obj2, null, 4)
console.log(result2)

/* {
    "name": "jsx",
    "age": 22,
    "lesson": [
        "jsx",
        "css",
        "vue"
    ]
} */
```



### 4-3 JSON 转换格式

`JSON.stringify()`将值转换为相应的 `JSON`格式：

- 转换值如果有 `toJSON()` 方法，该方法定义什么值将被序列化
- 非数组对象的属性不能保证以特定的顺序出现在序列化后的字符串中
- 布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值
  - `undefined`、任意的函数以及 `symbol` 值，在序列化过程中会被忽略（出现在非数组对象的属性值中时）或者被转换成 `null`（出现在数组中时）。函数、`undefined` 被单独转换时，会返回 `undefined`，如`JSON.stringify(function(){})` or `JSON.stringify(undefined)`
- 对包含循环引用的对象（对象之间相互引用，形成无限循环）执行此方法，会抛出错误
- 所有以 `symbol` 为属性键的属性都会被完全忽略掉，即便 `replacer` 参数中强制指定包含了它们
- `Date` 日期调用了 `toJSON()` 将其转换为了 `string` 字符串（同`Date.toISOString()`），因此会被当做字符串处理
- `NaN` 和 `Infinity` 格式的数值及 `null` 都会被当做 `null`
- 其他类型的对象，包括 `Map/Set/WeakMap/WeakSet`，仅会序列化可枚举的属性

```js
// JSON.stringify()
// 将js对象转换为JSON字符串
console.log(JSON.stringify({})); // '{}'
console.log(JSON.stringify(true)); // 'true'
console.log(JSON.stringify("foo")); // '"foo"'
console.log(JSON.stringify([1, "false", false])); // '[1,"false",false]'
console.log(JSON.stringify({
	x: 5
})); // '{"x":5}'

console.log(JSON.stringify({
	x: 5,
	y: 6
}));
// "{"x":5,"y":6}"

console.log(JSON.stringify([new Number(1), new String("false"), new Boolean(false)]));
// '[1,"false",false]'

console.log(JSON.stringify({
	x: undefined,
	y: Object,
	z: Symbol("")
}));
// '{}'

console.log(JSON.stringify([undefined, Object, Symbol("")]));
// '[null,null,null]'

console.log(JSON.stringify({
	[Symbol("foo")]: "foo"
}));
// '{}'

console.log(JSON.stringify({
	[Symbol.for("foo")]: "foo"
}, [Symbol.for("foo")]));
// '{}'

console.log(JSON.stringify({
		[Symbol.for("foo")]: "foo"
	},
	function(k, v) {
		if (typeof k === "symbol") {
			return "a symbol";
		}
	}
))


// undefined

// 不可枚举的属性默认会被忽略：
console.log(JSON.stringify(
	Object.create(
		null, {
			x: {
				value: 'x',
				enumerable: false
			},
			y: {
				value: 'y',
				enumerable: true
			}
		}
	)
))
// "{"y":"y"}"
```



## 5. JSON 对象与数组



### 5-1 JSON对象

* `JSON` 对象使用在大括号 `{}` 中书写。
* 对象可以包含多个`key/value`（键/值）对
* `key` 必须是字符串，`value` 可以是合法的  `JSON` 数据类型（字符串, 数字, 对象, 数组, 布尔值或 null）
* `key` 和 `value` 中使用冒号 `:` 分割
* 每个 `key/value` 对使用逗号 `,` 分割

```json
{ "name":"runoob", "alexa":10000, "site":null }
```

可以使用点号 `.` 或 `[]` 来访问对象的值

```js
let json = {"name": "jsx", "age": 22}
console.log(json['name'], json.age) // jsx 22
```

可以循环，嵌套，修改，删除 `JSON`对象属性

```js
// JSON对象
// 可通过. []来访问属性值
let json = {
	"name": "jsx",
	"age": 22
}
console.log(json['name'], json.age) // jsx 22

// 可循环
for (let key in json) {
	console.log(json[key]); // jsx 22
}

// 可修改
json.name = "ljj";
console.log(json); // {name: 'ljj', age: 22}

// 可删除
delete json.age;
console.log(json); // {name: 'ljj'}

// 可嵌套
let json1 = {
	"name": "runoob",
	"alexa": 10000,
	"sites": {
		"site1": "www.runoob.com",
		"site2": "m.runoob.com",
		"site3": "c.runoob.com"
	}
}
console.log(json1)
```



### 5-2 JSON数组

* `JSON` 数组在中括号中书写
* `JSON` 中数组值必须是合法的 `JSON` 数据类型（字符串, 数字, 对象, 数组, 布尔值或 null）
* `JavaScript` 中，数组值可以是以上的 ` JSON` 数据类型，也可以是 `JavaScript` 的表达式，包括函数，日期，及 `undefined`

```json
[ "Google", "Runoob", "Taobao" ]
```

可以使用索引值来访问数组

```js
let arrJson = ["jsx", "ljj", "ddc"]
console.log(arrJson[0]); // jsx
```

可以循环，嵌套，修改，删除 `JSON`数组元素

```js
// JSON数组
// 可通过索引来访问
let arrJson = ["jsx", "ljj", "ddc"]
console.log(arrJson[0]); // jsx

// 可循环
for (let key of arrJson) {
	console.log(key); // jsx ljj ddc
}

// 可修改
arrJson[0] = "jsx ❤ 520";
console.log(arrJson); // ['jsx ❤ 520', 'ljj', 'ddc']

// 可删除
delete arrJson[2];
console.log(arrJson); // ['jsx ❤ 520', 'ljj', 空白]

// 可嵌套
let arrJson1 = {
	"name": "网站",
	"num": 3,
	"sites": [{
			"name1": [["jsx", "ljj", "ddc"]]
		},
		{
			"name2": [["jsx", "ljj", "ddc"]]
		},
		{
			"name3": [["jsx", "ljj", "ddc"]]
		}
	]
}
console.log(arrJson1)
```

