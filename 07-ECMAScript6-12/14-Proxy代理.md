# Proxy 代理



## 1. Proxy 代理创建

`Proxy` 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义

`ES6` 原生提供 `Proxy` 构造函数，用来生成 `Proxy` 实例，`Proxy`对象需要传入两个参数

* `target` —— 被代理的目标对象，可以是任何东西，包括函数
* `handler` —— 代理配置，带有捕捉器即拦截操作的方法的对象，内部可以定义零个或多个代理函数

```js
let proxy = new Proxy(target, handler)
```

* 如果 `handler`  没有设置拦截，所有对 `proxy` 的操作都直接转发给了 `target` 目标对象

```js
// 创建代理
let obj = {};
let proxy = new Proxy(obj, {});

// 没有设置捕捉器则会将proxy操作直接转发给target
proxy.name = 'jsx';
console.log(proxy); // Proxy {name: 'jsx'}
```



## 2. Proxy 拦截器

* 代理对象必须与被代理的对象具有相同特性，被代理对象不能改的，代理对象同样不能改
* 代理对象的方法的返回值类型必须与被代理对象一致

`Proxy` 支持的 13 种拦截方法：

| 拦截器                                        | 描述                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| **get(target, propKey, receiver)**            | 拦截对象属性的读取                                           |
| **set(target, propKey, value, receiver)**     | 拦截对象属性的设置，返回一个布尔值                           |
| **has(target, propKey)**                      | `in` 操作符的捕捉器，返回一个布尔值                          |
| **deleteProperty(target, propKey)**           | `delete` 操作符的捕捉器，返回一个布尔值                      |
| **ownKeys(target)**                           | 拦截 `Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in` 循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()` 的返回结果仅包括目标对象自身的可遍历属性 |
| **getOwnPropertyDescriptor(target, propKey)** | 拦截 `Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象 |
| **defineProperty(target, propKey, propDesc)** | 拦截 `Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值 |
| **preventExtensions(target)**                 | 拦截 `Object.preventExtensions(proxy)`，返回一个布尔值       |
| **getPrototypeOf(target)**                    | 拦截 `Object.getPrototypeOf(proxy)`，返回一个对象            |
| **isExtensible(target)**                      | 拦截 `Object.isExtensible(proxy)`，返回一个布尔值            |
| **setPrototypeOf(target, proto)**             | 拦截 `Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截 |
| **apply(target, object, args)**               | 拦截 `Proxy` 实例作为函数调用的操作，`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)` |
| **construct(target, args, newTarget)**        | 拦截 `Proxy` 实例作为构造函数调用的操作，`new proxy(...args)` |



### 2-1 get()

`get(target, property, receiver)` 

该方法用于拦截某个属性的读取操作

* `target` —— 是目标对象，该对象被作为第一个参数传递给  `new Proxy`
* `property` —— 目标属性名
* `receiver` —— 如果目标属性是一个 `getter` 访问器属性，则 `receiver` 就是本次读取属性所在的 `this` 对象，通常是 `proxy` 实例本身

```js
// get(target, property, receiver) 读取属性
let obj = {
  name: 'jsx'
}
let proxy = new Proxy(obj, {
  get(target, property, receiver) {
    if (property in target) return target[property];
  }
})
console.log(proxy.name); // jsx
```



如果一个属性不可配置且不可写，则 `Proxy` 不能修改该属性，否则通过 `Proxy` 对象访问该属性会报错

```js
// 当一个对象不可读不可写 proxy则不能修改属性
let obj1 = Object.defineProperties({}, {
  name: {
    value: 'ljj',
    writable: false,
    configurable: false
  }
})

let proxy1 = new Proxy(obj1, {
  get(target, property, receiver) {
    // 原对象不可写不可配置，代理对象proxy则不可修改
    // return 'ljj'
    return target[property]
  }
});
console.log(proxy1.name); // ljj
```



### 2-2 set()

`set(target, property, value, receiver)`

该方法用来拦截对象属性的设置

* `target` —— 是目标对象，该对象被作为第一个参数传递给 `new Proxy`
* `property` —— 目标属性名称
* `value` —— 目标属性的值
* `receiver` —— 与 `get`  捕捉器类似，仅与 `setter` 访问器属性相关，通常是 `proxy` 实例本身

`set` 拦截无论成功或者失败应当返回一个布尔值，在严格模式中，`set` 拦截器如果没有返回`true`，就会报错，`set`代理返回`false`或者`undefined`，都会报错

```js
let obj = {
  name: 'jsx'
};
let proxy = new Proxy(obj, {
  set(target, property, value, receiver) {
    if (typeof value == 'string') {
      // 属性值必须是字符串才能赋值
      target[property] = value;
      return true
    } else {
      return false
    }
  }
})
proxy.name = 'ljj';
console.log(proxy.name); // 设置false后不会报错 无改变
```



如果目标对象自身的某个属性不可写，那么 `set` 方法将不会生效

对二级或以上的属性修改并不会触发 `set` 拦截器

```js
// 对象属性不可写。set拦截器不会生效
let obj1 = Object.defineProperty({}, 'name', {
  value: 'jsx',
  writable: false
})

let proxy1 = new Proxy(obj1, {
  set(target, property, value, receiver) {
    if (typeof value == 'string') {
      target[property] = value;
      return true
    } else {
      return false
    }
  }
})
proxy1.name = 22;
// 对二级以上属性，set拦截器不会触发
proxy1.name.title = 'javascript';
console.log(obj1.name); // jsx
```



### 2-3 apply()

`apply(target, object, args)`

函数调用、`call` 和 `apply` 操作时会被 `apply` 拦截器拦截

* `target` —— 目标对象
* `object` —— 目标对象的上下文对象（`this`）
* `args` —— 目标对象的参数数组

```js
let proxy = new Proxy(fn, {
  apply(target, object, args) {
    console.log(args)
    return fn(...args) * 2
  }
})

function fn(item1, item2) {
  return item1 + item2
}
console.log(proxy(3, 4)); // 14
console.log(proxy.call(null, 6, 4)); // 20
console.log(proxy.apply(null, [9, 4])); // 26
```



### 2-4 has()

`has(target, propKey)`

方法用来拦截 `hasOwnProperty` 操作与 `in`运算符，即判断对象是否具有某个属性时触发，`has` 方法只能返回布尔值，`return` 非布尔值会自动转成布尔值

* `target` —— 目标对象

* `propKey` —— 需查询的属性名

```js
// has 用来拦截对象是否有某个属性
let obj = {
  name: 'jsx',
  title: 'has'
}
let proxy = new Proxy(obj, {
  has(target, prop) {
    if (prop in target) {
      // 只能返回布尔值，反回非布尔值会自动转换为布尔值
      return true
    }
    return false
  }
})
console.log('name' in proxy); // true

// for in 循环不会触发has拦截器
for (let item in proxy) {
  console.log(item)
}
// hasOwnProperty 不会触发has拦截器
obj.hasOwnProperty('name')
```



`has()`方法不判断一个属性是对象自身的属性，还是继承的属性，`hasOwnProperty()`和 `for...in` 都不能触发 `has` 方法

如果原对象不可配置或者禁止扩展，这时`has()` 拦截会报错

```js
// 对象不可配置或者禁止扩展 has()会报错
let obj1 = {
  message: 'jsx'
};

// 对象不可扩展，不可配置
Object.preventExtensions(obj1);
Object.seal(obj1)

let proxy1 = new Proxy(obj1, {
  // has 拦截不可扩展，不可配置对象会报错
  has(target, prop) {
    if (prop in target) {
      // 只能返回布尔值，反回非布尔值会自动转换为布尔值
      return true
    }
    return false
  }
})
console.log('message' in proxy1); // Uncaught TypeError
```



### 2-5 construct()

`construct(target, args, newTarget)`

用于拦截`new`命令，当构造函数使用 `new` 关键调用时拦截

`construct()` 方法返回的必须是一个对象，否则会报错

`construct()` 拦截的是构造函数，所以它的目标对象必须是函数，否则就会报错

* `target` —— 目标对象
* `args` —— 构造函数的参数数组
* `newTarget` —— 创造实例对象时，`new`命令作用的构造函数

```js
function User(arg) {
  console.log(arg)
}
// 必须拦截的是函数
let proxy = new Proxy(User, {
  construct(target, args, newTarget) {
    console.log(args); // jsx
    return {
      value: 'abc'
    }
    // return 1; // 必须返回对象
  }
})
let result = new proxy('jsx');
console.log(result.value); // abc
```



### 2-6 deleteProperty()

`deleteProperty(target, propKey)`

* `target` —— 目标对象
* `propKey` —— 需删属性名

用于拦截 `delete` 操作，只能返回布尔值，非布尔值会被转成布尔值

如果这个方法抛出错误或者返回 `false`，当前属性就无法被`delete`命令删除

```js
// deleteProperty 拦截 delete 操作
let obj = {
  title: 'js'
};
let proxy = new Proxy(obj, {
  deleteProperty(target, prop) {
    if (prop === 'title') {
      // 只能返回布尔值，非布尔值会被转成布尔值
      delete target[prop]
      return true
    } else {
      return false
    }
  }
});
console.log(delete proxy.title); // true
```



### 2-7 defineProperty()

`defineProperty(target, propKey, descriptor)`

该方法拦截 `Object.defineProperty()` ，`Object.defineProperties()`、`obj.prop = 'value'` 等修改或者添加对象属性操作

* `target` —— 目标对象
* `propKey` —— 待检索其描述的属性名
* `descriptor` —— 待定义或修改的属性的描述符

该方法只能返回布尔值，非布尔值会转成布尔值，返回 `fasle` 会导致报错

```js
let obj = {
  title: 'js'
}

// 拦截defineProperty操作 
let proxy = new Proxy(obj, {
  defineProperty(target, prop, descript) {
    console.log(descript)
    return true
  }
})

console.log(Object.defineProperty(proxy, 'title', {
  value: 'vue'
}))
console.log(proxy.lesson = 'react')
```



如果目标对象不可扩展，则 `defineProperty()`不能增加目标对象上不存在的属性

如果目标对象的某个属性不可写或不可配置，则`defineProperty()`方法不可以改变这两个设置

```js
let obj1 = {
  lesson: 'js'
};

// 目标不可扩展，defineProperty不可添加不存在属性
// Object.preventExtensions(obj1)

// 不可写或不可配置, 代理后的对象不可改变这两个属性
Object.defineProperty(obj1, 'lesson', {
  value: 'js',
  writable: false,
  configurable: false
})
let proxy1 = new Proxy(obj1, {
  defineProperty(target, prop, descript) {
    console.log(descript)
    return true
  }
})
// Uncaught TypeError 不可扩展
// console.log(proxy1.name = 'jsx')

// console.log(Object.defineProperty(proxy1, 'lesson', {
//   value: 'vue',
//   writable: true,
//   enumerable: true
// })) // Uncaught TypeError
```



### 2-8 getOwnPropertyDescriptor()

`getOwnPropertyDescriptor(target, propKey)`

方法拦截 `Object.getOwnPropertyDescriptor()`，`Object.getOwnPropertyDescriptors()` 返回一个属性描述对象或者`undefined`

* `target` —— 目标对象
* `propKey` —— 待检索其描述的属性名

若返回值不是 `undefined`，则返回值必须是包含`configurable` 为 `true` 的对象，该值为 `false` 或省略会导致报错，若其他描述符未定义，以默认值填充，如果返回对象包含除描述符以外的属性，则该属性被忽略

```js
// 方法拦截 Object.getOwnPropertyDescriptor 操作
let obj = {
  name: 'jsx'
};

let proxy = new Proxy(obj, {
  getOwnPropertyDescriptor(target, prop) {
    return undefined
  }
})
// 可以返回undefined
console.log(Object.getOwnPropertyDescriptor(proxy, 'name')); // undefined

// {configurable: true} 属性值必须为true 返回false报错
let obj1 = {
  name: 'jsx'
}
let proxy1 = new Proxy(obj1, {
  getOwnPropertyDescriptor(target, prop) {
    return {
      configurable: true
    }
  }
})
console.log(Object.getOwnPropertyDescriptor(proxy1, 'name'));
// {value: undefined, writable: false, enumerable: false, configurable: true}

let obj2 = {
  name: 'jsx'
}
let proxy2 = new Proxy(obj2, {
  getOwnPropertyDescriptor(target, prop) {
    // return { configurable: false }
    // 不能返回false
  }
})
// 报错 Uncaught TypeError
console.log(Object.getOwnPropertyDescriptor(proxy2, 'name'));
```





### 2-9 getPrototypeOf()

`getPrototypeOf(target)`

主要用来拦截获取对象原型，拦截下面这些操作

- `__proto__`
- `Object.prototype.isPrototypeOf()`
- `Object.getPrototypeOf()`
- `Reflect.getPrototypeOf()`
- `instanceof`

`getPrototypeOf()` 方法的返回值必须是对象或者`null`，否则报错

如果目标对象不可扩展， `getPrototypeOf()` 方法必须返回目标对象的原型对象

```js
// getPrototypeOf 来拦截获取对象原型
// Object.prototype.__proto__
// Object.prototype.isPrototypeOf()
// Object.getPrototypeOf()
// Reflect.getPrototypeOf()
// instanceof
let obj = {
  name: 'jsx'
}
let proxy = new Proxy(obj, {
  getPrototypeOf(target) {
    return {
      message: '截获取对象原型'
    }
  }
})
console.log(obj.__proto__);
console.log(proxy.__proto__); //{message: '截获取对象原型'}
console.log(Object.getPrototypeOf(obj));
console.log(Object.getPrototypeOf(proxy)); // {message: '截获取对象原型'}
console.log(Object.prototype.isPrototypeOf(obj)); // true
console.log(Object.prototype.isPrototypeOf(proxy)); // true
console.log(obj instanceof Object) // true
console.log(proxy instanceof Object); // true
console.log(Reflect.getPrototypeOf(obj))
console.log(Reflect.getPrototypeOf(proxy)); // {message: '截获取对象原型'}
```



### 2-10 isExtensible()

`isExtensible(target)`

拦截 `Object.isExtensible()` 操作

该方法只能返回布尔值，否则返回值会被自动转为布尔值

这个方法有一个强限制，它的返回值必须与目标对象的 `isExtensible` 属性保持一致，否则就会抛出错误

```js
// isExtensible(target)
let obj = {
  name: 'jsx'
};
let proxy = new Proxy(obj, {
  isExtensible(target) {
    // 自动转为布尔值
    return true
  }
})
// 返回值必须与目标对象的isExtensible属性保持一致 否则报错
Object.preventExtensions(obj)
console.log(Object.isExtensible(obj)); // false
console.log(Object.isExtensible(proxy)); // true 报错
```



### 2-11 ownKeys()

`ownKeys(target)`

`ownKeys()`方法用来拦截对象自身属性的读取操作，拦截以下操作

- `Object.getOwnPropertyNames(obj)` 返回非 `Symbol` 键
- `Object.getOwnPropertySymbols(obj)` 返回 `Symbol` 键
- `Object.keys/values()` 返回带有 `enumerable` 标志的非 `Symbol` 键/值
- `for..in` 循环遍历所有带有 `enumerable` 标志的非 `Symbol` 键，以及原型对象的键

使用`Object.keys()`方法时，有三类属性会被 `ownKeys()` 方法自动过滤，不会返回

- 目标对象上不存在的属性
- 属性名为 `Symbol` 值
- 不可遍历`enumerable`的属性

```js
let obj = {
  name: 'jsx',
  [Symbol('title')]: 'javascript',
  message: 'thanks',
  type: true
};
Object.defineProperty(obj, 'type', {
  value: true,
  enumerable: false
})
let proxy = new Proxy(obj, {
  ownKeys(target) {
    // 不存在的属性，属性名为Symbol值，不可遍历enumerable的属性 ownkeys不会返回
    return ['name', 'message', 'asd', Symbol('title')]
  }
})
console.log(Object.keys(obj)); // ['name', 'message']
console.log(Object.keys(proxy)); // ['name', 'message']
```

* `ownKeys()`方法返回的数组成员，只能是字符串或 `Symbol` 值。如果有其他类型的值，或者返回的根本不是数组，就会报错

* 如果目标对象自身包含不可配置的属性，则该属性必须被`ownKeys()`方法返回，否则报错

* 如果目标对象是不可扩展的，这时 `ownKeys()` 方法返回的数组之中，必须包含原对象的所有属性，且不能包含多余的属性，否则报错

```js
let obj1 = {};
Object.defineProperty(obj1, 'info', {
  configurable: false,
  enumerable: true,
  value: 10
})
Object.preventExtensions(obj1)
let proxy1 = new Proxy(obj1, {
  ownKeys: function(target) {
    // 只能是字符串或 `Symbol` 值
    // return [123, true, undefined, null, {}, []];
    // 自身包含不可配置的属性必须被ownKeys()方法返回否则报错
    // return ['info', 'name', Symbol('title')]
    // 如果目标对象是不可扩展的ownKeys()方法返回的数组之中，必须包含原对象的所有属性，且不能包含多余的属性，否则报错
    return ['info']
  }
});
console.log(Object.getOwnPropertyNames(proxy1)); //  ['info', 'name']
```



### 2-12 preventExtensions()

`preventExtensions(target)`

拦截`Object.preventExtensions()` 操作，方法必须返回一个布尔值，否则会被自动转为布尔值

这个方法有一个限制，只有目标对象不可扩展时 `Object.isExtensible(proxy)` 为`false`，`proxy.preventExtensions` 才能返回`true`，否则会报错

```js
// 当目标对象不可扩展时， preventExtensions才能返回true
let obj = {};
let proxy = new Proxy(obj, {
  preventExtensions(target) {
    console.log('不可扩展')
    Object.preventExtensions(target)
    return true
  }
});
console.log(Object.preventExtensions(proxy))
```



### 2-13 setPrototypeOf()

`setPrototypeOf(target, proto)`

* `target` —— 目标对象
* `proto` —— 对象原型属性

主要用来拦截 `Object.setPrototypeOf()` 方法

只要修改 `target` 的原型对象，就会报错

该方法只能返回布尔值，否则会被自动转为布尔值。另外，如果目标对象不可扩展，`setPrototypeOf()` 方法不能改变目标对象的原型

```js
// 主要用来拦截Object.setPrototypeOf()方法
let obj = {};
let proxy = new Proxy(obj, {
  setPrototypeOf(target, proto) {
    console.log(proto); // {title: 'jsx'}
    return true
  }
})
let proto = {
  title: 'jsx'
}
console.log(Object.setPrototypeOf(obj, proto))
console.log(Object.setPrototypeOf(proxy, proto))
```



### 2-14 revocable 取消 proxy

`Proxy.revocable()` 方法返回一个可取消的 `Proxy` 实例

```js
let {proxy, revoke} = Proxy.revocable(target, handler)
```

该调用返回一个带有 `proxy` 和 `revoke` 函数的对象以将其禁用

`revoke` 属性是一个函数，可以取消`Proxy`实例，当执行 `revoke` 函数之后，再访问`Proxy` 实例，就会抛出一个错误

```js
// revocable 取消代理
let title = {
  name: 'jsx'
};
let { proxy, revoke } = Proxy.revocable(title, {
  get(target, prop, value) {
    return target[prop]
  }
})
console.log(proxy.name)
// 取消代理
revoke();
console.log(proxy.name); // Uncaught TypeError
```



## 3. Proxy 中 this 问题

* 在 `Proxy` 代理的情况下，目标对象内部的 `this` 关键字会指向 `Proxy` 代理，如果改变`this`指向，会导致 `Proxy` 无法代理目标对象

```js
// Proxy代理下，目标对象内部this指向代理对象
let title = {
  lesson: 'js',
  func: function() {
    console.log(this)
  }
}
title.func(); // {lesson: 'js', func: ƒ}

let proxy = new Proxy(title, {})
proxy.func(); // Proxy {lesson: 'js', func: ƒ}

// 改变`this指向，会导致 Proxy无法代理目标对象
let title1 = {
  lesson: 'js',
  func: function() {
    console.log(this.class)
  }
}

let title2 = {
  class: 'vue'
}
title1.func.call(title2); // vue

let proxy2 = new Proxy(title1, {})
proxy2.func(); // undefined
```



* `Proxy` 拦截函数内部的 `this`，指向的是 `handler` 对象
* `get()` 和 `set()` 拦截函数内部的 `this`，指向的都是 `handler` 对象

```js


// 拦截器内部this 指向handler对象
let obj = {
  name: 'jsx'
};
let proxy1 = new Proxy(obj, {
  get(target, prop, receiver) {
    console.log(this); // {get: ƒ}
    return target[prop]
  },
  set(target, prop) {
    console.log(this); // {get: ƒ, set: ƒ}
    return true
  }
})
console.log(proxy1.name)
console.log(proxy1.title = 'js')
```
