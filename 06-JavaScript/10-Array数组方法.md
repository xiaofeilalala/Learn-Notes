# Array数组方法

改变原数组的方法：`fill()`、`pop()`、`push()`、`shift()`、`splice()`、`unshift()`、`reverse()`、`sort()`；

不改变原数组的方法：`concat()`、`every()`、`filter()`、`find()`、`findIndex()`、`forEach()`、`indexOf()`、`join()`、`lastIndexOf()`、`map()`、`reduce()`、`reduceRight()`、`slice()`、`some()`



## 1. 添加删除元素

### 1-1 `splice()`

`arr.splice(start, deleteCount, elem1, .., elemN)`

`splice()` 方法可能是数组中的最强大的方法之一了，使用它的形式有很多种，它会向/从数组中添加/删除项目，然后返回被删除的项目。该方法会改变原始数组

**返回值**：由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组

* `start`: 必需。整数，规定添加/删除项目的位置
  * 使用负数从数组末位开始的第几位开始
  * 超出了数组的长度数组从末尾开始添加内容
* `deleteCount`：可选。要移除的数组元素的个数
  * 如果 `deleteCount` 是 0 或者负数，则不移除元素
  * 如果 `deleteCount` 大于 `start` 之后的元素的总数，则从 `start` 后面的元素都将被删除**（包含第 `start` 位）**
  * 如果 `deleteCount` 被省略或大于或者等于 `start `之后的所有元素的数量，那么`start`之后数组的所有元素都会被删除**（包含第 `start` 位）**
* `elem1, .., elemN`：可选。要添加进数组的元素，从`start` 位置开始。如果不指定，则 `splice()` 将只删除数组元素

```js
// splice() 改变原始数组 可以添加删除元素
// arr.splice(start, deleteCount, elem1, .., elemN)
let arr = ['jsx', 'ljj'];
arr.splice(0, 1, 'jsx', '520');
console.log(arr); // ['jsx', '520', 'ljj']

// 参数start必须 规定添加删除元素的位置
// 负数从数组末尾第几位开始，超出数组长度从末未开始
let arr1 = ['jsx', 'ljj', 'ddc'];
// 由被删除的元素组成的一个数组
console.log(arr1.splice(-1, 1)); // ['ddc']
console.log(arr1); // ['jsx', 'ljj']

// 参数deleteCount必须 要删除的项目数量
// 参数为0或负数，不移除元素
let arr2 = ['html', 'css', 'js'];
arr2.splice(0, 0); // 不移除元素
arr2.splice(0, -2); // 不移除元素
console.log(arr2); // ['html', 'css', 'js']

// 参数大于开始索引后元素数量总和，删除包含start索引后的所有元素
let arr3 = ['html', 'css', 'js'];
arr3.splice(1, 2);
console.log(arr3); // ['html']

// 如果 deleteCount被省略了从start位开始数组的所有元素都会被删除
let arr4 = ['html', 'css', 'js'];
arr4.splice(1);
console.log(arr4); // ['html']

// elem1, .., elemN 要添加进数组的元素
let arr5 = ['html', 'css', 'js'];
arr5.splice(3, 0, 'vue', 'react');
console.log(arr5); // ['html', 'css', 'js', 'vue', 'react']
```

从上面参数可知，splice主要有三种使用形式：

- **删除：** 需要给 `splice()` 传递两个参数，即要删除的第一个元素的位置和要删除的元素的数量

- **插入：** 需要给 `splice()` 传递至少三个参数，即开始位置、0（要删除的元素数量）、要插入的元素

- **替换：** `splice()` 方法可以在删除元素的同事在指定位置插入新的元素。同样需要传入至少三个参数，即开始位置、要删除的元素数量、要插入的元素。要插

  入的元素数量是任意的，不一定和删除的元素数量相等

```js
// 删除元素 指定start,deleteCount两个参数
let arr = ['html', 'css', 'js', 'vue'];
arr.splice(0, 2);
console.log(arr); // ['js', 'vue']

// 替换 指定start,deleteCount,elemN三个参数
let arr1 = ['html', 'css', 'js'];
arr1.splice(0, 3, 'jsx', 'ljj');
console.log(arr1); // ['jsx', 'ljj']

// 插入元素 通过deleteCount参数为0或者负数
let arr2 = ['html', 'css', 'js'];
arr2.splice(arr2.length, -1, 'webpack');
// arr2.splice(arr2.length, 0, 'webpack');
console.log(arr2); // ['html', 'css', 'js', 'webpack']
```



### 1-2 `slice()`

`arr.slice(start, end)`

`slice()` 方法可从已有的数组中返回选定的元素。返回一个新的数组，包含从 `start` 到 `end` （不包括该元素）的数组元素。原始数组不会被改变

**返回值：**选定的元素组成的新数组

* `beigin`：必需。规定从何处开始选取
  * 如果是负数，那么它规定从数组尾部开始算起的位置
* `end`：可选。在该索引处结束提取原数组元素。
  * 如果指定该 `end` 参数，提取原数组中索引从 `begin` 到 `end` 的所有元素**（包含 `begin`，但不包含 `end`）**
  * 如果 `end` 参数是负数，那么它规定的是从数组尾部开始算起的元素
  * 如果 `end` 被省略或者大于数组的长度，则 `slice` 会一直提取到原数组末尾

```js
// slice(begin, end) 返回从begin到end 不包括end的新数组
let arr = ['html', 'css', 'js'];
console.log(arr.slice(0, 2)); // ['html', 'css']
console.log(arr.slice(-3, -1)); //  ['html', 'css']
console.log(arr.slice(0)); // ['html', 'css', 'js']
console.log(arr.slice(0, -1)); // ['html', 'css', 'js']
```





## 2. 搜索元素

`ECMAScript` 提供了两类搜索数组的方法：按照严格相等搜索和按照断言函数搜索

* 严格相等：`indexOf()`、`lastIndexOf()`、`includes()`
* 断言函数：`find()`、`findIndex()`, `filter()`



### 2-1 严格相等

`indexOf(item, from)` 从索引 `from` 开始搜索 `item`，如果找到则返回索引，否则返回 `-1`**（从左向右搜索）**

`lastIndexOf(item, from)` 和 `idnexOf()` 相同，**（从右向左搜索）**

`includes(item, from)` 从索引 `from` 开始搜索 `item`，如果找到则返回 `true`，如果没找到，则返回 `false`

* `item`：要查找的元素
* `from`：开始查找的位置
  * 如果该索引值大于或等于数组长度，则表示不在数组
  * 如果索引值为负数，则从末尾开始查找，-1 表示从最后一个元素开始查找，-2 表示从倒数第二个元素开始查找

```js
// 数组搜索方法：严格搜索相等
let arr = ['html', 'css', 'js', 'vue'];
console.log(arr.indexOf('webpack', 0)); // -1
console.log(arr.indexOf('vue', -1)); // 3
console.log(arr.lastIndexOf('vue', -2)); // -1
console.log(arr.includes('vue', 0)); // true
```



### 2-2 断言函数

`arr.find(callback(item, index, array){}, thisArg)`

如果它返回 `true`，则搜索停止，并返回 `item`。如果没有搜索到，则返回 `undefined`（**不会修改所调用的数组**）

`find()` 返回的是第一个匹配的元素，如果没有符合条件的元素返回 `undefined`

```js
let arr = [1, 45, 30];
let result = arr.find(function(item, index, arr) {
	console.log(item);
	console.log(index);
	console.log(arr);
	return item > 20
})

console.log(result); // 45
```



`arr.findIndex(callback(item, index, array){}, thisArg)`

返回找到元素的索引，而不是元素本身。如果没有搜索到任何内容时返回 `-1`（**不会修改所调用的数组**）

`findIndex()` 返回的是第一个匹配的元素的索引，如果没有符合条件的元素返回  `-1`

* `callback`：数组中的每个元素, 都会执行该回调函数，执行时会自动传入下面三个参数
  * `item`：当前元素
  * `index`：当前元素的索引
  * `array`：调用`find()` 或 `findIndex`的数组

* `thisArg`：执行 `callback` 回调时作为 `this` 对象的值，如果没有被提供，将会使用 `undefined`

```js
let arr1 = [2, 59, 18];
let result1 = arr1.findIndex(function(item, index, arr) {
	return item > 20
})
console.log(result1); // 1

let obj = [{
		id: 1,
		name: "John"
	},
	{
		id: 2,
		name: "Pete"
	},
	{
		id: 3,
		name: "Mary"
	}
]
let res = obj.find((item, index) => this.id = 3)
console.log(res.id, res.name); // 1 'John'
```





### 2-3 `filter()`

`arr.filter(callback(item, index, arr), thisArg)`

用于过滤数组，满足条件的元素会被返回。它的参数是一个回调函数，所有数组元素依次执行该函数，返回结果为 `true` 的元素会被返回

该方法会返回一个新的数组，不会改变原数组

可以使用 `filter()` 方法来移除数组中的`undefined`、`null`、`NaN`  等值

```js
let arr = ['jsx', null, undefined, 520, 'ljj'];
let result = arr.filter(Boolean);
console.log(result); //  ['jsx', 520, 'ljj']
let res = arr.filter(function(item, index, arr) {
	console.log(item);
	if (String(item))
		return item;
})
console.log(res); // ['jsx', 520, 'ljj']
```



## 3. 合并拆分转换

### 3-1 `split()`

`str.split(separator, length)`

通过给定的分隔符将字符串分割成一个数组

* `length` 对数组长度的限制，如果使用参数，那么超出长度限制的元素会忽略
* 带有空参数的 `split()`，会将字符串拆分为字母数组

```js
// split() 通过分隔符将字符串分隔为一个数组
let str = 'javascript';

// 空参数会将字符串拆分为单个字母
console.log(str.split('')); // ['j', 'a', 'v', 'a', 's', 'c', 'r', 'i', 'p', 't']

let str1 = 'jsx, ljj, ddc, zdj';
console.log(str1.split(',')); // ['jsx', ' ljj', ' ddc', ' zdj']

// 第二个参数控制数组长度，超出设置的长度会忽略
console.log(str1.split(',', 2)); // ['jsx', ' ljj']
```



### 3-2 `join()`

`arr.join(separator)`

把数组中的所有元素放入一个字符串，元素是通过指定的分隔符进行分隔的，该方法返回一个字符串

* `separator`：可选。用来指定要使用的分隔符，如果省略该参数，则使用逗号作为分隔符

```js
let arr = ['jsx', '520', 'ljj'];
let arr1 = arr.join('');
console.log(arr1); // jsx520ljj

arr1 = arr.join('-')
console.log(arr1); // jsx-520-ljj

// 如果省略该参数，则使用逗号作为分隔符
arr1 = arr.join()
console.log(arr1); // jsx,520,ljj
```



### 3-3 `concat()`

`arr.concat(arg1, arg2.., argN)`

用于连接两个或多个数组。该方法不会改变现有的数组，而会创建一个新数组

* `arg1, arg2...`：可选，可以是具体的值，也可以是数组对象。可以是任意多个
* 如果省略了所有 `agrgN` 参数，则 `concat` 会返回调用此方法的现存数组的一个浅拷贝

```js
let arr = ['jsx', 'ljj'];
let arr1 = ['ddc', 'zdj'];
let arr2 = arr.concat(arr1, arr);
// 不会改变元素数组
console.log(arr); // ['jsx', 'ljj']
console.log(arr2); //  ['jsx', 'ljj', 'ddc', 'zdj','jsx', 'ljj']
let arr3 = arr.concat();
console.log(arr3); // ['jsx', 'ljj']
```



### 3-4 `toString()`

`arr.toString()`

`toString` 方法连接数组并返回一个字符串，其中包含用逗号分隔的每个数组元素

```js
let arr = ['jsx', 'ljj', 'ddc', 'zdj'];
console.log(arr.toString()); // jsx,ljj,ddc,zdj
```



## 4. 数组排序

### 4-1 `reverse()`

`arr.reverse()`

用于颠倒数组中元素的顺序。该方法会改变原来的数组，而不会创建新的数组

```js
let arr = ['jsx', 'ljj', 1, 2];
let arr1 = arr.reverse();
console.log(arr === arr1); // true
console.log(arr1); // [2, 1, 'ljj', 'jsx']
```



### 4-2 `sort()`

`arr.sort(fn(a, b))`

对数组进行原位排序，更改元素的顺序，该方法会在原数组上进行排序，会改变原数组

`sort()` 方法在没有参数使用时，会在每一个元素上调用 `String` 类型转换函数，然后通过比较各个字符的 `Unicode` 位点来决定顺序，即使数组的元素都是数值，也会将数组元素先转化为字符串在进行比较、排序

* `fn` 可选。用来规定排序顺序，它是一个比较函数，用来判断哪个值应该排在前面
  * `a < b` 返回 -1   a 会被排列到 b 之前
  * `a = b` 返回 0   a 和 b 的相对位置不变
  * `a > b` 返回 1  b 会被排列到 a 之前
* 省略参数，元素按照类型转换后的字符串的各个字符的 `Unicode` 位点进行排序

```js
let arr = [1, 2, 'jsx', 3];
console.log(arr.sort()); //  [1, 2, 3, 'jsx']
console.log('1'.charCodeAt()); // 49
console.log('2'.charCodeAt()); // 50
console.log('3'.charCodeAt()); // 51
console.log('jsx'.charCodeAt()); // 106

let array = [0, 1, 5, 10, 15];
// 升序排序
let array2 = array.sort((a, b) => a - b); 
console.log(array2) // [0, 1, 5, 10, 15]

// 降序排序
let array3 = array.sort((a, b) => b - a); // 降序排序
console.log(array3) // [15, 10, 5, 1, 0]
```



## 5. 复制与填充

### 5-1 `copyWithin()`

`arr.copyWithin(target, start, end)`

会按照指定范围来浅复制数组中的部分内容，然后将它插入到指定索引开始的位置

* `target`：必需。复制到指定目标索引位置
  * 如果是负数，`target` 将从末尾开始计算
  * 如果 `target` 大于等于 `arr.length`，将会不发生拷贝
  * 如果 `target` 在 `start` 之后，复制的序列将被修改以符合 `arr.length`
* `start`：开始复制元素的起始位置
  * 如果是负数，`start` 将从末尾开始计算
  * 如果 `start` 被忽略，`copyWithin` 将会从0开始复制
* `end`：开始复制元素的结束位置，`copyWithin` 将会拷贝到该位置，但不包括 `end` 这个位置的元素
  * 如果是负数， `end` 将从末尾开始计算
  * 如果 `end` 被忽略，`copyWithin` 方法将会一直复制至数组结尾

```js
let arr = ['jsx', 'ljj', '520', 'love'];
// target 复制到指定位置
// start,end 开始复制起始和结束位置，范围包括start不包括end
arr.copyWithin(0, 1); // ['ljj', '520', 'love', 'love']
console.log(arr);

// target大于arr.length不会发生拷贝
arr.copyWithin(1, 0);
console.log(arr); //  ['ljj', 'ljj', '520', 'love']

// start,end都为负数，则都从末尾开始计算
arr.copyWithin(0, -1, -3);
console.log(arr); //  ['ljj', 'ljj', '520', 'love']

// start,end被忽略，start则从0开始，end则一直复制到结尾
arr.copyWithin(1);
console.log(arr); // ['ljj', 'ljj', 'ljj', '520']
```



### 5-2 `fill()`

`arr.fill(value, start, end)`

用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引

- `value`：必需。填充的值
- `start`：可选。开始填充位置，如果 `start` 是个负数, 则开始索引会被自动计算成为 `length + start`
- `end`：可选。停止填充位置 (默认为 *array*.length)，如果不提供结束索引，则一直填充到数组末尾。如果是负值，则结束索引会被自动计算成为 `length + end`

```js
let arr = ['html', 'css', 'js'];
arr.fill('vue', 1, 2);
console.log(arr); // ['html', 'vue', 'js']

// 当start或end负数时，开始和结束索引为arr.length + start/end
arr.fill('vue', -1);
console.log(arr); // ['html', 'vue', 'vue']

arr.fill('react', 0, -2);
console.log(arr); // ['react', 'vue', 'vue']
```



## 6. 去扁平化

### 6-1 `flat()`



### 6-2 `flatMap()`



## 7. 迭代遍历

### 7-1 `forEach`



### 7-2 `map()`



### 7-3 `every()`



### 7-4 `some()`



### 7-5 `reduce()`



### 7-6 `reduceRight()`



## 8. 静态方法

### 8-1 `Array.from()`



### 8-2 `Array.isArray()`



### 8-3 `Array.of()`

