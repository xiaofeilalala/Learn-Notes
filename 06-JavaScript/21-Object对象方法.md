# Object 对象方法



## 1. 对象原始转换

对象中不存在布尔转换，只有字符串和数值转换

数值转换发生在对象相减或应用数学函数时

字符串转换通常发生在 `alert()` 输出时

为了进行转换，`JavaScript` 尝试查找并调用三个对象方法：

* `obj[Symbol.toPrimitive](hint)`

*  `obj.toString()` 

* `obj.valueOf()`



### 1-1 Symbol.toPrimitive

`Symbol.toPrimitive` 的内建 `symbol`，它被用来给转换方法命名

如果 `Symbol.toPrimitive` 方法存在，可以用来处理所有的转换场景

类型转换有三种变体，它们被称为 `hint`：

* `string`：对象到字符串的转换，当我们对期望一个字符串的对象执行操作时
* `number`：对象到数字的转换，例如当我们进行数学运算时
* `default`：当运算符不确定期望值的类型时

```js
obj[Symbol.toPrimitive] = function(hint) {
  // 这里是将此对象转换为原始值的代码
  // 它必须返回一个原始值
  // hint = "string"、"number" 或 "default" 中的一个
}

let obj = {
	name: 'jsx',
	age: 22,
	money: true,
	[Symbol.toPrimitive](hint) {
		console.log(hint)
		return hint == 'string' ? `{name: '${this.name}'}` : this.age;
	}
}
console.log(obj + 2); // 24
console.log(obj + 'ljj'); // 22ljj
console.log(+obj); // 22
```



### 1-2 toString() & valueOf()

如果没有 `Symbol.toPrimitive`，那么 `JavaScript` 将尝试寻找 `toString` 和 `valueOf` 方法，转换并不限制返回类型

* 对于 `string hint`：`toString`，如果它不存在，则 `valueOf`
* 对于其他 `hint`：`valueOf`，如果它不存在，则 `toString`

默认情况下，普通对象具有 `toString` 和 `valueOf` 方法：

- `toString` 方法返回一个字符串 `[object Object]`
- `valueOf` 方法返回对象自身

```js
let obj = {
	name: 'jsx',
	age: 22,
	money: true,
	valueOf: function() {
		return this.age
	},
	toString: function() {
		return this.name
	}
}
// toString 返回字符串
console.log(obj + 3); // 22

// valueOf 返回对象自身
console.log(`${obj}`); // jsx
```



## 2. 属性方法

### 2-1 检测属性

`obj.hasOwnProperty(prop)`

* `prop`：要检测的属性的 `String` 字符串形式表示的名称，或者 `Symbol`

方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性，不检测原型链上继承的属性

```js
let obj = {
	name: 'jsx',
	age: 22
}
console.log(obj.hasOwnProperty('name')); // true

// 在obj的原型对象中有hasOwnProperty属性
console.log(obj.hasOwnProperty('hasOwnProperty')); // false
```



### 2-2 获取属性名

`Object.getOwnPropertyNames(obj)`

返回一个由指定对象的所有自身属性的属性名组成数组

包括不可枚举属性但不包括 `Symbol` 值作为名称的属性

```js
let obj = {
	name: 'jsx',
	age: 22,
	if: true,
	fn: function() {}
}
console.log(Object.getOwnPropertyNames(obj)); // ['name', 'age', 'if', 'fn']
```



### 2-3 复制属性

`Object.assign(target, ...sourcse)`

* `target`：目标对象
* `sourcse`：源对象

用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象

* 如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖
* 后面的源对象的属性将类似地覆盖前面的源对象的属性

```js
 let obj = {
 	name: 'jsx'
 }
 let age = {
 	age: 22
 }
 let user = Object.assign({}, obj, age);
 console.log(user); // {name: 'jsx', age: 22}

 // 目标对象键 = 源对象键 源对象覆盖
 let age1 = {
 	age: 23
 }
 let user1 = Object.assign(age, age1); // {age: 23}
 console.log(user1)

 // 后面源属性 > 源属性 覆盖
 let age2 = {
 	age: 24
 }
 let user2 = Object.assign(age, age1, age2); // {age: 24}
 console.log(user2)
```



### 2-4 可枚举&不可枚举属性

`obj.propertyIsEnumerable(prop)`

返回一个布尔值，表示指定的属性是否可枚举

可枚举属性：是指那些内部可枚举标志设置为 true 的属性。对于通过直接的赋值和属性初始化的属性，该标识值默认为即为 true

可枚举与不可枚举通过 `enumerable` 属性设置决定，值为 `true`/ `false`，当 `enumerable` 属性为 `false`时不可枚举

下列三种方法无法遍历不可枚举属性：

* `for..in` 循环
* `Object.keys` 方法
* `JSON.stringify` 方法

```js
let obj = {
	name: 'jsx',
	age: 22,
	lesson: 'js'
}
Object.defineProperty(obj, 'lesson', {
	enumerable: false
})
console.log(obj)

// obj.propertyIsEnumerable(prop) 判断指定属性是否可枚举
console.log(obj.propertyIsEnumerable('lesson')); // false

// 下面三种遍历方式不可遍历不枚举属性
for (let i in obj) {
	console.log(i) // name age
}
console.log(Object.keys(obj)); //  ['name', 'age']
console.log(JSON.stringify(obj)); // {"name":"jsx","age":22}
```



## 3. 对象遍历

### 3-1 Object.keys

`Object.keys(obj)`

返回一个由一个给定对象的自身可枚举属性组成的数组

数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致

```js
let obj = {
	name: 'jsx',
	age: 22,
	lesson: 'js',
	fn: function() {
		console.log(this.name)
	},
	grilFriend: {
		name: 'ljj'
	}
}
console.log(Object.keys(obj));
// ['name', 'age', 'lesson', 'fn', 'grilFriend']
```



### 3-2 Object.values

`Object.values(obj)`

返回一个给定对象自身的所有可枚举属性值的数组

值的顺序与使用  `for...in` 循环的顺序相同 ( 区别在于 `for-in` 循环枚举原型链中的属性 )

```js
console.log(Object.values(obj));
// ['jsx', 22, 'js', ƒ, {name: 'ljj'}]
```



### 3-3 Object.entries

`Object.entries(obj)`

返回一个给定对象自身可枚举属性的键值对数组

其排列顺序与使用  `for...in` 循环的顺序相同 ( 区别在于 `for-in` 循环枚举原型链中的属性 )

```js
console.log(Object.entries(obj));
// [['name', 'jsx'], ['age', 22], ['lesson', 'js'], ['fn', ƒ], ['grilFriend', {name: 'ljj'}]]
```



## 4. 属性特征

### 4-1 属性特征描述

对象属性（properties），除 `value` 外，还有三个特殊的特性，也就是所谓的标志

* `value` — 对象属性的值，默认值为 `undefined`

* `writable` — 如果为 `true`，则值可以被修改，否则它是只可读的
* `enumerable` — 如果为 `true`，则会被在循环中列出，否则不会被列出
* `configurable` — 如果为 `true`，则此属性可以被删除，这些特性也可以被修改，否则不可以



### 4-2 查看属性特殊

`Object.getOwnPropertyDescriptor(obj, prop)`

* `obj`：需要查找的目标对象
* `prop`：目标对象内属性名称

返回指定对象上一个自有属性对应的属性描述符（不需要从原型链上进行查找的属性）

```js
let obj = {
	name: 'jsx',
	age: 22,
	lesson: ['html', 'css']
}
// 通过Object.getOwnPropertyDescriptor() 查看特征
console.log(Object.getOwnPropertyDescriptor(obj, 'name'))
// {value: 'jsx', writable: true, enumerable: true, configurable: true}
```



`Object.getOwnPropertyDescriptors(obj)`

获取一个对象的所有自身属性的描述符

```js
// Object.getOwnPropertyDescriptors() 获取对象身上所有属性描述
console.log(Object.getOwnPropertyDescriptors(obj))
// age: { value: 22, writable: true, enumerable: true, configurable: true }
// lesson: { value: Array(2), writable: true, enumerable: true, configurable: true }
// name: { value: 'jsx', writable: true, enumerable: true, configurable: true }
```



### 4-3 配置属性特征

`Object.defineProperty(obj, prop, descriptor)` 

`obj`：要定义属性的对象

`prop`：要定义或修改的属性的名称或 `Symbol`（属性名为字符串）

`descriptor`：要定义或修改的属性描述符对象

方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象

```js
let obj = {
	name: 'jsx',
	age: 22
}
Object.defineProperty(obj, 'name', {
	value: 'ljj',
	configurable: false,
	enumerable: false,
	writable: false
})
// 只读
console.log(obj.name); // ljj
obj.name = 'xlb';
console.log(obj.name); // ljj
// 不能遍历
console.log(Object.keys(obj)); // ['age']
// 不能删除
delete obj.name;
console.log(obj); // {age: 22, name: 'ljj'}
// 不可配置
Object.defineProperty(obj, 'name', {
	value: 'ljj',
	configurable: true,
	enumerable: true,
	writable: true
})
obj.name = 'asdasd';
console.log(obj.name); // 报错 name属性不可配置
```



`Object.defineProperties(obj, descriptor)`

允许一次定义多个属性

```js
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```



## 5. 密封对象

### 5-1 禁止添加

`Object.preventExtensions(obj)`

让一个对象变的不可扩展，也就是永远不能再添加新的属性

```js
let obj = {
	name: 'jsx'
};
Object.preventExtensions(obj);
obj.age = 22;
console.log(obj); // {name: 'jsx'}
```



### 5-2 封闭对象

`Object.seal(obj)`

禁止添加/删除属性。为所有现有的属性设置 `configurable: false`

* 属性标记为不可配置，属性的值只要原来是可写的就可以改变

```js
let obj = {
	name: 'jsx'
};
Object.seal(obj);
// 禁止添加
obj.age = 22;
// 禁止删除
delete obj.name;
// 原先可读为true 则可修改
obj.name = 'ljj'
console.log(obj); // {name: 'ljj'}
```



### 5-3 冻结对象

`Object.freeze(obj)`

禁止添加/删除/更改属性。为所有现有的属性设置 `configurable: false`，` writable: false`

* 不能添加新属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值

```js
let obj = {
	name: 'jsx'
};
obj.age = 22;
delete name;
obj.name = 'ljj';
Object.defineProperty(obj, 'name', {
	value: 'xdm',
	configurable: true,
	enumerable: true,
	writable: true
})
console.log(obj); // {name: 'xdm', age: 22}
```



### 5-4 密封对象检测

`Object.isExtensible(obj)`：如果添加属性被禁止，则返回 `false`，否则返回 `true`

`Object.isSealed(obj)`：如果添加/删除属性被禁止，并且所有现有的属性都具有 `configurable: false`则返回 `true`

`Object.isFrozen(obj)`：如果添加/删除/更改属性被禁止，并且所有当前属性都是 `configurable: false, writable: false`，则返回 `true`

```js
let obj = {
	name: 'jsx'
}
Object.isExtensible(obj);
// 检测添加属性禁止
console.log(Object.isExtensible(obj)); // true

Object.seal(obj);
// 检测是否密封
console.log(Object.isSealed(obj)); // true

Object.freeze(obj);
检测是否冻结
console.log(Object.isFrozen(obj)); // true
```



## 6. 属性访问器

### 6-1 getter&setter

访问器属性是对象属性的两个特性，`setter` / `getter` 是在定义对象方法

* `set prop(val) {}`

将对象属性绑定到要调用的函数

在写入访问器属性时，会调用 `setter`函数并传入新值，这个函数负责决定如何处理数据，参数 `val` 用于设置属性新值

* `{get prop() {} } `

将对象属性绑定到查询该属性时将被调用的函数

在读取访问器属性时，会调用`getter` 函数，这个函数负责返回有效的值

```js
let obj = {
  get propName() {
    // 当读取 obj.propName 时，getter 起作用
  },

  set propName(value) {
    // 当执行 obj.propName = value 操作时，setter 起作用
  }
};
```

```js
let obj = {
	name: 'jsx',
	set newName(val) {
		console.log('设置属性时执行')
		console.log(val)
		this.name = val
	},
	get newName() {
		console.log('读取时执行')
		console.log(this.name)
	}
}
obj.newName; // jsx

obj.newName = 'ljj'; // ljj
```



### 6-2 访问器描述符

对于访问器属性，没有 `value` 和 `writable`，但是有 `get` 和 `set` 函数

- `get` —— 获取属性时调用的函数，默认值为 `undefined`
- `set` —— 写入属性时调用的函数，默认值为 `undefined`
- `enumerable` —— 与数据属性的相同
- `configurable` —— 与数据属性的相同

可以通过 `Object.defineProperty()` 来定义访问器属性

```js
let obj = {
 	name: 'jsx',
 	age: 22
 }
 Object.defineProperty(obj, 'userInfo', {
 	set(val) {
 		console.log(val);
 		[this.name, this.age] = val.split(',')
 	},
 	get() {
 		return `${this.name} ${this.age}`
 	},
 	// 不可枚举
 	enumerable: false,
 	// 不可删除
 	configurable: false
 })
 obj.userInfo;

 // 判断访问器属性是否能改
 obj.userInfo = 'ljj,23';
 console.log(obj); // obj 里有userInfo 属性
 // 可枚举
 console.log(Object.keys(obj)); // ['name', 'age']
 // 是否能删除
 delete obj.userInfo;
 console.log(obj)
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220329171714.png)
