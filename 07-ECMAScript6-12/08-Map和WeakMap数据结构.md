# Map 和 WeakMap



## 1. Map 创建

`Map` 是一个键值对的数据项的集合，就像一个 `Object` 一样，但是它们最大的差别是 `Map` 允许任何类型的键 `key` 或者值 `value`

由于对象只接受字符串或者 `Symbol` 作为键，`ES6` 提供了 `Map` 数据结构，字符串，各种类型的值包括对象都可以当作键或者值

具有 `Iterator` 接口、且每个元素都是一个双元素的数组的数据结构都可以当作`Map`构造函数的参数

```js
// Map()构造函数参数必须是可迭代对象
let map = new Map([['name', 'jsx']]);
// Map 里的元素可以是任何类型
console.log(map); // {'name' => 'jsx'}
```



## 2. Map 特性

* 如果对同一个键多次赋值，后面的值将覆盖前面的值
* 如果读取一个未知的键，则返回 `undefined`
* 只有对同一个对象的引用，`Map` 结构才将其视为同一个键
* 如果 `Map` 的键是一个简单类型的值，则只要两个值严格相等，`Map` 将其视为一个键，同一个键则后面的值将覆盖前面的值
* 在 `Map` 数据结构中 `NaN` 与 `NaN` 相等的，键的比较是基于 `sameValueZero` 算法类似于 `===` 运算符

```js
// 同一个键多次赋值，后面的键会覆盖前面的
let map1 = new Map();
map1.set('user', 'jsx');
map1.set('user', 'ljj');
console.log(map1); // {'user' => 'ljj'}

// 读取未知的键返回undefined
console.log(map1.get('user1')); // undefined

// 只有对象同一引用才为一个键，简单数据类型严格相等为同一个键
let map2 = new Map();
let key1 = ['use'];
let key2 = ['use'];
map2.set(key1, 'jsx');
map2.set(key2, 'jsx');
console.log(map2); // {Array(1) => 'jsx', Array(1) => 'jsx'}

// 简单数据类型严格相等的键会被覆盖
map2.set('user', 'jsx'); // 被覆盖
map2.set('user', 'ljj');
console.log(map2); // {Array(1) => 'jsx', 'user' => 'ljj'}
```



## 2. Map 属性与方法

* `map.set(key, value)` —— 设置键名 `key` 对应的键值为 `value`，然后返回整个 `Map` 结构，如果 `key` 已经有值，则键值会被更新，否则就新生成该键，可以采用链式写法
* `map.get(key)` —— 根据键来返回值，如果 `map` 中不存在对应的 `key`，则返回 `undefined`
* `map.has(key)` —— 如果 `key` 存在则返回 `true`，否则返回 `false`
* `map.delete(key)` —— 删除指定键的值返回 `true`， 如果删除失败，返回`false`
* `map.clear()` —— 清空 `map`
* `map.size` —— 返回 `Map` 结构的成员总数

```js
// set 设置key 和 value 返回整体解构
// key重复则不会产生新的键值对
let map = new Map();
// 链式写法
map.set('study', 'Map方法')
	.set('code', '学习')
console.log(map); // {'study' => 'Map方法', 'code' => '学习'}

// get 读取对应key值，不存在则为undefined
console.log(map.get('study')); // Map方法

// size 返回元素数量
console.log(map.size); // 2

// has 检测key是否在map中 存在返回true 不存在返回false
console.log(map.has('study')); // true

// delete 删除键 删除成功返回true 失败返回false
console.log(map.delete('code')); // true
console.log(map)

// clear 清空
map.clear();
console.log(map); // Map(0) {size: 0}
```



## 3. Map 迭代

- `Map.keys()`：返回键名的遍历器
- `Map.values()`：返回键值的遍历器
- `Map.entries()`：返回所有成员的遍历器
- `Map.forEach(value, key, map)`：遍历 `Map` 的所有成员

```js
// map迭代
let map = new Map();
map.set('name', 'jsx')
  .set('study', ['html', 'css'])
console.log(map)

for (let item of map.keys()) {
  console.log(item); // name study
}

for (let item of map.values()) {
  console.log(item); // jsx  ['html', 'css']
}

for (let [key, value] of map.entries()) {
  console.log(key, value); //  name jsx  study (2) ['html', 'css']
}

let result = map.forEach((item, key) => {
  console.log(item, key);
  // jsx name
  // ['html', 'css'] 'study'
})
```



## 4. 数据结构转换

* 使用扩展运算符 `...` 将 `Map` 结构转为数组结构
* `Array.form` 静态方法将 `Map` 结构转为数组
* 结合数组的`map`方法、`filter`方法，可以实现 `Map` 的遍历和过滤

```js
// 扩展运算符 转换为素组
let map = new Map();
map.set('name', 'jsx')
  .set('lesson', 'js');
let arrkey = [...map.keys()];
let arrvalue = [...map.values()];
console.log(arrkey); //  ['name', 'lesson']
console.log(arrvalue); // ['jsx', 'js']

// Array,form()转换
let arrentries = Array.from(map.entries())
console.log(arrentries); // [['name', 'jsx'], ['lesson', 'js']]

// 使用map 和filter方法
[...map].map((item) => {
  console.log(item);
  //  ['name', 'jsx']
  // ['lesson', 'js']
})

let result = [...map].filter(([key, value]) => {
  return key === 'name'
})
console.log(result); // ['name', 'jsx']
```



## 4. WeakMap 类型

`WeakMap` 对象是一组键/值对的集合，其中的键是弱引用的，其键必须是对象，而值可以是任意的，正常引用

`WeakMap`的专用场合就是，它的键所对应的对象，可能会在将来消失。`WeakMap`结构有助于防止内存泄漏

```js
// WeakMap 键必须是对象是弱引用的 值可以是任何类型
let weakmap = new WeakMap();
// weakmap.set('name', 'jsx'); // Uncaught TypeError
weakmap.set({dom: 'div'}, 'jsx');
console.log(weakmap); // WeakMap {{…} => 'jsx'}
weakmap.set({dom: 'box'}, 123);
console.log(weakmap); // {{…} => 123, {…} => 'jsx'}
```



## 5. Map 与 WeakMap 区别

* `WeakMap` 只接受对象作为键名 `null` 除外，不接受其他类型的值作为键名
* `WeakMap` 的键名所引用的对象都是弱引用，如果我们在 `weakMap` 中使用一个对象作为键，并且没有其他对这个对象的引用，该对象将会被从内存和 `map` 中自动清除

* `WeakMap` 不支持迭代以及 `keys()`，`values()` 和 `entries()` 方法。所以没有办法获取 `WeakMap` 的所有键或值

```js
// WeakMap只接受对象为键名
let map = new Map([
  ['name', 'jsx'],
  [85, 'ljj']
]);
console.log(map); // {'name' => 'jsx', 85 => 'ljj'}

let weakmap = new WeakMap([
  [{
    'obj': 'name'
  }, '键为对象']
]);
console.log(weakmap); //  {{…} => '键为对象'}

// WeakMap不支持迭代 无法获取key value
for (let item of weakmap) {
  console.log(item)
}
// Uncaught TypeError: weakmap is not iterable
```



## 6. WeakMap 方法与垃圾回收收

`WeakMap`只有四个方法可用：`get()`、`set()`、`has()`、`delete()`

- `WeakMap.get(key)` —— 根据键来返回值，如果 `WeakMap` 中不存在对应的 `key`，则返回 `undefined`
- `WeakMap.set(key, value)` —— 设置键名 `key` 对应的键值为 `value`，然后返回整个 `WeakMap` 结构，如果 `key` 已经有值，则键值会被更新，否则就新生成该键，可以采用链式写法
- `WeakMap.has(key)` —— 如果 `key` 存在则返回 `true`，否则返回 `false`
- `WeakMap.delete(key)` —— 删除指定键的值返回 `true`， 如果删除失败，返回`false`

```js
// get set has delete 方法与Map一样
// set 设置key value ,key必须为对象，值可以为任何类型
let weakmap = new WeakMap();
weakmap.set({
  'name': 'jsx'
}, '任意类型')
console.log(weakmap); // {{…} => '任意类型'}

// get 根据key获取返回值
// {'name': 'jsx'} 不是同一个引用
console.log(weakmap.get({
  'name': 'jsx'
})); // undefined

// has 检测属性是否存在
let obj = {
  'now': '马上'
};
weakmap.set(obj, '字符类型');
console.log(weakmap.has(obj)); // true
console.log(weakmap); // {{…} => '字符类型', {…} => '任意类型'}

// delete 删除键
weakmap.delete(obj);
console.log(weakmap); //{{…} => '任意类型'}
```



`WeakMap` 应用的典型场合就是 DOM 节点作为键名，防止内存泄漏风险

```js
// 结合DOM使用
let weakmap = new WeakMap();
weakmap.set(document.querySelector('.btn'), {
  num: 0
});
console.log(weakmap)

document.querySelector('.btn').addEventListener('click', function() {
  let data = weakmap.get(document.querySelector('.btn'));
  data.num++;
  document.querySelector('h3').innerText = data.num;
}, false)
```



`WeakMap ` 的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，`WeakMap` 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用

```js
let weakmap = new WeakMap();
weakmap.set({
  name: 'jsx'
}, 'hello');
console.log(weakmap); // WeakMap {{…} => 'hello'}

setTimeout(() => {
  console.log(weakmap); // WeakMap {}
}, 1000)
```

