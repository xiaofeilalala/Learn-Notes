# Set 和 WeakSet



## 1. Set 创建

`Set` 是一个特殊的类型集合 —— 值的集合没有键，它的每一个值只能出现一次

`new Set(iterable)` 可以接受一个数组或者具有 `iterable` 接口的其他数据结构作为参数，用来初始化

```js
// 声明
let set = new Set();

// 提供数组对象，则会复制数组元素以并去重
let set = new Set([1, 2, 5, 5, 5])
console.log(set); // Set(3) {1, 2, 5}

// 去除字符串里面的重复字符
console.log([...new Set('hello')].join('')); // helo
```

* `Set` 中的元素是严格类型约束的，通过 `===` 判断元素类型是否相等，如果 `Set` 中含有此元素则不会添加进去

* 在 `Set` 中 `NaN` 等于自身 `NaN === NaN`

```js
// Set中数据是严格相等的，NaN全等于NaN
let set2 = new Set(['1', 1, true, undefined, null]);
console.log(set2); // {'1', 1, true, undefined, null}

console.log(set2.add('1')); // {'1', 1, true, undefined, null}
console.log(set2.add(NaN)); // {'1', 1, true, undefined, null, NaN}
console.log(set2.add(NaN)); // {'1', 1, true, undefined, null, NaN}
```



## 2. Set 属性与方法

* `set.add(value)`  —— 添加一个值，返回 `set` 本身
* `set.delete(value)` —— 删除值，如果 `value` 在这个方法调用的时候存在则返回 `true` ，否则返回 `false`

- `set.has(value)` —— 如果 `value` 在 `set` 中，返回 `true`，否则返回 `false`
- `set.clear()` —— 清空 `set`
- `set.size` —— 返回元素个数

```js
// add 添加set值
let set = new Set();
set.add(1);
set.add('1');
console.log(set); // {1, '1'}

// delete 删除元素 删除元素存在返回true 不存在为false
let set1 = new Set([true, false]);
console.log(set1.delete(true)); // true
set1.delete(true);
console.log(set1); // {false}

// size 获取元素数量
let set2 = new Set([1, '2', true]);
console.log(set2.size); // 3

// has 检测元素是否在set中
let set3 = new Set([1, '3', true]);
console.log(set3.has('1')); // false

// clear 清空set
let set4 = new Set([1, '1', true]);
set4.clear();
console.log(set4); // {size: 0}
```

* 数组转换
* 数组去重
* 交集、并集、差集

```js
// 数组去重
let arr = [1, 2, 'jsx', 'jsx', 3, 4];
console.log([...new Set(arr)]); // [1, 2, 'jsx', 3, 4]

// 数组转换
let setover = new Set(['jsx', 22, 'js']);
console.log([...setover]); // ['jsx', 22, 'js']
console.log(Array.from(setover)); // ['jsx', 22, 'js']

// 交集 共同元素
let seta = new Set(['jsx', 'ljj']);
let setb = new Set(['ljj', 'ddc']);
let setc = new Set([...seta].filter(item => {
	return setb.has(item);
}))
console.log(setc); //  {'ljj'}

// 差集 seta1 相对于 setb1 的差集
let seta1 = new Set(['jsx', 'ljj']);
let setb1 = new Set(['ljj', 'ddc']);
let setc1 = new Set([...seta1].filter(item => {
	return !setb1.has(item);
}))
console.log(setc1); //  {'jsx'}

// 并集 合并 set会去重
console.log([...seta, ...setb]);
// ['jsx', 'ljj', 'ljj', 'ddc']
```



## 3. Set 迭代

`keys`方法、`values`方法、`entries`方法返回的都是遍历器对象，由于 `Set` 结构没有键名，只有键值，所以 `keys` 方法和 `values` 方法的行为完全一致

* `set.keys()` —— 遍历并返回所有的值
* `set.values()` —— 与 `set.keys()` 作用相同，这是为了兼容 `Map`
* `set.entries()` —— 遍历并返回所有的实体 `[value, value]`，它的存在也是为了兼容 `Map`
* `...` 扩展运算符，`for...of`，`forEach` 可以用于 `Set` 结构遍历

```js
let set = new Set(['jsx', 'ljj']);
console.log(set.keys()); // {'jsx', 'ljj'}
console.log(set.values()); // {'jsx', 'ljj'}
console.log(set.entries()); // {'jsx' => 'jsx', 'ljj' => 'ljj'}

// for of循环
for (let item of set) {
	console.log(item); // jsx ljj
}

// forEach迭代
set.forEach((item) => {
	console.log(item); // jsx ljj
})

// ...扩展
let newset = [...set].map(item => {
	console.log(item); // // jsx ljj
})
```



## 4. WeakSet 类型

`WeakSet` 是一个构造函数，可以使用 `new` 命令，创建 `WeakSet` 数据结构

作为构造函数，`WeakSet` 可以接受一个数组或类似数组的对象（任何具有 `Iterable` 接口的对象）作为参数，`WeakSet` 元素只能是对象类型

```js
// 声明
let weakset = new WeakSet();

let weakset = new WeakSet('jsx'); // Uncaught TypeError
let weakset = new WeakSet(['jsx', 'ljj']); // Uncaught TypeError

// // 初始化时会复制数组的元素，数组元素为数组对象不报错
let weakset = new WeakSet([['jsx', 'ljj']]);
console.log(weakset); // {Array(2)}
```



## 5. WeakSet 与 Set 区别

* 与`Set`相比，`WeakSet` 只能是对象的集合，而不能是任何类型的任意值
* `WeakSet` 集合中对象的引用为弱引用，如果没有其他的对 `WeakSet` 中对象的引用，那么这些对象会被当成垃圾回收掉
* `WeakSet` 中的对象都是弱引用，没有 `keys()`，`values()`，`entries()` 等方法和 `size` 属性，并且不可迭代无法获取所有当前内容

```js
let set = new Set([{name: 'jsx'}, 1, true]);
console.log(set);

let weakset = new WeakSet([
	['jsx', 'css']
]);
console.log(weakset); // WeakSet {Array(2)}

// WeakSet没有size属性 不可遍历
weakset.size(); // Uncaught TypeError
for(let item of weakset) {
     console.log(item); // Uncaught TypeError
}
```



## 6. WeakSet方法与垃圾回收

`WeakSet` 只支持 `add`，`has` 和 `delete` 方法

- `WeakSet.add(value)`：向 `WeakSet` 实例添加一个新元素
- `WeakSet.delete(value)`：清除 `WeakSet` 实例的指定元素
- `WeakSet.has(value)`：返回一个布尔值，表示某个值是否在

```js
// WeakSet add has delete方法
const weakset = new WeakSet();
const arr = ["hdcms"];
//添加操作
weakset.add(arr);
console.log(weakset.has(arr)); // true

//删除操作
weakset.delete(arr);

//检索判断
console.log(weakset.has(arr)); // false
```



`WeakSet` 中的对象都是弱引用，如果其他对象都不再引用 `WeakSet` 集合中的对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 `WeakSet` 之中

通过定时器来查看，当 `WeakSet` 里的对象无引用时，则会被浏览器垃圾回收

```js
const weakset = new WeakSet([['jsx']]);
console.log(weakset);

setTimeout(() => {
	console.log(weakset); // WeakSet {}
}, 100);
```

