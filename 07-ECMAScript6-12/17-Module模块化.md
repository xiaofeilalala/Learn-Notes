# Module 模块化

## 1. 模块起步

### 1-1 模块化规范

* `AMD` —— 异步模块定义规范，最初由 `require.js` 库实现，用于浏览器的模块系统
* `CommonJS` —— 为 `Node.js` 服务器创建的模块系统
* `UMD` —— 通用模块定义规范，作为通用的模块系统
* `CMD` —— 阿里 `SeaJS`，用于浏览器的模块系统
* `ES6 Module` —— `JavaScript` 模块浏览器和服务器通用的模块系统

| 规范       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| `CommonJS` | 是服务器模块的规范，加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作 |
| `AMD`      | 规范加载模块是异步的，并允许函数回调，不必等到所有模块都加载完成，后续操作可以正常执行，只要模块作为依赖时，就会加载并初始化 |
| `CMD`      | `CMD` 规范和 `AMD` 类似通用模块定义，模块作为依赖且被引用时才会初始化，否则只会加载 |
| `UMD`      | 通用模块定义，`UMD` 是 `AMD` 和 `CommonJS` 的结合，实现跨平台的解决方案，先判断是否支持 `Node.js` 的模块（`exports`）是否存在，存在则使用 `Node.js` 模块模式。再判断是否支持 `AMD`（`define` 是否存在），存在则使用 `AMD` 方式加载模块 |



### 1-2 什么是模块？

`ES Module` 是 `ES6` 中提出的一个新的模块加载方式，完全可以取代 `commonJs` 和 `AMD` 两种模块加载方式，成为服务端和浏览器端模块加载的解决方案

`ES6` 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量，所以在资源加载速度和静态分析方面效率更高

* `export` —— 从当前模块外部访问的变量和函数
* `import` —— 允许从其他模块导入功能

> 通过使用 `<script type="module">` 特性告诉浏览器，脚本应该被当作模块 `module` 来对待

```html
<!-- 添加type="module"让浏览器以模块解析脚本 -->
<script type="module">
  // import 从其它模块引入功能
  import { module } from './module.js'
  console.log(module)
</script>
```

```js
// module.js
let module = 'ES Modules'
// export 从当前模块导出变量与方法
export { module }
```



### 1-3 模块严格模式

在模块下始终在严格模式下运行，始终使用 `use strict`

* 变量必须声明后再使用
* 函数的参数不能有同名属性，否则报错
* 全局 `this` 执行 `undefined`

```html
<script>
  // 不在模块下允许 this指向window
  console.log(this); // window
</script>
<script type="module">
  console.log(this); // defined
</script>
```



### 1-4 模块作用域

每个模块都有自己的作用域，一个模块中的作用域变量和函数在其他脚本中是不可访问的

对于模块，我们使用导入/导出而不是依赖全局变量

```js
<script src="./normal.js"></script>

<!-- 模块A -->
<script type="module">
  let module = 'module js'
</script>

<!-- 模块B -->
<script type="module">
  // 对于模块只能使用导入导出 不能依赖全局变量
  console.log(module)
  //Uncaught ReferenceError module is not defined
</script>

<script>
  // 不在模块下可访问外部组用于
  console.log(normal)
  console.log(module); 
  //Uncaught ReferenceError module is not defined
</script>
```

```js
// normal.js
let normal = 'normal js'
```



### 1-5 模块解析

模块代码仅在第一次导入时被解析，当导出的对象或者其它类型数据时被修改了，后面再导入时获取的是修改后的新值

`import.meta` 对象包含关于当前模块的 `url` 信息，在浏览器环境中，它包含当前脚本的 `URL`，或者如果它是在 `HTML` 中的话，则包含当前页面的 `URL`

```html
<script type="module">
  // import.meta获取当前脚本url
  console.log(import.meta.url)
    
  // 模块代码仅在第一次导入时被解析
  // a模块导入module立即执行改变name属性 b模块引入module里的name是被更改的
  import {} from './module-a.js'
  import {} from './module-b.js'
</script>
```

```js
// module.js
// 模块代码只会在第一次导入时解析
export let obj = {
  name: 'jsx'
}
```

```js
// module-a.js
import { obj } from './module.js'
obj.name = 'ljj'
```

```js
// module-b.js
import { obj } from './module.js'
console.log(obj.name)
```



### 1-6 模块延迟

模块脚本是延迟加载的，与 `defer` 特性一样

* 外部模块脚本 `<script type="module" src="...">` 不会阻塞 `HTML` 的处理，它们会与其他资源并行加载
* 模块脚本会等到 `HTML` 文档完全准备就绪，然后才会运行
* 保持脚本的相对顺序：在文档中排在前面的脚本先执行

```html
<!-- 普通脚本不会延时加载不会等待文档加载会阻塞文档加载 -->
<!-- 1. defer -->
<!-- 2. 脚本放置元素后 -->
<!-- defer async只能用于外部引入的脚本延时加载 -->
<script src="./normal.js"></script>

<!-- 模块脚本具有defer属性 延时加载不会阻塞文档加载 等文档加载完再加载 -->
<script type="module">
  let head = document.querySelector('h3')
  console.log(head)
</script>

<script type="module" src="./module.js"></script>
```

```js
// module.js
// 延时加载
let head = document.querySelector('h3')
console.log(head)
```

```js
// normal.js
let head = document.querySelector('h3')
console.log(head)
```





具有 `type="module"` 的内联脚本具有 `async` 属性，不会等待任何东西，文档加载预模块脚本谁先加载完谁就先执行

```html
<!-- 模块内联脚本可以使用async 不会等待文档加载不会阻塞 -->
<script type="module" async>
  let head = document.querySelector('h3')
  console.log(head)
</script>
```



具有 `type="module"` 的外部脚本，具有相同 `src` 的外部脚本仅运行一次

从另一个源获取的外部脚本需要配置 `CORS` 跨域请求，远程服务器必须提供表示允许获取的 `header` `Access-Control-Allow-Origin`

```html
<!-- 相同的外部脚本引入只运行一次 -->
<script type="module" src="./module.js" async></script>
<script type="module" src="./module.js" async></script>

<!-- cdnjs.cloudflare.com 必须提供 Access-Control-Allow-Origin -->
<!-- 否则，脚本将无法执行 -->
<script type="module" src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.14/vue.min.js"></script>
```



### 1-7 模块路径与兼容

* `import` 必须给出相对或绝对的 `URL` 路径，没有任何路径的模块被称为裸模块。在 `import` 中不允许这种模块
* 通过 `nomodule` 特性设置不支持 `type="module"` 模块的浏览器提示

```html
<!-- 模块中import必须使用绝对或者相对路径 没有任何路径的模块无效 -->
<script type="module">
  // 相对引用必须以“/”、“/”或“./”开头
  // import { module } from 'module.js'
  import { module } from './module.js'
</script>

<!-- 兼容处理 nomodule属性 -->
<script type="module">
  alert("支持ES Module");
</script>

<script nomodule>
  alert("不支持ES Module")
</script>
```



## 2. 导入&导出

`ES6` 使用基于文件的模块，即一个文件一个模块

- 使用 `export` 将开发的接口导出
- 使用 `import` 导入模块接口
- 使用 `*` 可以导入全部模块接口
- 导出是以引用方式导出，无论是标量还是对象，即模块内部变量发生变化将影响已经导入的变量

| 表达式                                       | 说明             |
| -------------------------------------------- | ---------------- |
| export function show(){}                     | 导出函数         |
| export const name='jsx'                      | 导出变量         |
| export class User{}                          | 导出类           |
| export default show                          | 默认导出         |
| const name = 'jsx' export {name}             | 导出已经存在变量 |
| export {name as hd_name}                     | 别名导出         |
| import defaultVar from 'hello.js'            | 导入默认导出     |
| import {name,show} from 'hello.j'            | 导入命名导出     |
| Import {name as asName,show} from 'hello.js' | 别名导入         |
| Import * as api from 'hello.js'              | 导入全部接口     |



### 2-1 声明与导出

导出 `export` 和导入`import` 指令使用



在声明前导出 —— 通过在声明之前放置 `export` 来标记任意声明为导出，无论声明的是变量，函数还是类都可以

```js
// 变量声明前导出
export let module = 'export'

// 函数声明前导出
export function sayHi() {
  console.log('Hi')
}

// 类声明前导出
export class User {
  constructor(name) {
    this.name = name
  }
}
```



导出与声明分开 —— 可以将 `export` 分开放置，通过变量，函数名，类名导出

```js
// 声明与导出分开
let name = 'jsx'

function sayHi() {
  console.log('Hi')
}

class User {
  constructor(name) {
    this.name = name
  }
}

export {name, sayHi, User}
```



### 2-2 具名导入

具名导入 —— 通过 `import {...}` 按名称导入

```js
// 具名导出 通过import{} 按export导出名导出
import {name, sayHi, User} from './export.js'
console.log(name)
sayHi()
let user = new User('ljj')
console.log(user.name)
```

```js
// export.js
let name = 'jsx'

function sayHi() {
  console.log('Hi')
}

class User {
  constructor(name) {
    this.name = name
  }
}

export {name, sayHi, User}
```



`import` 静态导入时，模块路径必须是原始类型字符串，无法根据条件或者在运行时导入，模块默认是在顶层静态导入，为了方便模块编译打包

> 请注意在 `{...}` 中的 `import/export` 语句无效

```js
if (true) {
  // import静态导入无法条件判断导入
  import {name, sayHi, User} from './export.js'
}
```



### 2-3 全部导入

全部导入 —— 使用 `import * as objName` 将所有内容导入为一个对象

```js
// 通过import*全部导入到一个对象内
import * as all from './export.js'
console.log(all.name)
all.sayHi()
let user = new all.User('ljj')
console.log(user.name)
```



按需导入 —— 现代的构建工具 `webpack` 会将模块打包到一起并对其进行优化，以加快加载速度并删除未使用的代码

优化器会检测到 `import` 导入的变量，并从打包好的代码中删除那些未被使用的函数，从而使构建更小

```js
// 按需导入 通过import{} 按需求导入 没有引入的函数在打包时会删除减少打包体积
import {sayHi} from './export.js'
sayHi(); // Hi
```

```js
// export.js
// sayHello 函数在打包优化时会删除，因为未被引用
function sayHi() {
  console.log('Hi')
}

function sayHello() {
  console.log('Hello')
}

export {sayHi, sayHello}
```



### 2-4 别名

导出别名 —— 模块可以通过 `as` 对导出给外部的功能起别名

```js
// 别名导出 通过as重命名
function sayHi() {
  console.log('Hi')
}

function sayHello() {
  console.log('Hello')
}

export {sayHi as hi, sayHello as hello}
```



导入别名 —— 可以为导入的模块重新命名通过 `as` ，可以防止模块命名冲突与模名称简洁性

```html
<script type="module">
  // 别名导出后 引入必须是导出的别名
  import { hi, hello } from './alias.js'
  hi()
  hello()
</script>

<script type="module">
  // import as 配置别名
  import { hi as sayHi, hello as sayHello } from './alias.js'
  sayHi()
  sayHello()
</script>
```



### 2-5 默认导出

模块分为两种：

* 包含库或函数包的模块
* 声明单个实体的模块

模块提供了一个特殊的默认导出 `export default` 语法，用于导出一个模块中只有单个实体的模块

* 将 `export default` 放在要导出的实体前
* 每个文件可能只有一个 `export default`
* 默认导出的模块，导入时不需要花括号
* 由于每个文件最多只能有一个默认的导出，因此导出的实体可以没有名称

> 命名的导出会强制我们使用正确的名称进行导入，对于默认的导出，我们总是在导入时选择名称，团队成员可能会使用不同的名称来导入相同的内容

| 命名的导出                | 默认的导出                        |
| :------------------------ | :-------------------------------- |
| `export class User {...}` | `export default class User {...}` |
| `import {User} from ...`  | `import User from ...`            |

```js
// export default 一个模块只能有一个
export default function sayHello() {
  console.log('Hello')
}
```

```js
// 默认导入模块不需要{}
import sayHello from './default.js'
sayHello()
```

```js
// 默认导出可以没有名称
export default sayHello() {
  console.log('Hello')
}
```



### 2-6 混合导出

`default` 关键字被用于引用默认的导出，可以通过 `as default` 将函数与其定义分开导出（将函数默认导出）

```js
// 通过default关键字将函数与定义分开导出
function sayHello () {
  console.log('Hello')
}

export { sayHello as default }
```

```js
import {default as sayHello} from './mixin.js'
sayHello()
```



在一个模块中同时有默认的导出和命名的导出时，使用 `default as` 别名为默认导出定义

```js
// 当模块有具名与默认导出时
export default function () {
  console.log('Hi')
}

export function sayBye() {
  console.log('bye')
}
```

```js
// 模块导出默认与具名导出时
// default表示默认导出 将default别名配置
import {default as sayHi, sayBye} from './mixin.js'
sayHi()
sayBye()
```



将所有内容 `*` 作为一个对象导入，那么 `default` 属性是默认的导出

```js
// *所有导入时 default属性为默认导入
import * as user from './mixin.js'
user.default();
console.log(user)
user.sayBye()
```



### 2-6 重新导出

`export ... from ...` 允许导入内容，并立即将其导出

当一个包含大量模块的文件夹需要导出一些功能到外部，每个模块都有一些功能，需要通过单个入口暴露包的功能（单个模块包含所有模块暴露的功能），需要将各个模块的功能在主文件中 `import` 导入再 `export` 导出给外部使用

* 重新导出时，默认导出需要单独处理，必须明确写出 `export {default as User}`
* 通过 `export * from ...` 重新导出只导出了命名的导出，忽略默认的导出
* 再通过 `export {default as 别名} from ...` 重新导出默认的导出

```js
import {sayBye, friend} from './index.js'
friend()
sayBye()
```

```js
// index.js
// 需要先导入各个模块功能 再导出
// import {getName, getGrilFriend, default as friend} from './get.js'
// export {getName, getGrilFriend, friend}

// import {sayHello, sayHi, default as sayBye} from './say.js'
// export {sayHello, sayHi, sayBye}

// 通过export from 导入重新导出
// export {getName, getGrilFriend, default as friend} from './get.js'
// export {sayHello, sayHi, default as sayBye} from './say.js'

// 重新导入默认导出
// export * from ... 重新导出命名的导出
// export {default} 重新导出默认的导出
export * from './get.js'
export {default as friend} from './get.js'

export * from './say.js'
export {default as sayBye} from './say.js'
```

```js
// get.js
function getName() {
  console.log('jsx')
}

function getGrilFriend() {
  console.log('ljj')
}

function friend() {
  console.log('ddc zj')
}

export {getName, getGrilFriend, friend as default}
```

```js
// say.js
function sayHello() {
  console.log('Hello')
}

function sayHi() {
  console.log('Hi')
}

function sayBye() {
  console.log('bye')
}

export {sayHello, sayHi, sayBye as default}
```



## 3. 动态导入

### 3-1 静态导入

通过 `import ...` 语句用于导入由另一个模块导出的绑定，该语句是静态导入

`import` 语句只能在声明了 `type="module"` 的 `script` 的标签中使用

* 静态导入模块路径必须是原始类型字符串
* 无法根据条件或者在运行时导入

```js
import ... from getModuleName(); // error 路径必须是字符串
if(...) {
  import ...; // 不能条件判断执行
}
```



### 3-2 import动态导入

`import(module)` 表达式，它不需要依赖 `type="module"` 的 `script` 标签

该表达式加载模块并返回一个 `Promise`，该 `Promise` `resolve` 为一个包含其所有导出的模块对象。我们可以在代码中的任意位置调用这个表达式

静态与动态 `import` 优点：

* 按照一定的条件或者按需加载模块的时候，动态 `import()` 是非常有用的
* 静态型的 `import` 是初始化加载依赖项的最优选择，使用静态 `import` 更容易从代码静态分析工具和 `tree shaking` 中受益

```js
// import() 动态导入返回Promise resolve是一个包含所有导出模块的对象
const promise = import('./get.js')
promise.then((reslove, reject) => {
  console.log(reslove)
})

// 按需加载动态导入
if (true) {
  let result = import('./get.js')
    .then(res => {
      res.getName()
    })
}

async function importFunc() {
  let result = await import('./say.js')
  console.log(result)
  result.default()
}
```

```html
<button onclick="importFunc()">按需加载</button>
```



### 3-3 Webpack编译模块

创建文件夹 `bundle` 生成配置 `package.json`

```shell
npm init -y
```

修改 `package.json` 配置文件，添加打包命令

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "dev": "webpack --mode development --watch"
}
```

安装 `webpack` 工具包

```shell
npm i webpack webpack-cli --save-dev
```

目录结构

```
bundle文件夹
--dist打包压缩文件
--node_modules依赖包
--index.html
--src文件夹
----index.js入口文件
----style.js功能模块
```

`index.html` 内容

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>webpack打包编译</title>
  <script defer src="./dist/main.js"></script>
</head>

<body>

</body>

</html>
```

`index.js` 入口文件

```js
import Style from './style'
Style()
```

`style.js` 功能模块

```js
export default function () {
  document.body.style.backgroundColor = "#3496db"
}
```

执行打包

```shell
npm run dev
```



