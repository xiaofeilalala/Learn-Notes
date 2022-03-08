# Math 和 Date 对象



## 1. Math 对象

**`Math`** 是一个内置对象，它拥有一些数学常数属性和数学函数方法。`Math` 不是一个函数对象

`Math` 不是一个构造器，`Math` 的所有属性与方法都是静态的

`Math` 用于 `Number`类型，它不支持 `bigInt` 类型

| 方法                | 描述                                       |
| :------------------ | :----------------------------------------- |
| `Math.PI`           | 圆周率，`Math` 对象的属性                  |
| `Math.abs()`        | 返回绝对值                                 |
| `Math.random()`     | 生成0 ~ 1之间的随机浮点数                  |
| `Math.floor()`      | 向下取整（往小取值）                       |
| `Math.ceil()`       | 向上取整（往大取值）                       |
| `Math.trunc()`      | 移除小数点后的所有内容而没有舍入           |
| `Math.round()`      | 四舍五入取整（正数四舍五入，负数五舍六入） |
| `Math.max(x, y, z)` | 返回多个数中的最大值                       |
| `Math.min(x, y, z)` | 返回多个数中的最小值                       |
| `Math.pow(x,y)`     | 乘方：返回 x 的 y 次幂                     |
| `Math.sqrt()`       | 开方：对一个数进行开方运算                 |



## 2. 舍入

### 2-1 `Math.floor()`

`Math.floor(num)`

向下舍入往小取值：`3.1` 变成 `3`，`-1.1` 变成 `-2`

```js
console.log(Math.floor(2.9)); // 2
console.log(Math.floor(-1.1)); // -2
```



### 2-2 `Math.ceil()`

`Math.ceil(num)`

向上舍入往大取值：`3.1` 变成 `4`，`-1.1` 变成 `-1`

```js
console.log(Math.ceil(2.9)); // 3
console.log(Math.ceil(-1.1)); // -1
```



### 2-3 `Math.round()`

`Math.round(num)`

四舍五入取整，正数四舍五入，负数五舍六入

向最近的整数舍入：`3.1` 变成 `3`，`3.6` 变成 `4`，中间值 `3.5` 变成 `4`

```js
console.log(Math.round(2.9)); // 3
console.log(Math.round(-1.1)); // -1
```



### 2-4 `Math.trunc()`

`Math.trunc(num)` **IE浏览器不支持**

移除小数点后的所有内容而没有舍入：`3.1` 变成 `3`，`-1.1` 变成 `-1`

```js
console.log(Math.trunc(2.9212)); // 2
console.log(Math.trunc(-1.112)); // -1
```



## 3. 其它数学函数

### 3-1 `Math.random()`

`Math.random()`

返回一个从 0 到 1 的随机数（不包括 1）

```js
 console.log(Math.random());

 // 两个数中的随机数
 function getRandomArbitrary(min, max) {
 	return Math.random() * (max - min) + min;
 }
 console.log(getRandomArbitrary(1, 3));

 // 两个数之间的随机整数
 function getRandomInt(min, max) {
 	min = Math.ceil(min);
 	max = Math.floor(max);
 	return Math.floor(Math.random() * (max - min)) + min; 
     //不含最大值，含最小值
 }
 console.log(getRandomInt(1, 8));

 // 两个数之间的随机整数 包括两个数
 function getRandomIntInclusive(min, max) {
 	min = Math.ceil(min);
 	max = Math.floor(max);
 	return Math.floor(Math.random() * (max - min + 1)) + min; 
     //含最大值，含最小值 
 }
 console.log(getRandomIntInclusive(1, 8));
```



### 3-2 `Math.max()` & `Math.min()`

`Math.max(a, b, c...)` 和 `Math.min(a, b, c...)`

从任意数量的参数中返回最大/最小值

```js
// 在任意参数值返回最大值
console.log(Math.max(2, 4, 7)); // 7
console.log(Math.max(2, 4, 7.5)); // 7.5
        
// 在任意参数值返回最小值
console.log(Math.min(2, 4, 7, 10, 1)); // 1
```



### 3-3 `Math.pow()`

`Math.pow(n, power)`

返回 `n` 的给定 `power` 次幂

```js
// 幂
console.log(Math.pow(2, 4)); // 16
```



### 3-4 `Math.abs()`

`Math.abs(x)`

返回指定数字的绝对值
$$
{\mathtt{\operatorname{Math.abs}(x)}} = {|x|} = \begin{cases} x & \text{if} \quad x \geq 0 \\ -x & \text{if} \quad x < 0 \end{cases}
$$

```js
// 绝对值
console.log(Math.abs(10)); // 10
console.log(Math.abs(-10)); // 10
```



### 3-5 `Math.sqrt()`

`Math.sprt(x)`

返回一个数的平方根

如果参数 `number` 为负值，则 `sqrt` 返回 `NaN`

```js
// 开方
function sqrtNum(a, b) {
	return Math.sqrt((a * a) + (b * b));
}
console.log(sqrtNum(3, 4)); // 5
```



## 4. Date 对象

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220301215125.png)

`Date` 对象用于处理日期与时间

`Date` 与 `Math` 对象不同，Date 对象是一个**构造函数** ，需要**先实例化**后才能使用

```js
let date = new Date();
```



### 4-1 创建

`new Date()`

如果没有提供参数，那么新创建的 `Date` 对象表示实例化时刻的日期和时间，将返回一个字符串，而非 `Date` 对象

```js
let date = new Date();
console.log(date); // Tue Mar 01 2022 22:13:05 GMT+0800 (中国标准时间)
```



`new Date(value)`

自 `1970-01-01 00:00:00` 以来经过的毫秒数，该整数被称为 **时间戳**

`JavaScript` 中时间戳以毫秒为单位，而不是秒

通过时间戳来创建日期，并可以使用 `date.getTime()` 将现有的 `Date` 对象转化为时间戳

```js
// 0 表示 01.01.1970 UTC+0
let date = new Date(0);
console.log(date); // Thu Jan 01 1970 08:00:00 GMT+0800 (中国标准时间)
```



`new Date(datestring)`

表示日期的字符串值，如果只有一个参数，并且是字符串，那么它会被自动解析。该算法与 `Date.parse` 所使用的算法相同

```js
let date = new Date("2022-03-02");
console.log(date); // Wed Mar 02 2022 08:00:00 GMT+0800 (中国标准时间)
// 会根据你运行代码时的时区进行调整
```



`new Date(year, month, date, hours, minutes, seconds, ms)`

使用当前时区中的给定组件创建日期。只有前两个参数是必须的。

- `year` 表示年份的整数值，必须是四位数：`2013` 是合法的，`98` 是不合法的
- `month` 表示月份的整数值，计数从 `0`（一月）开始，到 `11`（十二月）结束
- `date` 表示一个月中的第几天的整数值，是当月的具体某一天，如果缺失，则为默认值 `1`
- `hours` 表示一天中的小时数的整数值 (24小时制)，默认值为 `0`（午夜）
- `minutes` 表示一个完整时间（如 01:10:00）中的分钟部分的整数值，默认值为 `0`
- `seconds` 表示一个完整时间（如 01:10:00）中的秒部分的整数值，默认值为 `0`
- `ms` 表示一个完整时间的毫秒部分的整数值，默认值为 `0`
- 如果 `hours/minutes/seconds/ms` 缺失，则均为默认值 `0`

```js
// 1秒 = 1000 毫秒
let date = new Date(2022, 2, 2, 22, 50, 59, 999);
console.log(date); // Wed Mar 02 2022 22:50:59 GMT+0800 (中国标准时间)
```



## 5. Date 对象方法

### 5-1 `Date.now()`

`Date.now()`

会返回当前的时间戳，相当于 `new Date().getTime()`，但它不会创建中间的 `Date` 对象

```js
console.log(Date.now()); //1646208865454
console.log(new Date(1646208865454)); 
// Wed Mar 02 2022 16:14:25 GMT+0800 (中国标准时间)

// 也可用于性能测试
let start = Date.now();
for (let i = 0; i <= 1000; i++) {
	console.log(i)
}
let end = Date.now();
let result = `使用了${end - start}毫秒`
console.log(result); // 返回的时毫秒
```



### 5-2 `Date.parse()`

`Date.parse(str)`

解析一个表示日期的字符串，并返回表示日期的时间戳

字符串的格式应该为：`YYYY-MM-DDTHH:mm:ss.sssZ`，其中：

- `YYYY-MM-DD` —— 日期：年-月-日
- 字符 `"T"` 是一个分隔符
- `HH:mm:ss.sss` —— 时间：小时，分钟，秒，毫秒
- 可选字符 `'Z'` 为 `+-hh:mm` 格式的时区。单个字符 `Z` 代表 UTC+0 时区

```js
console.log(Date.parse('2022-03-02')); // 1646179200000
console.log(new Date(1646179200000)); 
// Wed Mar 02 2022 08:00:00 GMT+0800 (中国标准时间)

// 字符串格式不对返回NaN
console.log(Date.parse('12asd')); // NaN
```

`Date.parse(str)` 调用会解析给定格式的字符串，并返回时间戳（自 1970-01-01 00:00:00 起所经过的毫秒数）

如果给定字符串的格式不正确，则返回 `NaN`



### 5-3 `Date.UTC()`

`Date.UTC(year,month,date,hrs,min,sec,ms) `

以世界时间为标准，并返回表示日期的时间戳，接受以逗号隔开的日期参数

* `year` 1900 年后的某一年份
* `month` 0 到 11 之间的一个整数，表示月份
* `date` 1 到 31 之间的一个整数，表示某月当中的第几天
* `hrs` 0 到 23 之间的一个整数，表示小时
* `min` 0 到 59 之间的一个整数，表示分钟
* `sec` 0 到 59 之间的一个整数，表示秒
* `ms` 0 到 999 之间的一个整数，表示毫秒

```js
var utcDate = new Date(Date.UTC(96, 11, 1, 0, 0, 0));
console.log(utcDate); // Sun Dec 01 1996 08:00:00 GMT+0800 (中国标准时间)

// 当年分指定在0-99时该方法会默认1900+year
// 当月份超出范围 年分会增加 月份也会重新开始
console.log(new Date(Date.UTC(2021, 15, 0, 0, 0, 0)));
// Thu Mar 31 2022 08:00:00 GMT+0800 (中国标准时间)
```

如果年份被指定为 0 到 99 之间，则该方法会将年份转换为 20 世纪的一个年份（即 1900 + year）

如果有一个指定的参数超出其合理范围，则 UTC 方法会通过更新其他参数直到该参数在合理范围内，月份指定 15，则年份将会加 1，然后月份将会使用 3



## 6. 访问日期组件

### 6-1 本地时区标准

* `getFullYear()` 根据本地时间，返回一个指定的日期的年份（返回一个4位数年份）
* `getMonth()` 根据本地时间，返回一个指定的日期对象的月份（返回一个从 0 到 11的整数值）
* `getDate()` 根据本地时间，返回一个指定的日期对象为一个月中的哪一日（返回一个从 1 到 31的整数值）
* `getHours()` 返回一个指定的日期对象的小时（返回一个从 0 到 23的整数值）
* `getMinutes()` 根据本地时间，返回一个指定的日期对象的分钟数 （返回一个0 到 59的整数值的整数值）
* `getSeconds()` 根据本地时间，返回一个指定的日期对象的秒数（返回一个 0 到 59 的整数值）
* `getMilliseconds()` 根据本地时间，返回一个指定的日期对象的毫秒数（返回一个0 到 999的整数）
* `getDay()` 根据本地时间，返回一个具体日期中一周的第几天，0 表示星期天 （返回一个0到6之间的整数值）

```js
// 可以自定义日期,也可以不传参数返回当前日期
let date = new Date('Wed Mar 02 2022 20:49:37.100 GMT+0800 (中国标准时间)');
console.log(date);

// 根据本地时间返回指定日期对象的年份 4位数
console.log(date.getFullYear()); // 2022

// 根据本地时间返回指定日期对象的月份
console.log(date.getMonth()); // 2 

// 根据本地时间返回指定日期对象中的一日
console.log(date.getDate()); // 2

// 根据本地时间返回指定日期对象的小时
console.log(date.getHours()); // 20

// 根据本地时间返回指定日期对象的分钟数
console.log(date.getMinutes()); // 49

// 根据本地时间返回指定日期对象的秒数
console.log(date.getSeconds()); // 37

// 根据本地时间返回指定日期对象的毫秒数
console.log(date.getMilliseconds()); // 100

// 根据本地时间返回指定日期对象的星期几
console.log(date.getDay()); // 3
```



### 6-2 世界标准时间

北京时间所属时区: UTC/GMT +8

* `getUTCFullYear()` 以世界时为标准，返回一个指定的日期的年份（返回一个4位数年份）
* `getUTCMonth()` 以世界时为标准，返回一个指定的日期对象的月份（返回一个从 0 到 11的整数值）
* `getUTCDate()` 以世界时为标准，返回一个指定的日期对象为一个月中的哪一日（返回一个从 1 到 31的整数值）
* `getUTCHours()` 以世界时为标准，返回一个指定的日期对象的小时（返回一个从 0 到 23的整数值）
* `getUTCMinutes()` 以世界时为标准，返回一个指定的日期对象的分钟数 （返回一个0 到 59的整数值的整数值）
* `getUTCSeconds()` 以世界时为标准，返回一个指定的日期对象的秒数（返回一个 0 到 59 的整数值）
* `getUTCMilliseconds()` 以世界时为标准，返回一个指定的日期对象的毫秒数（返回一个0 到 999的整数）
* `getUTCDay()` 以世界时为标准，返回一个具体日期中一周的第几天，0 表示星期天 （返回一个0到6之间的整数值）

```js
// UTC 世界时间标准日期
// 北京时间所属时区: UTC/GMT +8
// 可以自定义日期,也可以不传参数返回当前日期
let date = new Date('Wed Mar 02 2022 20:49:37.100 GMT+0800 (中国标准时间)');
console.log(date);

// 以世界时为标准返回指定日期对象的年份 4位数
console.log(date.getUTCFullYear()); // 2022

// 以世界时为标准返回指定日期对象的月份
console.log(date.getUTCMonth()); // 2 

// 以世界时为标准返回指定日期对象中的一日
console.log(date.getUTCDate()); // 2

// 以世界时为标准返回指定日期对象的小时
console.log(date.getUTCHours()); // 12

// 以世界时为标准返回指定日期对象的分钟数
console.log(date.getUTCMinutes()); // 49

// 以世界时为标准返回指定日期对象的秒数
console.log(date.getUTCSeconds()); // 37

// 以世界时为标准返回指定日期对象的毫秒数
console.log(date.getUTCMilliseconds()); // 100

// 返回指定日期对象的星期几
console.log(date.getUTCDay()); // 3
```



### 6-3 `getTime()`

`getTime()`  方法返会当前时间戳，从 1970-1-1 00:00:00 UTC+0 开始到现在所经过的毫秒数

```js
// getTime() 返回一个从 1970-1-1 00:00:00 UTC+0 开始到现在所经过的毫秒数
let date = new Date();
console.log(date); // Wed Mar 02 2022 22:13:14 GMT+0800 (中国标准时间)
console.log(date.getTime()); // 1646230394645
```



### 6-4 `getTimezoneOffset()`

`getTimezoneOffset()` 方法返回协调世界时（UTC）相对于当前时区的时间差值，单位为分钟

```js
let date = new Date();
// 返回 UTC 与本地时区之间的时差以分钟为单位
console.log(date.getTimezoneOffset()); // -480
// 480分钟 8小时 与UTC世界时间相差8小时
```



## 7. 设置日期组件

### 7-1 本地时区标准

`setFullYear(yearValue, monthValue, dayValue)`

根据本地时间为一个日期对象设置年份

* 如果你没有指定具体的 `monthValue` 和 `dayValue`，将会使用 `getMonth()` 和 `getDate()` 方法的返回值
* 如果你指定的参数超出了期待范围，`setFullYear()` 方法将会根据 `Date` 对象，更新其他参数和日期信息

```js
let date = new Date('Wed Mar 02 2022 20:49:37.100 GMT+0800 (中国标准时间)');
console.log(date.setFullYear(2008, 10, 20));
console.log(date); // Thu Nov 20 2008 20:49:37 GMT+0800 (中国标准时间)
```



`setMonth(monthValue, dayValue)`

根据本地时间为一个设置年份的日期对象设置月份

* 如果不指定 `dayValue` 参数，就会使用 `getDate()`方法的返回值。
* 如果有一个指定的参数超出了合理范围，`setMonth()` 会相应地更新日期对象中的日期信息

```js
console.log(date.setMonth(11));
console.log(date); // Sat Dec 20 2008 20:49:37 GMT+0800 (中国标准时间)
```





`setDate(datevalue)` 

根据本地时间来指定一个日期对象的天数

* 如果 `dayValue` 指定为 0，那么日期就会被设置为上个月的最后一天
* 如果 `dayValue` 设置为负数，日期会设置为上个月最后一天往前数这个负数绝对值天数后的日期

```js
// 设置为o,则为上个月最后一天
console.log(date.setDate(0));
console.log(date); // Sun Nov 30 2008 20:49:37 GMT+0800 (中国标准时间)
        
// 设置为负数 则是上个月最后一天减去负数的绝对值
console.log(date.setDate(-3)); // 31 - 3 = 28
console.log(date); // Sun Nov 28 2008 20:49:37 GMT+0800 (中国标准时间)
```



`setHours(hoursValue, minutesValue, secondsValue, msValue)`

根据本地时间为一个日期对象设置小时数

* 如果不指定 `minutesValue`，`secondsValue` 和 `msValue` 参数，则会使用 `getMinutes()`，`getSeconds()` 和 `getMilliseconds()` 方法的返回值
* 如果有一个参数超出了合理范围，`setHours()` 会相应地更新日期对象中的日期信息

```js
console.log(date.setHours(20));
console.log(date); // Tue Oct 28 2008 20:49:37 GMT+0800 (中国标准时间)
```



`setMinutes(minutesValue, secondsValue, msValue)` 

根据本地时间为一个日期对象设置分钟数

* 如果不指定 `secondsValue` 和 `msValue` 参数，则会使用，`getSeconds()` 和 `getMilliseconds()` 方法的返回值
* 如果有一个指定的参数超出了合理范围，`setMinutes()` 会相应地更新日期对象中的时间信息

```js
console.log(date.setMinutes(59));
console.log(date); // Tue Oct 28 2008 20:59:37 GMT+0800 (中国标准时间)
```



`setSeconds(secondsValue, msValue)` 

根据本地时间设置一个日期对象的秒数

* 如果没有指定 `msValue` 参数，就会使用 `getMilliseconds()` 方法的返回值
* 如果一个参数超出了合理范围， `setSeconds()` 方法会相应地更新日期对象的时间信息

```js
console.log(date.setSeconds(33));
console.log(date); // Tue Oct 28 2008 20:59:33 GMT+0800 (中国标准时间)
```



`setMilliseconds(millisecondsValue)`

据本地时间设置一个日期对象的豪秒数

* 如果指定的数字超出了合理范围，则日期对象的时间信息会被相应地更新

```js
console.log(date.setMilliseconds(6000));
console.log(date); // Tue Oct 28 2008 20:59:39 GMT+0800 (中国标准时间)
```



### 7-2 世界时间标准

只需要在 `set` 之后插入 `UTC` 即可

北京时间所属时区: UTC/GMT +8



`setUTCFullYear(yearValue, monthValue, dayValue)`

根据世界标准时间为一个日期对象设置年份

* 如果你没有指定具体的 `monthValue` 和 `dayValue`，将会使用 `getUTCMonth()` 和 `getUTCDate()` 方法的返回值
* 如果你指定的参数超出了期待范围，`setUTCFullYear()` 方法将会根据 `Date` 对象，更新其他参数和日期信息

```js
let date = new Date('Wed Mar 02 2022 20:49:37.100 GMT+0800 (中国标准时间)');
        
console.log(date.setUTCFullYear(2008, 10, 20));
console.log(date); // Thu Nov 20 2008 20:49:37 GMT+0800 (中国标准时间)
```



`setUTCMonth(monthValue, dayValue)`

根据世界标准时间为一个设置年份的日期对象设置月份

* 如果不指定 `dayValue` 参数，就会使用 `getUTCDate()`方法的返回值。
* 如果有一个指定的参数超出了合理范围，`setUTCMonth()` 会相应地更新日期对象中的日期信息

```js
console.log(date.setUTCMonth(11));
console.log(date); // Sat Dec 20 2008 20:49:37 GMT+0800 (中国标准时间)
```



`setUTCDate(datevalue)` 

根据世界标准时间来指定一个日期对象的天数

* 如果 `dayValue` 指定为 0，那么日期就会被设置为上个月的最后一天
* 如果 `dayValue` 设置为负数，日期会设置为上个月最后一天往前数这个负数绝对值天数后的日期

```js
// 设置为0,则为上个月最后一天
console.log(date.setUTCDate(0));
console.log(date); // Sun Nov 30 2008 20:49:37 GMT+0800 (中国标准时间)
        
// 设置为负数 则是上个月最后一天减去负数的绝对值
console.log(date.setUTCDate(-3)); // 31 - 3 = 28
console.log(date); // Sun Nov 28 2008 20:49:37 GMT+0800 (中国标准时间)
```



`setUTCHours(hoursValue, minutesValue, secondsValue, msValue)`

根据世界标准时间为一个日期对象设置小时数

* 如果不指定 `minutesValue`，`secondsValue` 和 `msValue` 参数，则会使用 `getUTCMinutes()`，`getUTCSeconds()` 和 `getUTCMilliseconds()` 方法的返回值
* 如果有一个参数超出了合理范围，`setUTCHours()` 会相应地更新日期对象中的日期信息

```js
console.log(date.setUTCHours(20)); // 20 + 8 = 28 = 4
console.log(date); // Wed Oct 29 2008 04:49:37 GMT+0800 (中国标准时间)
```



`setUTCMinutes(minutesValue, secondsValue, msValue)` 

根据世界标准时间为一个日期对象设置分钟数

* 如果不指定 `secondsValue` 和 `msValue` 参数，则会使用，`getUTCSeconds()` 和 `getUTCMilliseconds()` 方法的返回值
* 如果有一个指定的参数超出了合理范围，`setUTCMinutes()` 会相应地更新日期对象中的时间信息

```js
console.log(date.setUTCMinutes(59));
console.log(date); // Wed Oct 29 2008 04:59:37 GMT+0800 (中国标准时间)
```



`setUTCSeconds(secondsValue, msValue)` 

根据世界标准时间设置一个日期对象的秒数

* 如果没有指定 `msValue` 参数，就会使用 `getUTCMilliseconds()` 方法的返回值
* 如果一个参数超出了合理范围， `setUTCSeconds()` 方法会相应地更新日期对象的时间信息

```js
console.log(date.setUTCSeconds(33));
console.log(date); // Wed Oct 29 2008 04:59:33 GMT+0800 (中国标准时间)
```



`setUTCMilliseconds(millisecondsValue)`

根据世界标准时间设置一个日期对象的豪秒数

* 如果指定的数字超出了合理范围，则日期对象的时间信息会被相应地更新

```js
console.log(date.setUTCMilliseconds(6000));
console.log(date); // Wed Oct 29 2008 04:59:39 GMT+0800 (中国标准时间)
```



### 7-3 `setTime()`

`setTime(timeValue)`

使用 `setTime()` 方法用来把一个日期时间赋值给另一个 `Date `对象， 传入一个时间戳

```js
let date = new Date();
console.log(Date.parse('Wed Mar 02 2022 21:56:22 GMT+0800 (中国标准时间)'));
console.log(date.setTime(1646229382000));
console.log(date); // Wed Mar 02 2022 21:56:22 GMT+0800 (中国标准时间)
```



## 8. Date 组件其它方法

### 8-1 获取 Date 对象时间戳

```js
// 方式一：获取 Date 对象的时间戳（最常用的写法）
const timestamp1 = +new Date();
console.log(timestamp1); // 打印结果举例：1646229838796

// 方式二：获取 Date 对象的时间戳（较常用的写法）
const timestamp2 = new Date().getTime();
console.log(timestamp2); // 打印结果举例：1646229838796

// 方式三：获取 Date 对象的时间戳
const timestamp3 = new Date().valueOf();
console.log(timestamp3); // 打印结果举例：1646229838796

// 方式4：获取 Date 对象的时间戳
const timestamp4 = new Date() * 1;
console.log(timestamp4); // 打印结果举例：1646229838796

// 方式5：获取 Date 对象的时间戳
const timestamp5 = Number(new Date());
console.log(timestamp5); // 打印结果举例：1646229838796
```



### 8-2 性能检测

连续运行函数很多次，并计算时间差

```js
// 通过getTime获取时间戳 将函数运行时间相减来测量
function fun1(a, b) {
	return 100000000 + a * b;
}

function fun2(a, b) {
	return a ** b;
}

function compare(f) {
	let x = 14564506456;
	let y = 20045666666666666;

	let start = Date.now();
	for (let i = 0; i <= 100000; i++) {
		f(x, y);
	}
	return Date.now() - start;
}

let time1 = 0;
let time2 = 0;

for (let i = 0; i < 10; i++) {
	time1 += compare(fun1);
	time2 += compare(fun2);
}
console.log(time1);
console.log(time2);
```



### 8-3 moment.js

`JavaScript` 日期处理类库

引入`js` 文件 `<script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.29.1/moment.min.js"></script>`

引入语言环境库 `<script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.29.1/locale/zh-cn.js"></script>`

```html
// 日期格式化
moment().format('MMMM Do YYYY, h:mm:ss a');

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>moment.js日期函数库</title>
    <script src="http://cdn.staticfile.org/moment.js/2.24.0/moment.min.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.29.1/locale/zh-cn.js"></script>
    <script>
        console.log(moment().format('MMMM Do YYYY, h:mm:ss a'))
        document.documentElement.innerHTML = `<h1>${moment().format('MMMM Do YYYY, h:mm:ss a')}</h1>`;
    </script>
</head>

<body>
    
</body>

</html>
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220303194530.png)

