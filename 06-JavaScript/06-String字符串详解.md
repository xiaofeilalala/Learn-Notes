# String 字符串详解



## 1. String 字符串 



### 1-1 字符串引号

字符串可以包含在单引号 `''`、双引号 `""` 或反引号 ` `` ` 中

* 单引号和双引号不能混用
* 同类引号不能嵌套：双引号里不能再放双引号，单引号里不能再放单引号
* 单引号里可以嵌套双引号；双引号里可以嵌套单引号
* 反引号允许字符串跨行

```js
let str = 'hello world';
let str1 = "hello world";
let str2 = `${1 + 2}`;
let str3 = '<div class="box"></div>'

let str4 = `jsx
ljj
zdj
ddc`;
```



### 1-2 模版字符串

反引号允许我们通过 `${…}` 将任何表达式嵌入到字符串中

**可以使用变量**

```js
let str = 'world';
console.log(`hello ${str}`); // hello world
```



**可以使用表达式**

```js
let str1 = 2;
console.log(5 - `${str1}`); // 3
console.log(Number(`${5 - 1}`)); // 4
```



**可以调用函数**

模板字符串中可以调用函数。字符串中调用函数的位置，将会显示函数执行后的返回值

```js
function addNum() {
    return 5;
}
console.log(`${addNum()}` + 55); // 555
```



**可以嵌套使用**

```js
let str2 = 'jsx';
let str3 = `${'hello' + '\t' + `${str2}`}`;
console.log(str3);
```



### 1-3 字符串拼接

`+` 加号拼接字符串（隐式类型转换）当字符串类型与其它类型之间使用 `+` 加号时会将其它类型转换为字符串然后拼接组合在一起

拼接前，会把与字符串相加的这个数据类型转成字符串，然后再拼接成一个新的字符串

```tex
字符串 + 任意数据类型 = 拼接之后的新字符串;
```

```js
console.log('js' + 'hello'); //jshello
console.log('js' + 1); //js1
console.log('js' + true); // jstrue
console.log('js' + null); // jsnull
console.log('js' + undefined); // jsundefined
console.log('js' + {name: 'jsx', age: 23}); // js[object Object]
console.log('js' + [1, 2, 3]); // js1,2,3
```

* 空字符串`''`、字符串 `'1'`、字符串 `'0'` 转换为 `''`、`'1'`、`'0'`

* 数字类型 `0`、`1`、`NaN`、`Infinity`、`-Infinity` 转换为 `'0'`、`'1'`、`'NaN'`、`'Infinity'`、`'-Infinity'`
* 布尔类型 `true` 和 `false` 转换为` 'true'`、`'false'`
* `null`、`undefined` 值 转换为 `'null'`、`'undefined'`
* 空对象 `{}` 转换为 `'[object Object]'`
* 数组 `[]`空数组、`['jsx']` 数组、`['jsx', 'ljj']`数组 转换为 `''`、`'jsx'`、`'jsx'` `'ljj'` 
* 函数 `function() {}` 转换为 `'function() {}'`



### 1-4 字符串不可变

字符串不可更改，改变字符是不可能的

通常的解决方法是创建一个新的字符串，并将其分配给 `str` 而不是以前的字符串

```js
let str = 'hello';
console.log(str); // hello
str[0] = 'H';  // 这种方式只能访问字符串不能修改
console.log(str[0]); // h

str =  str[0] + 'ELLO';
console.log(str); // hELLO
```

看上去是改变了字符串，实际上没有改变，只是重新给变量赋值了内存中开辟了新的内存空间，变量指向的地址变了，原始变量的值不会被修改依然存在



## 2. 转义字符

需要避免引号的问题，以确保它们被识别成文本，而不是代码的一部分，可以通过在字符之前放一个反斜杠来实现这一点 `\`

所有的特殊字符都以反斜杠字符 `\` 开始。它也被称为 **转义字符**

可以通过使用换行符，以支持使用单引号和双引号来创建跨行字符串。换行符写作 `\n`

| 字符                                    | 描述                                                         |
| :-------------------------------------- | :----------------------------------------------------------- |
| `\n`                                    | 换行                                                         |
| `\r`                                    | 回车：不单独使用。Windows 文本文件使用两个字符 `\r\n` 的组合来表示换行 |
| `\'`, `\"`                              | 引号                                                         |
| `\\`                                    | 反斜线                                                       |
| `\t`                                    | 制表符                                                       |
| `\b`, `\f`, `\v`                        | 退格，换页，垂直标签 —— 为了兼容性，现在已经不使用了。       |
| `\xXX`                                  | 具有给定十六进制 Unicode `XX` 的 Unicode 字符，例如：`'\x7A'` 和 `'z'` 相同 |
| `\uXXXX`                                | 以 UTF-16 编码的十六进制代码 `XXXX` 的 unicode 字符，例如 `\u00A9` —— 是版权符号 `©` 的 unicode。它必须正好是 4 个十六进制数字 |
| `\u{X…XXXXXX}`（1 到 6 个十六进制字符） | 具有给定 UTF-32 编码的 unicode 符号。一些罕见的字符用两个 unicode 符号编码，占用 4 个字节。这样我们就可以插入长代码了 |

```js
// 插入引号
let str = 'I\'m a beauitfy gril';

// 通过单引号，双引号，反引号联合使用
let str = `I'm a cool boy`;

// 多个反斜杠
let str = '\\\\'; 
console.log(str); // \\

console.log('\\'); // \
console.log('\''); // '
console.log('\"'); // "
console.log('java \r script'); // java  script
console.log('java \b script'); // java 退格符 script
console.log('java \t script'); // java    script
console.log('java \r script'); // java  script
```



## 3. 字符串方法



### 3-1 length 属性

字符串是由若干个字符组成的，这些字符的数量就是字符串的长度。我们可以通过字符串的 `length` 属性可以获取整个字符串的长度

`length` 属性表示字符串长度，`length` **是一个属性而不是一个函数**

- 一个中文算一个字符，一个英文算一个字符

- 一个标点符号（包括中文标点、英文标点）算一个字符

- 一个空格算一个字符

```js
let name = 'jsx';
console.log(name.length); // 3
```



### 3-2 访问字符串

字符的索引第一个字符索引位置为 0, 第二个字符索引位置为 1,以此类推

> **Tips：** 下面两种访问字符串方法都不支持负索引形式，索引不能为负

* `[index]` 通过方括号指定的索引，返回字符串指定索引位置的字符。没有找到字符返回 `undefined`
* `str.charAt(index)` 返回指定索引位置的字符，没有找到字符返回一个空字符串 `''`

```js
let str = 'hello';
console.log(str[0]); // h
// []没有找到字符返回undefined
console.log(str[8]); // undefined

console.log(str.charAt(1)); // e
// str.charAt()没有找到字符返回空字符串''
console.log(str.charAt(8)); // ''

// 访问字符串最后一个字符
console.log(name[name.length-1]);
```



### 3-3 字符串大小写

* `str.toLowerCase()` 字符转换为小写
* `str.toUpperCase()` 字符转换为大写

```js
let str = 'hello';
console.log(str.toLowerCase()); // hello
console.log(str.toUpperCase()); // HELLO

// 改变子字符的大小写
console.log(str[0].toUpperCase() + str.slice(1, str.length-1)); // Hello
```



### 3-4 删除前后空格

`str.trim()` 

去除字符串前后的空白

```js
let str = '  java script   ';
console.log(str); //   java script   
console.log(str.trim()); // java script
```



### 3-5 字符串替换

`str.replace(searchValue, newValue)` 

用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的字符串

* 方法不会改变调用它的字符串。它返回的是新字符串

* 对大小写敏感，只替换首个匹配，如果要全局替换，需要使用正则

```js
let str = 'Hello World';
let str1 = str.replace('World', 'JSX');
console.log(str); // Hello World
console.log(str1); // Hello JSX

// 对大小写敏感，只替换首个匹配 如果要全局替换，需要使用正则
let str2 = 'Today is fine day,today is fine day';
str3 = str2.replace('Today', 'Torrow');
console.log(str2.replace('today', 'Torrow')); // 大小写敏感 匹配不到不替换
console.log(str3); // Torrow is fine day,today is fine day 只匹配了首个

// 替换一个与正则表达式匹配的字符串
str4 = str2.replace(/Today/gi, 'Torrow');
console.log(str4); // Torrow is fine day,Torrow is fine day
```



`str.replaceAll(regexp|substr, newSubstr|function)` 

用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的字符串，该函数会替换所有匹配到的子字符串

```js
let str = 'hello jsx hello ljj';
console.log(str.replaceAll('hello', 'HELLO')); // HELLO jsx HELLO ljj
```



### 3-6 重复字符串

`str.repeat(count)` 

字符串复制指定次数，重复次数不能为负数

* `count` 参数用来设置重复的次数，次数不能为负数

```js
// 字符串复制指定次数
let iphone = '13872458795';
console.log(iphone.slice(0, -4) + '*'.repeat(4)); // 1387245****
        
// 重复次数不能为负
console.log('jsx'.repeat(0));
console.log('jsx'.repeat(2));
```



### 3-7 连接字符串

`str.concat(str1, str2, ..., strN)`

用于连接两个或多个字符串，该方法没有改变原有字符串，但是会返回连接两个或多个字符串新字符串

```js
// 连接两个或者多个字符串 
console.log('hello'.concat(' world')); // hello world

// 不会改变字符串，返回连接字符串的新值
let str = 'jsx';
str.concat('❤ljj');
console.log(str); // jsx
        
let newStr = str.concat('❤ljj');
console.log(newStr); // jsx❤ljj
```



## 4. 查找子字符串



### 4-1 `seach()` 

`str.seach(seachValue)` 

用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并返回某个指定的字符串值在字符串中首次出现的位置索引

* 区分大小写，无法设置第二个开始位置参数

* 如果没有找到任何匹配的字符，则返回 -1

```js
let str = 'hello';
console.log(str.search('o')); // 4
        
// 如果没有找到任何匹配的字符，则返回 -1
console.log(str.search('p')); // -1

// 可以使用正则表达式
console.log(str.search(/E/i)); // 1 忽略大小写匹配字符串
```



### 4-2 `indexOf()`

`str.indexOf(searchValue, start)`

返回某个指定的字符串值在字符串中首次出现的位置

* 区分大小写，如果没有找到匹配的字符串则返回 -1
* 可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是 0 到 `str.length - 1`。如省略该参数，则将从字符串的首字符开始检索

```js
let str = 'hello javascript hello jsx';
console.log(str.indexOf('h')); // 0

// 如果没有找到匹配的字符串则返回 -1
console.log(str.indexOf('b')); // -1

// 可以设置第二个参数，参数为字符串开始检索的起始位置
console.log(str.indexOf('l', 10)); // 19 跳过了前面两个字符
```



### 4-3 `lastIndexOf()`

`str.lastIndexOf(searchValue, start)`

返回一个指定的字符串值最后出现的位置，如果指定第二个参数 `start`，则在一个字符串中的指定位置从后向前搜索，合法取值是 0 到 `str.length - 1`

* 区分大小写，如果没有找到匹配的字符串则返回 -1

* 该方法将从后向前检索字符串，但返回是从起始位置 (0) 开始计算子字符串最后出现的位置

```js
let str = 'jsx ljj';
console.log(str.lastIndexOf('j')); // 6

// 当知道第二个参数时，则在一个字符串中的指定位置从后向前搜索
console.log(str.lastIndexOf('j', 3)); // 0

// 如果没有找到匹配字符串则返回 -1
console.log(str.lastIndexOf('p')); // -1
```



### 4-4 `includes()`

`str.includes(searchValue, start)`

用于判断字符串是否包含指定的子字符串

* 区分大小写，如果找到匹配的字符串则返回 `true`，否则返回 `false`
* 如果不指定参数，则默认为 0 字符串的首字符开始检索；如果指定，则规定了检索从起始位置开始检索，合法取值是 0 到 `str.length - 1`

```js
 let str = 'jsx ljj zdj ddc ljj zdj ddc';
// 如果找到匹配的字符串则返回 true，否则返回 false
console.log(str.includes('ljj')); // true
console.log(str.includes('ccm')); // false

// 可以指定第二个参数，规定了检索从起始位置开始检索(没有第二个参数则从索引0开始)
console.log(str.includes('jsx', 2)); // false 从索引2开始检索
```



### 4-5 `startsWith()`

`str.starstWith(searchValue, start)`

用于检测字符串是否以指定的子字符串开始

* 区分大小写，如果传入的子字符串在搜索字符串的开头则返回 `true`；否则将返回 `false`
* 如果不指定参数，则默认为 0 字符串的首字符开始检索；如果指定，则规定了检索从起始位置开始检索，合法取值是 0 到 `str.length - 1`

```js
let str = 'hello';
// 如果字符串是以指定的子字符串开头返回true，否则返回false
console.log(str.startsWith('h')); // true
console.log(str.startsWith('e')); // false

// 可以指定第二个参数，规定了检索从起始位置开始检索(没有第二个参数则从索引0开始)
console.log(str.startsWith('h', 1)); // false
```



### 4-6 `endsWith()`

`str.endsWith(searchValue, length)`

用来判断当前字符串是否是以指定的子字符串结尾的

* 区分大小写，如果传入的子字符串在搜索字符串的末尾则返回 `true`；否则将返回 `false`
* `length` 参数设置字符串的长度。默认值为原始字符串长度

```js
let str = 'hello';
// 如果传入的子字符串在搜索字符串的末尾则返回 true；否则将返回 false
console.log(str.endsWith('h')); // false
console.log(str.endsWith('o')); // true

// 可以指定第二个参数，设置字符串的长度。默认值为原始字符串长度
console.log(str.startsWith('e', 1)); // true
```



## 5. 获取子字符串

| 方法                    | 选择方式……                                            | 负值参数            |
| :---------------------- | :---------------------------------------------------- | :------------------ |
| `slice(start, end)`     | 从 `start` 到 `end`（不含 `end`）                     | 允许                |
| `substring(start, end)` | `start` 与 `end` 之间（包括 `start`，但不包括 `end`） | 负值代表 `0`        |
| `substr(start, length)` | 从 `start` 开始获取长为 `length` 的字符串             | 允许 `start` 为负数 |



### 5-1 `slice()`

`str.slice(start, end)`

可提取字符串的某个部分，并以新的字符串返回被提取的部分

* 返回字符串从 `start` 到（但不包括）`end` 的部分
* 如果没有第二个参数，`slice()` 会一直运行到字符串末尾
* `start/end` 也有可能是负值（负值索引起始位置从字符串结尾计算 ，字符串末尾位置索引从 `-1` 开始）

```js
let str = 'hello';
console.log(str.slice(1, 3)); // el

// 当没有指定第二个参数时，slice()函数会执行到字符串末尾
console.log(str.slice(2)); // llo

// 参数的值可以为负数， 负数则开始位置索引为字符串末尾
console.log(str.slice(-4)); // ello
console.log(str.slice(-3, -1)); // ll
```



### 5-2 `substring()`

`str.substring(start, end)`

返回字符串在 `start` 到 （但不包括）`end` 的部分

* 允许 `start` 大于 `end`，当 `start` 大于 `end` 自动调整参数的位置自动交换
* 如果没有第二个参数，`substring()` 会一直运行到字符串末尾
* 不支持负参数，负参数视为 0

```js
let str = 'javascript';
console.log(str.substring(3, 2)); // va

// 当 start 大于 end 自动调换位置
console.log(str.substring(5, 1)); // avas

// 当没有第二个参数，则默认到字符串结尾
console.log(str.substring(3)); // ascript

// 不支持负参数 负参数默认为0
console.log(str.substring(-3)); // javascript
console.log(str.substring(-3, -1)); // '' 相当于(0, 0) 空字符
```



### 5-3 `substr()`

`str.substr(start, length)`

> **Tips：**ECMAscript 没有对 `substr()` 方法进行标准化，因此不建议使用它

返回字符串从 `start` 开始的给定 `length` 的部分

* `length` 参数规定被提取部分的字符串长度
* 第一个参数可能是负数，从结尾算起
* 如果没有第二个参数，`substr()` 默认 `length` 参数返回字符串从开始位置到结尾的字符

```js
// 返回字符串从 start 到 给定 length 长度的部分
let str = 'javascript';
console.log(str.substr(2, 1)); // v
console.log(str.substr(-2, 2)); // pt

// 当没有第二个参数时，默认返回从字符串开始到结尾的字符
console.log(str.substr(2)); // vascript
console.log(str.substr(-2)); // pt
```



## 6. 比较字符串

在比较字符串的大小时，`JavaScript` 会使用 `Unicode` 编码顺序，按字符（母）逐个进行比较的

所有的字符串都使用 `UTF-16` 编码，每个字符都有对应的数字代码，有特殊的方法可以获取代码表示的字符，以及字符对应的代码

* `charCodeAt()` 和 `fromCharCode()`  
* `codepointAt()` 和 `fromCodePoint()` 

`JavaScript` 内部，字符以 `UTF-16` 的格式储存，每个字符固定为2个字节

对于那些需要4个字节储存的字符（`Unicode` 码点大于0xFFFF的字符），`JavaScript `会认为它们是两个字符



### 6-1 `charCodeAt()`

`str.charCodeAt(pos)`

返回指定位置字符的 Unicode 编码，返回值是 0 - 65535 之间的整数

只能分别返回前两个字节和后两个字节的值，如果在指定的位置没有元素则返回 NaN

```js
let str = '*^%';
console.log(str.charCodeAt(1)); // 94

// 如果在指定的位置没有元素则返回 NaN
console.log(str.charCodeAt(4)); // NaN
```



### 6-2 `fromCharCode()`

` String.fromCharCode(code)`

用于从 `Unicode` 码点返回对应字符，但是这个方法不能识别码点大于`0xFFFF`的字符

```js
console.log('🦓'.charCodeAt(0)); // 55358 🦓 是4字节 55358为前2个字节的编码值
console.log('🦓'.charCodeAt(1)); // 56723 🦓 是4字符 55358为后2个字节的编码值

console.log('🦓'.codePointAt(0)); // 129427 该方法可以处理4字节 返回一个编码值
console.log(String.fromCharCode(55358 )); // 煉 该方法识别大于0xFFFF的字符最高位2会被舍弃 导致返回的字符不正确
console.log(String.fromCodePoint(129427)); // 🦓 能识别大于0xFFFF的字符
```



### 6-3 `codePointAt()`

`str.codePointAt(pos)`

能够正确处理 4 个字节储存的字符，返回一个字符的 `Unicode` 码点

如果在指定的位置没有元素则返回 `undefined`

```js
console.log('🐹'.codePointAt(0)); // 128057

// 如果在指定的位置没有元素则返回 undefined
console.log('🐹'.codePointAt(5)); // undefined
```



### 6-4 `fromCodePoint()`

`String.fromCodePoint(code)`

用于从 `Unicode` 码点返回对应字符，可以识别大于`0xFFFF`的字符，弥补了`String.fromCharCode()`方法的不足

```js
// 可以识别大于0xFFFF的字符
console.log(String.fromCodePoint(128038)); // 🐦

// 也可以识别小于0xFFFF的字符
console.log(String.fromCodePoint(90)); // Z
```

