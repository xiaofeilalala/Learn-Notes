# Class类与继承



## 1. Class基础语法

### 1-1 定义类

`ES6` 引入了 `Class` 类构造方式，作为对象的模板，通过`class`关键字，可以定义类

`ES6` 的 `class` 是一个语法糖，新的 `class` 写法只是让对象原型的写法与继承更加清晰简洁

> **Tips：**类方法与方法之间不需要逗号分隔

* 定义类的一种方法是使用类声明，通过 `class` 关键字定义类
* 类表达式是定义类的另一种方法，类表达式可以命名或不命名。命名类表达式的名称是该类体的局部名称，仅在类内部可见

```js
// 第一种方式 通过class关键字定义
class User {
	// class 方法
    constructor(name) {
        this.name = name
    }
}
let user = new User();

// 第二种方式 类表达式形式
let Info = class {
    sayHi() {
        console.log("Hello");
    }
}
```

* `ES6` 的类，完全可以看作构造函数的另一种写法，直接对类使用 `new` 命令，跟构造函数的用法完全一致
* 类的 `prototype` 属性里的的 `constructor()` 属性，直接指向类的本身

```js
// 类是构造函数的另一种写法
function User(title) {
  this.title = title
}
let user = new User('func')
console.log(user.title)

class Message {
  constructor(title) {
    this.title = title
  }
}
let message = new Message('class')
console.log(message.title)
console.log(typeof Message); // function
console.log(Message === Message.prototype.constructor); // true
```



### 1-2 constructor方法

`constructor()` 方法是类的默认方法，通过 `new` 命令生成对象实例时，自动调用该方法，可以在 `constructor()` 中初始化对象，传递对象的初始参数

一个类必须有 `constructor()` 方法，如果没有显式定义，一个空的 `constructor()` 方法会被默认添加

类必须使用 `new` 调用，否则会报错，普通构造函数，不用 `new` 也可以执行

```js
// constructor 是类默认的方法
// 没有定义系统会自动定义
class User {
  constructor() {}
}
// constructor指向实例对象
let user = new User();
console.log(user instanceof User); // true

// 可以用来初始化对象，传递对象初始参数
class Message {
  constructor(name) {
    this.name = name
  }
}
let message = new Message('jsx')
console.log(message.name); // jsx
```



### 1-3 类的实例

* `class` 类的实例属性与方法除非定义在其本身（定义在 `this` 上），否则都是定义在原型上

* `class` 类的所有实例共享一个原型对象

```js
// class的实例属性
class User {
  sayHi() {
    console.log('Hi')
  }
  constructor(name) {
    this.name = name
  }
}
// class 类中定义的属性方法都是定义在原型上
let user = new User('jsx');
// 定义在this上的属性不会在原型上
console.log(user)
user.sayHello = function() {
  console.log('sayHello')
}

// 类创建的所有实例都共享同一个原型对象
let user1 = new User('ljj');
console.log(user1.__proto__ === user.__proto__); // true
console.log(user1.__proto__.constructor === User); // true
```



### 1-4 getter和setter

在类的内部可以使用 `get` 和 `set` 关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为

类的属性名可以是使用表达式用方括号 `[]` 包起来

```js
// getter 和 setter 设置存执取值函数
let method = 'getName'
class User {
  constructor(name) {
    this.name = name
  }
  // 类的属性名可以是使用表达式 通过[]包裹
  [method]() {
    return this.name
  }
  get name() {
    return this._name
  }
  set name(value) {
    this._name = value
  }
}
let user = new User('jsx');
console.log(user.name); // jsx

console.log(user[method]()); // jsx
```



### 1-5 class表达式

类 `class` 也可以使用表达式形式定义

* 命名类表达式 —— 命名的类表达式只能在 `class` 内部使用
* 不命名类表达式 —— 可以省略命名，形成一个没有命名的类
* 类表达式可以立即执行，直接 `new class` 最后传入参数 `(arg)`

```js
// 命名类表达式
let User = class Mine {
  constructor(name) {
    this.name = name
  }
  getInfo() {
    console.log(Mine.name)
    console.log(this.name)
  }
}
let user = new User('jsx')
user.getInfo()

// 不命名类表达式
let Message = class {
  constructor(name) {
    console.log(name)
  }
}
let message = new Message('jsx'); // jsx

// 立即执行类
let Product = new class {
  constructor(name) {
    this.name = name;
  }
  getInfo() {
    console.log(this.name)
  }
}('ljj')
Product.getInfo(); // ljj
```



### 1-6 类的特性

* 类声明和类表达式的主体都执行在严格模式下，严格作用域下函数内部 `this` 为 `undefined`

```js
// 类声明和类表达式的主体都是在严格模式下
'use strict'

function User() {
  console.log(this)
}
User(); // window
// 使用严格模式后 undefined

// 类声明和类表达式内部默认严格模式
class Message {
  show() {
    function hide() {
      console.log(this)
    }
    hide();
  }
}
let message = new Message();
message.show(); // undefined
```

* 函数声明和类声明之间的一个重要区别在于, 函数声明会提升，类声明不会

```js
// 类声明不会提升
// console.log(User) Uncaught ReferenceError
class User {
  constructor() {}
}

// 构造函数声明会变量提升
console.log(Message); // ƒ Message() {}
function Message() {}
```

* `class` 类拥有 `name` 属性

```js
// Class类有name属性
class Info {
  constructor() {}
}
console.log(Info.name); // Info
```

* 如果某个方法之前加上星号 `*`，就表示该方法是一个 `Generator` 函数

```js
class User {
  constructor(...arg) {
      this.arg = arg
    }
    *[Symbol.iterator]() {
      for (let list of this.arg) {
        yield list
      }
    }
}
let user = new User('jsx', 'ljj')
for (let item of user) {
  console.log(item); // jsx ljj
}
```

* 类的内部所有定义的方法，都是不可枚举的

```js
// 类中定义的方法不可枚举
class User {
  constructor() {

  }
  sayHi() {
    console.log(this)
  }
  sayHello() {
    console.log(this)
  }
}
console.dir(User)
console.log(Object.keys(User.prototype)); // []
```



## 2. Class的this指向

### 2-1 类中的this

* 类的方法内部如果有 `this`，默认指向类的实例，静态方法的 `this` 默认指向调用它的对象
* 单独使用内部定义方法，则该方法暴露全局对象中，但是类的内部为严格模式则 `this` 为 `undefined`
* 当调用静态或原型方法时没有指定 `this` 的值，那么方法内的 `this` 值将被置为 `undefined`

```js
class User {
  constructor(name) {
    this.name = name
  }
  sayHi() {
    return this
  }
  static sayHello() {
    return this
  }
}
let user = new User('jsx');
// User类静态方法this指向User类
console.dir(User.sayHello()); // User
// User类方法中的this指向实例对象
console.log(user.sayHi())
// 方法单独使用时
let method = user.sayHi;
console.log(method()); // undefined
// 静态方法单独使用
let method1 = User.sayHello;
console.log(method1()); // undefined
```



### 2-2 改变this指向

* 在构造方法 `constructor` 中通过 `bind` 绑定 `this`

```js
// 在构造方法中通过bind绑定绑定this
class User {
  constructor(name) {
    this.name = name;
    this.sayHi = this.sayHi.bind(this);
  }
  sayHi() {
    return this
  }
  static sayHello() {
    return this
  }
}
let user = new User('jsx');
console.log(user)
let {
  sayHi
} = user;
console.log(sayHi()); // User {name: 'jsx', sayHi: ƒ}
console.dir(User.sayHello())
let methods = user.sayHi
console.dir(methods())

User.sayHello = User.sayHello.bind(this)
console.log(User.sayHello()); // window
```



* 通过箭头函数，箭头函数内部的 `this` 总是指向定义时所在的对象

```js
// 通过箭头函数
class Message {
  constructor() {
    this.sayNo = () => this
  }
}
let message = new Message()
let {
  sayNo
} = message;
console.log(sayNo()); // Message {sayNo: ƒ}
```



* 通过 `Proxy` 代理拦截，改变方法中的 `this`

```js
// proxy代理拦截
class Info {
  constructor() {
    this.name = 'jsx'
  }
  sayBye() {
    return this
  }
}
let info = new Info()
console.log(info.sayBye()) // Info {name: 'jsx'}

let cache = new WeakMap();
let proxy = new Proxy(info, {
  get(target, key) {
    let value = Reflect.get(target, key)
    if (typeof value !== 'function') {
      return value
    }
    if (!cache.has(value)) {
      cache.set(value, value.bind(target))
    }
    return cache.get(value)
  }
})

let result = proxy.sayBye;
console.log(result()); // Info {name: 'jsx'}
```





## 3. 静态属性与方法

### 3-1 静态属性

静态属性指的是 `Class` 本身的属性，而不是定义在实例对象上的属性

在 `class` 中为属性添加 `static` 关键字即声明为静态属性

```js
// 静态属性就是类本身的属性，不会再原型上的属性
class User {
  // 通过 static关键字声明
  static message = '静态属性'
  constructor() {
    // 这里面定义的是实例属性
  }
}
// 通过.点或者[]方括号方式添加类的属性
User.title = '静态属性'
console.dir(User)
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220426193713.png)



### 3-2 静态方法

在 `class` 类中定义的方法都会定义在 `class` 类的 `prototype` 原型属性中，类生成的实例对象会继承原型方法与属性

静态方法就是把一个方法赋值给类的函数本身，而不是赋给它的 `prototype` 原型。这样的方法被称为静态方法 `static`

```js
// 静态方法就是定义在calss类上的方法
class User {
  // static关键字声明的方法为静态方法
  static sayHello() {
    console.log('hello')
  }
  constructor() {}
}

// 通过.添加方法
User.sayHi = function() {
  console.log('静态方法')
}
// 静态方法只能class类访问，实例不能访问
User.sayHi(); // 静态方法
let user = new User();
// user.sayHi(); 报错
User.sayHello(); // hello
// user.sayHello() // hello
```

* 静态方法中的 `this` 指向的是类，不是指向的实例对象
* 静态方法可以和非静态方法重名
* 父类的静态方法可以被子类继承
* 静态方法也是可以从 `super` 对象上调用

```js
class User {
  static saythis() {
    console.log(this)
  }
  saythis() {
    console.log(this)
  }
}
let user = new User();
// 静态方法 this指向class类
User.saythis(); // User
// 静态与动态方法与属性可以重名
user.saythis(); // User{}

 // 子类继承父类的静态方法
 class Foo {
   static classMethod() {
     console.log('hello')
   }
 }
 class Bar extends Foo {}
 Bar.classMethod() // hello
```



## 3. 实例属性与方法

### 3-1 实例属性

* 定义在 `constructor()` 方法里面的 `this` 上面的属性是实例属性

* 实例属性也可以定义在类的最顶层

```js
// 实例属性就是class类生成的实例对象属性
// 通过constructor内this定义
class User {
  constructor(name) {
    this.name = name
  }
}
let user = new User('jsx');
console.log(user.name); // jsx

// 直接定义在class内部最顶层
class Message {
  title = '实例属性'
}
let message = new Message();
console.log(message.title); // 实例属性
```



### 3-2 实例方法

定义在 `constructor()` 方法里面的 `this` 上面的方法是实例方法

由于类的所有实例共享一个原型对象，所以需要将实例方法设置在类的原型上

```js
class User {
  constructor() {
    this.name = 'jsx'
    this.asyHi = function() {
      console.log('Hi')
    }
  }
}
let user = new User();
console.log(user); // User {name: 'jsx', asyHi: ƒ}
```



## 4. 私有和受保护的属性与方法

### 4-1 命名规则保护

在类的内部与外部都可以访问到的被称为不受保护的属性

私有方法和私有属性，是只能在类的内部访问的方法和属性，外部不能访问

* 受保护的属性通常以下划线 `_` 作为前缀，告诉开发者者这是一个私有属性，请不要在外部使用
* 添加一个 `setter` 访问器，当外部修改 `_` 定义私有属性时给予提示

```js
class User {
  // 私有属性
  _title = 'title'
  // 私有方法
  _sayHi() {
    console.log('Hi')
  }
  set title(val) {
    throw new Error('私有属性不可修改')
  }
  // 内部使用
  getTitle() {
    console.log(this._title)
  }
}
let user = new User()
console.log(user.title); // 不可访问
// user.title = '修改'; // Uncaught Error: 私有属性不可修改
user.getTitle(); // title
```



### 4-2 Symbol定义

利用 `Symbol` 值的唯一性，将私有方法的名字命名为一个 `Symbol` 值

```js
let symbol = Symbol();
class User {
  constructor() {
    this[symbol] = {}
    this[symbol].name = 'jsx'
  }
  set name(val) {
    throw new Error('私有属性不可修改')
  }
  // 内部使用
  getSymbol() {
    console.log(this[symbol].name)
  }
}
let user = new User();
console.log(user.name); // undefined
// user.name = 'ljj'; // Uncaught Error: 私有属性不可修改
user.getSymbol(); // jsx
```



### 4-3 使用WeakMap保护

`WeakMap` 是一组键/值对的集，通过使用 `WeakMap` 类型特性定义私有属性

```js
let name = new WeakMap();
class User {
  constructor() {
    name.set(this, 'jsx')
  }
  set name(val) {
    throw new Error('私有属性不可修改')
  }
  // 内部使用
  getName() {
    console.log(name.get(this))
  }
}
console.log(name)
let user = new User();
console.log(user.name); // undefined
// user.name = 'ljj'; // Uncaught Error: 私有属性不可修改
user.getName(); // jsx
```



### 4-4 私有属性与方法 #

通过在 `class` 内为属性或方法名前加 `#` 声明为私有属性与私有方法

私有属性与私有方法只能在声明的类中使用，在外部使用报错

* 私有属性不限于从 `this` 引用，只要是在类的内部，实例也可以引用私有属性

* 私有属性和私有方法前面加上 `static` 关键字表示这是一个静态的私有属性或私有方法

```js
class User {
  #title = 'jsx'
  getTitle() {
    // 只能在内部使用
    console.log(this.#title)
  }
  // 私有方法
  #sayHi() {
    return 'ljj'
  }
  // 内部使用私有方法
  getSayHi() {
    console.log(this.#sayHi())
  }
  // 私有静态属性
  static #message = '私有静态属性'
  // 私有静态方法
  static #getMessage() {
    return '私有静态方法'
  }
  static getAll() {
    console.log(this.#message + ' ' + this.#getMessage())
  }
}
let user = new User();
console.log(user)
// console.log(user.#title); 报错
user.getTitle(); // jsx
// user.#sayHi(); 报错
user.getSayHi(); // ljj
// User.#message 报错
// User.getMessage(); 报错
User.getAll(); // 私有静态属性 私有静态方法
```



## 5. 类继承

### 5-1 extends关键字

`Class` 可以通过 `extends` 关键字在类声明或类表达式中用于创建一个类作为另一个类的一个子类，并实现继承，让子类继承父类的属性和方法

除了私有属性，父类的所有属性和方法，都会被子类继承，其中包括静态方法，私有属性只能在定义它的 `class` 里面使用

```js
class User {
  title = 'extends'
  sayHi() {
    console.log('Hi')
  }
  static message = 'hello'
  static sayHello() {
    console.log('hello')
  }
  #value = '私有'
}

class Info extends User {
  constructor() {
    super()
  }
}
let info = new Info();
console.dir(info)
console.log(info.title)
console.log(Info.message)
info.sayHi()
Info.sayHello()
// console.log(info.#value) 报错
```



### 5-2 super关键字

`super` 关键字，既可以当作函数使用，也可以当作对象使用

* `super` 作为函数调用时，代表父类的 `constructor` 构造函数，只能用在子类的 `constructor` 构造函数之中，或者对象的方法之中
* `super` 作为对象时，在子类的普通方法中，指向父类的原型对象，在子类的静态方法中，指向父类

```js
// super关键字可作为对象或者函数使用
// 作为函数调用时super()只能在子类的或者对象方法中
class User {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log('Hello')
  }
  static sayHi() {
    console.log('Hi')
  }
}

class Message extends User {
  constructor(name) {
    // 子类super代表父类的constructor方法
    super(name);
  }
  childMethod() {
    // 作为对象时，在子类方法中指向父类的原型对象
    super.sayHello()
  }
  // 在子类静态方法中，super指向父类
  static staticMethod() {
    super.sayHi()
  }
}
let message = new Message('jsx');
console.log(message); // Message {name: 'jsx'}
message.childMethod(); // Hello
Message.staticMethod(); // Hi
```



### 5-3 super使用

* `ES6` 规定，子类必须在 `constructor()` 方法中调用 `super()`，否则就会报错
* `ES6` 的继承机制，则是先将父类的属性和方法，加到一个空的对象上面，然后再将该对象作为子类的实例，即继承在前，实例在后
* 在子类的 `constructor()` 方法中，只有调用 `super()` 之后，才可以使用 `this` 关键字，否则会报错
* 如果子类没有定义 `constructor()` 方法，这个方法会默认添加，并且里面会调用`super()`
* 子类方法与父类方法同名时，使用子类方法

```js
// super必须在constructor中调用
class User {
  constructor() {
    this.title = 'jsx'
  }
  sayHi() {
    console.log('Hi')
  }
}

class Message extends User {
  constructor() {
    // console.log(this) 需要等先执行super()后才能使用this
    super()
    console.log(this); // 指向当前子类的实例
  }
  sayHi() {
    console.log('message Hi')
  }
}
// let message = new Message(); // 不调用则会报错
let message = new Message();
console.log(message)
// 子类会继承父类的所有属性与方法，除了私有
message.sayHi(); // Hi

// 方法重写后 先使用自身方法如果没有则找父类
message.sayHi(); // message Hi
```



### 5-4 super指向

* `super` 作为对象时，在子类的普通方法中，由于 `super` 指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过 `super` 调用的
* 在子类普通方法中通过 `super` 调用父类的方法时，方法内部的 `this` 指向当前的子类实例
* 如果 `super` 作为对象，用在静态方法之中，这时 `super` 将指向父类，而不是父类的原型对象
* 在子类的静态方法中通过 `super` 调用父类的方法时，方法内部的 `this` 指向当前的子类，而不是子类的实例

```js
// 普通方法中的super指向父类原型
class User {
  constructor() {
    this.name = 'jsx'
  }
  sayHi() {
    console.log('sayHi')
  }
  sayNo() {
    console.log(this)
  }
  static sayGood() {
    console.log('父类自身方法')
    console.log(this)
  }
}

class Message extends User {
  constructor() {
    super()
  }
  sayHello() {
    console.log(this)
    super.sayHi(); // sayHi
    // super调用父类原型方法时，内部的this指向子类实例
    super.sayNo(); // Message {name: 'jsx'}
  }
  static sayBack() {
    // 在static中的super表示父类
    // 内部this指向子类
    super.sayGood()
  }
}
let message = new Message();
console.log(message)
message.sayHello()
Message.sayGood(); // 父类自身方法
```



### 5-5 类与实例的原型

在 `ES5` 中，每一个对象都有 `__proto__` 属性，指向对应的构造函数的 `prototype` 属性

`Class` 作为构造函数的语法糖，同时有 `prototype` 属性和 `__proto__` 属性，因此同时存在两条继承链

* 子类的 `__proto__` 属性，表示构造函数的继承，总是指向父类
* 子类 `prototype` 属性的 `__proto__` 属性，表示方法的继承，总是指向父类的 `prototype` 属性
* 子类实例的 `__proto__` 属性的 `__proto__` 属性，指向父类实例的 `__proto__` 属性，子类的原型的原型，是父类的原型

```js
// ES5中的原型
function User() {}
let user = new User();
console.log(user.__proto__ === User.prototype); // true

// ES6的原型
class Message {
  constructor() {}
}
console.dir(Message); // 同时拥有__proto__和prototype
let message = new Message();
console.log(message.__proto__ === Message.prototype)

class Info extends Message {
  constructor() {
    super();
  }
}
let info = new Info();
// 构造函数的继承
console.log(Info.__proto__ === Message); // true
// 子类的原型的原型是父类的原型
console.log(Info.prototype === info.__proto__); // true
console.log(info.__proto__.__proto__ === message.__proto__); // true
```



### 5-6 扩展内置类

`ES6` 允许继承原生构造函数定义子类，可以使用 `extends` 来扩展原生的类，也就是继承内置的构造函数

`ECMAScript` 的原生构造函数：`Boolean()`、`Number()`、`String()`、`Array()`、`Date()`、`Function()`、`RegExp()`、`Error()`、`Object()`

* 当扩展原生的类时，不会继承静态方法
* 继承 `Object` 原生类时，如果 `Object` 方法不是通过 `new Object()` 这种形式调用，ES6 规定 `Object` 构造函数会忽略参数

```js
// ES6 可以继承原生的内置内
// 只能继承原生构造函数的实例方法与属性
// 不能继承静态与实例属性方法
let arr = new Array();
class Inherit extends Array {
  // 继承原生的数组构造函数
  // 只能继承实例属性与方法
  constructor(...arg) {
    super(...arg)
    console.log(arg)
  }
}
let inherit = new Inherit('jsx', 'ljj', 'ddc', 'zdj');
console.log(inherit);
// 子类的实例对象的原型的原型指向父类的实例对象的原型
console.log(inherit.__proto__.__proto__ === arr.__proto__); // true
// 使用继承的方法
inherit.map(item => {
  console.log(item); // jsx ljj ddc zdj
})
```



### 5-7 mixin混入继承

`JS` 不能实现多继承，如果要使用多个类的方法时可以使用 `mixin` 混合模式来完成

* 一个类继承多个类的属性与方法，首先的继承每个类的实例对象的属性与方法，和类的静态属性与方法，以及类的实例对象原型上的属性与方法
* 定义一个 `copyProperties` 方法，用来复制类的静态属性与原型属性以及实例属性
* 通过 `assignMixin` 方法定义一个中间类，将多个类的数组循环，并传入 `arg` 实例对象的属性，然后类会通过 `copyProperties` 方法获取所有属性与方法，然后返回中间类

```js
function assignMixin(...mixins) {
  // 设置一个类用来继承类集合
  class Mixin {
    // 传入实例属性与方法
    constructor(...arg) {
      mixins.forEach((mixin, index) => {
        copyProperties(this, new mixin(arg[index])); // 拷贝实例属性
      })
    }
  }

  for (let mixin of mixins) {
    // 拷贝类集合的静态属性与原型属性
    copyProperties(Mixin, mixin); // 拷贝静态属性
    copyProperties(Mixin.prototype, mixin.prototype); // 拷贝原型属性
  }

  return Mixin;
}

function copyProperties(target, source) {
  // target为Mixin类的实例对象 source时集合类的实例对象
  Reflect.ownKeys(source)
    .forEach(key => {
      // 迭代集合类的实例对象属性与方法
      // 判断只继承自定义的属性
      if (!/^(?:initializer|constructor|prototype|arguments|caller|name|bind|call|apply|toString|length)$/.test(key)) {
        // 将自定义的属性与方法设置给target类
        return Reflect.defineProperty(target, key, Reflect.getOwnPropertyDescriptor(source, key));
      }
    })
}

class Hello {
  constructor(info) {
    this.info = info;
  }
  sayHello() {
    console.log('Hello')
  }
}

class Welcome {
  constructor(title) {
    this.title = title
  }
  sayWelcome() {
    console.log('Welcome')
  }
}

class Thanks {
  constructor(message) {
    this.message = message
  }
  sayThanks() {
    console.log('Thanks')
  }
}

class Back extends assignMixin(Hello, Welcome, Thanks) {
  constructor(...arg) {
    super(...arg)
  }
}
let back = new Back('jsx', 'ljj', 'ddc');
console.log(back)
console.log(back.info) // jsx
console.log(back.title) // ljj
console.log(back.message) // ddc
back.sayHello() // Hello
back.sayWelcome() // Welcome
back.sayThanks(); // Thanks
```
