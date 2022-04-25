# Reflect 映射

## 1. Reflect 映射

`Reflect`对象与 `Proxy` 对象一样，也是 `ES6` 为了操作对象而提供的新 `API`

`Reflect` 是一个内置的对象，而不是一个函数对象，因此它是不可构造的

`Reflect` 作用：

* 优化 `Object` 的一些操作方法以及合理的返回 `Object` 操作返回的结果
* `Proxy` 在拦截访问目标对象，或者对代理对象操作都是通过 `Reflect` 映射来完成

```js
let obj = {
  name: 'jsx'
};
// 获取对象属性
console.log(Reflect.ownKeys(obj)); // ['name']

// 结合Proxy使用
let proxy = new Proxy(obj, {
  get(target, prop, receiver) {
    return Reflect.get(target, prop, receiver);
  },
  set(target, prop, value, receiver) {
    return Reflect.set(target, prop, value, receiver)
  }
})
console.log(proxy.name); // jsx
console.log(proxy.title = 'Reflect映射'); // Reflect映射
```



## 2. Reflect 静态方法

对于每个可被 `Proxy` 捕获的内部方法，在 `Reflect` 中都有一个对应的方法，其名称和参数与 `Proxy` 捕捉器相同

`Proxy` 对象可以方便地调用对应的 `Reflect` 方法，完成默认行为，作为修改行为的基础。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为



### 2-1 Reflect.get

`Reflect.get(target, name, receiver)`

* `Reflect.get`方法查找并返回`target`对象的`name`属性，如果没有该属性，则返回`undefined`
* 如果`name`属性部署了读取函数 `get()`，则读取函数的`this`绑定`receiver`
* 如果第一个参数不是对象，`Reflect.get`方法会报错``

```js
// Reflect.get(target, name, receiver) 返回target对象的name属性
let myobj = {
	name: 'jsx',
	age: 22,
	get info() {
		return this.name + ' ' + this.age
	}
}
console.log(Reflect.get(myobj, 'name')) // jsx
// 没有属性返回undefined
console.log(Reflect.get(myobj, 'names')); // undefined
// 设置 get()函数后 this 指向第三个参数 receiver
let receiver = {
	name: 'ljj',
	age: 23
}
console.log(Reflect.get(myobj, 'info', receiver)); // ljj 23

// 第一个参数必须是对象，否则报错
// console.log(Reflect.get(true, 'name')) Uncaught TypeError
```



### 2-2 Reflect.set

`Reflect.set(target, name, value, receiver)`

* `Reflect.set`方法设置`target`对象的`name`属性等于`value`

* 如果`name`属性设置了赋值函数，则赋值函数的`this`绑定`receiver`
* 如果第一个参数不是对象，`Reflect.set`会报错

```js
// Reflect.set(target, name, value, receiver)
// 设置target对象的name属性等于value 返回布尔值
let myobj = {
	name: 'jsx',
	age: 22,
	set info(value) {
		return this.name
	}
}

console.log(Reflect.set(myobj, 'info', 'javascript')); // true
console.log(myobj.name) // jsx
let receiver = {
	name: 'vue'
}
// 设置第四个参数后，set() 赋值函数this指向第四个参数receiver
console.log(Reflect.set(myobj, 'info', 'javascript', receiver))
console.log(receiver.name); // vue

// 第一个参数必须为对象
// console.log(Reflect.set(true, 'info')) Uncaught TypeError
```



如果 `Proxy`对象和 `Reflect`对象联合使用，前者拦截赋值操作，后者完成赋值的默认行为，并且传入了`receiver`，那么`Reflect.set`会触发`Proxy.defineProperty`拦截

```js
// proxy和Reflect联合使用时 指定第四个参数后会触发proxy中的definedProperty拦截器
let title = {
	lesson: 'jsx'
}
let proxy = new Proxy(title, {
	set(target, key, value, receiver) {
		console.log('触发')
		return Reflect.set(target, key, value, receiver)
	},
	defineProperty(target, key, desc) {
		console.log('触发')
		return Reflect.defineProperty(target, key, desc)
	}
})
console.log(proxy.lesson = 'javascript')
```



### 2-3 Reflect.has

`Reflect.has(obj, name)`

* `Reflect.has`方法对应`name in obj`里面的`in`运算符
* 如果`Reflect.has()`方法的第一个参数不是对象，会报错

```js
// Reflect.has(obj, name)
// 对应name in obj里面的in运算符
let obj = {
	name: 'jsx'
}

// 旧写法
console.log('name' in obj) // true

// 新写法
console.log(Reflect.has(obj, 'name')); // true
```



### 2-4 Reflect.deleteProperty

`Reflect.deleteProperty(obj, name)`

`Reflect.deleteProperty`方法等同于`delete obj[name]`，用于删除对象的属性

* 该方法返回一个布尔值。如果删除成功，或者被删除的属性不存在，返回`true`；删除失败，被删除的属性依然存在，返回`false`
* 如果`Reflect.deleteProperty()`方法的第一个参数不是对象，会报错

```js
// Reflect.deleteProperty(obj, name)
// 删除对象属性
let obj = {
	name: 'jsx',
	lesson: 'js'
}
// 旧写法
delete obj.name;
console.log(obj); // {lesson: 'js'}

// 新写法
console.log(Reflect.deleteProperty(obj, 'lesson')); // true
console.log(obj); // {}

// 第一个参数必须为对象，否则报错
// console.log(Reflect.defineProperty(true, 'lesson'))
```



### 2-5 Reflect.apply

`Reflect.apply(func, thisArg, args)`

`Reflect.apply`方法等同于`Function.prototype.apply.call(func, thisArg, args)`，调用一个方法并且显式地指定 `this` 变量和参数列表，参数列表可以是数组，或类似数组的对象

```js
// Reflect.apply(func, thisarg, args) 
// 指定函数，指定this指向，参数数组 类似于Function.prototype.apply.call(func, thisArg, args)
let obj = {
	name: 'jsx'
}

function getName(name) {
	return name
}
// 旧写法
console.log(Function.prototype.apply.call(getName, obj, ['ljj'])); // ljj

// 新写法
console.log(Reflect.apply(getName, obj, ['jsx'])); // jsx
```



### 2-6 Reflect.construct

`Reflect.construct(target, args)`

`Reflect.construct`方法等同于`new target(...args)`，这提供了一种不使用`new`，来调用构造函数的方法

* 如果`Reflect.construct()`方法的第一个参数不是函数，会报错

```js
// Reflect.construct(target, args)
// 类似于 new调用函数 相等于不使用new调用构造函数
function User(name) {
	this.name = name
}

// new 写法
let user = new User('jsx');
console.log(user); // User {name: 'jsx'}

// Reflect.construct
let user1 = Reflect.construct(User, ['ljj'])
console.log(user1); // User {name: 'ljj'}

// 第一个参数不是函数会报错
// let user2 = Reflect.construct(true, ['ljj']); Uncaught TypeError
```



### 2-7 Reflect.getPrototypeOf

`Reflect.getPrototypeOf(obj)`

`Reflect.getPrototypeOf`方法用于读取对象的`__proto__`属性，对应`Object.getPrototypeOf(obj)`

`Reflect.getPrototypeOf`和`Object.getPrototypeOf`的区别：

* 如果参数不是对象，`Object.getPrototypeOf`会将这个参数转为对象，然后再运行
* 如果参数不是对象，`Reflect.getPrototypeOf`会报错

```js
// Reflect.getPrototype(obj)
// 用于获取对象的原型，相等于__proto__
function User() {}
let user = new User()

// 旧的写法
console.log(Object.getPrototypeOf(user));
console.log(Object.getPrototypeOf(1)); // 参数不是对象会转换成对象类型

// 新的写法
console.log(Reflect.getPrototypeOf(user))
// console.log(Reflect.getPrototypeOf(1)); // 参数不是对象会报错
```



### 2-8 Reflect.setPrototypeOf

`Reflect.setPrototypeOf(obj, newProto)`

`Reflect.setPrototypeOf`方法用于设置目标对象的原型，对应`Object.setPrototypeOf(obj, newProto)`方法。它返回一个布尔值，表示是否设置成功

* 如果无法设置目标对象的原型目标对象禁止扩展，`Reflect.setPrototypeOf`方法返回`false`

* 如果第一个参数不是对象，`Object.setPrototypeOf`会返回第一个参数本身，而`Reflect.setPrototypeOf`会报错
* 如果第一个参数是`undefined`或`null`，`Object.setPrototypeOf`和`Reflect.setPrototypeOf`都会报错

```js
// Reflect.setPrototypeOf(obj, newProto)
// 类似于Objec.setPrototypeOf方法，用于设置对象的原型
let myobj = {
	name: 'jsx'
}
let proto = {
	proto: 'ljj'
};

// 旧的写法
console.log(Object.setPrototypeOf(myobj, proto));
// 设置成功返回成功后的对象
console.log(myobj)
// 第一个参数不是对象，返回第一个参数
console.log(Object.setPrototypeOf(1, proto)); // 1
// 第一个参数为null或者undefined 会报错
// Object.setPrototypeOf(null, proto)

// 新的写法
console.log(Reflect.setPrototypeOf(myobj, proto)); // 设置成功返回true
// 第一个参数不是对象，报错
// console.log(Reflect.setPrototypeOf(1, proto)); // Uncaught TypeError
// 第一个参数为null或者undefined 会报错
// Reflect.setPrototypeOf(null, proto)
```



### 2-9 Reflect.defineProperty

`Reflect.defineProperty(target, propertyKey, attributes)`

`Reflect.defineProperty`方法基本等同于`Object.defineProperty`，用来为对象定义属性

* 如果`Reflect.defineProperty`的第一个参数不是对象，就会抛出错误
* 可以与`Proxy.defineProperty`配合使用

```js
// Reflect.defineProperty(target, key, description)
// 设置对象的描述特征 相当于Object.defineProperty
let obj = {
  name: 'jsx'
};
let proxy = new Proxy(obj, {
  defineProperty(target, key, desc) {
    console.log(desc);
    // {value: 'ljj', writable: true, enumerable: true, configurable: true}
    return Reflect.defineProperty(target, key, desc)
  }
})
console.log(Object.defineProperty(proxy, 'name', {
  value: 'ljj',
  writable: true,
  enumerable: true,
  configurable: true
})); // {name: 'ljj'}

// 如果第一个参数不是对象，会报错
// Reflect.defineProperty(true, 'name', {value: 'js'}) Uncaught TypeError
```



### 2-10 Reflect.getOwnPropertyDescriptor

`Reflect.getOwnPropertyDescriptor(target, propertyKey)`

`Reflect.getOwnPropertyDescriptor`基本等同于`Object.getOwnPropertyDescriptor`，用于得到指定属性的描述对象

* `Reflect.getOwnPropertyDescriptor` 如果第一个参数不是对象会抛出错误
* `Object.getOwnPropertyDescriptor` 如果第一个参数不是对象不报错，返回`undefined`

```js
// Reflect.getOwnPropertyDescriptor(target, propertyKey)
// 等同于Object.getOwnPropertyDescriptor 获取对象属性描述
let obj = {};
Object.defineProperty(obj, 'name', {
	value: 'jsx',
	writable: true,
	enumerable: true,
	configurable: true
})

// 旧的写法
console.log(Object.getOwnPropertyDescriptor(obj, 'name'));
// {value: 'jsx', writable: true, enumerable: true, configurable: true}
// 参数不为对象返回undefined
console.log(Object.getOwnPropertyDescriptor(true, 'name')); // undefined

// 新的写法
console.log(Reflect.getOwnPropertyDescriptor(obj, 'name'));
// {value: 'jsx', writable: true, enumerable: true, configurable: true}
// 参数不为对象报错
// console.log(Reflect.getOwnPropertyDescriptor(true, 'name')); // Uncaught TypeError
```



### 2-11 Reflect.isExtensible

`Reflect.isExtensible (target)`

`Reflect.isExtensible`方法对应`Object.isExtensible`，返回一个布尔值，表示当前对象是否可扩展

* 如果参数不是对象，`Object.isExtensible`会返回`false`，
* 如果参数不是对象，`Reflect.isExtensible`会报错

```js
// Reflect.isExtensible(target)
// 等同于Object.isExtensible 对象是否可扩展 返回布尔在
let obj = {
	name: 'jsx'
};
// 旧的写法
console.log(Object.isExtensible(obj)) // true
// 参数不是对象返回false
console.log(Object.isExtensible(true)) // false

// 新的写法
console.log(Reflect.isExtensible(obj)); // true
// 参数不是对象报错
// console.log(Reflect.isExtensible(true)) // 报错
```



### 2-12 Reflect.preventExtensions

`Reflect.preventExtensions(target)`

`Reflect.preventExtensions`对应`Object.preventExtensions`方法，用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功

* 如果参数不是对象，`Object.preventExtensions`在 ES5 环境报错，在 ES6 环境返回传入的参数
* 如果参数不是对象，`Reflect.preventExtensions` 会报错

```js
// Reflect.preventExtensions(target)
// 等同于Object.preventExtensions 设置对象不可扩展
let obj = {
	name: 'jsx'
};

// 旧的写法
Object.preventExtensions(obj);
console.log(Object.isExtensible(obj)); // false
// 参数不是对象返回传入的参数
console.log(Object.preventExtensions(1))

// 新的写法
Reflect.preventExtensions(obj);
console.log(Reflect.isExtensible(obj)); // false
// 参数不是对象报错
// Reflect.preventExtensions(1) // 报错
```



### 2-13 Reflect.ownKeys

`Reflect.ownKeys (target)`

`Reflect.ownKeys`方法用于返回对象的所有属性，基本等同于`Object.getOwnPropertyNames`与`Object.getOwnPropertySymbols`之和

* 如果`Reflect.ownKeys()`方法的第一个参数不是对象，会报错

```js
// Reflect.ownKeys(target)
// 获取对象的所有属性，字符串类型和Symbol类型
// Object.getOwnPropertySymbols + Object.getOwnPropertyNames
let obj = {
	name: 'jsx',
	'title': 'Reflect',
	[Symbol('hello')]: 'hello'
}
console.log(Reflect.ownKeys(obj))
// ['name', 'title', Symbol(hello)]

// 参数不是对象则会报错
// console.log(Reflect.ownKeys(true)); // 报错
```

