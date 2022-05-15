# Vue 起步

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220422140406.png)

## 1. Vue简介

### 1-1 什么是Vue?

> `Vue` (读音 `/vjuː/`，类似于 `view`) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，`Vue` 被设计为可以自底向上逐层应用。`Vue` 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，`Vue` 也完全能够为复杂的单页应用提供驱动

`Vue` 是一个用于创建用户界面的开源 `JavaScript` 框架，也是一个创建单页应用的 `Web` 应用框架

`Vue` 所关注的核心是 `MVC` 模式中的视图层，同时，它也能方便地获取数据更新，并通过组件内部特定的方法实现视图与模型的交互

`Vue` 并没有完全遵循 `MVVM` 模型，但是 `Vue` 的设计也受到了它的启发，通过尽可能简单的 `API` 实现响应的数据绑定和组合的视图组件

* 解耦视图与数据
* 响应式和组件化
* 前端路由
* 状态管理
* 双向绑定
* 虚拟 `DOM`



### 1-2 Vue优点

* `Vue` 是一个轻量级框架。`Vue` 的体积只有几十 `kb`，非常轻量
* `Vue` 简单易学，对新手友好度高
* 双向数据绑定。`Vue` 提供了双向数据绑定 `v-model` 的语法糖，让我们可以避免 `DOM` 操作
* 组件化开发。我们可以把页面拆分成大大小小的组件，这样大大提高了代码的可复用率和可读性



### 1-3 Vue与React对比

`React` 和 `Vue` 都是 `MVVM` 框架，它们之间有很多相似之处：

* 两者都是用于 `web` 开发的 `JavaScript` 库
* 两者的使用都快速轻便
* 两者都是基础组件式的开发
* 两者都使用了虚拟 `DOM`

`React` 和 `Vue` 在某些方面也存在一定的差异：

- `Vue` 的数据可变的，通过对每一个属性建立 `Watcher` 来监听，当属性变化的时候，响应式的更新对应的虚拟 `DOM`，而 `React` 则是基于数据不可变，`React` 需要通过 `setState` 来触发渲染流程，同时可以通过 `shouldComponentUpdate` 来控制视图是否更新
- `Vue` 推荐使用模板语法，把 `html`、`css`、`js` 组合到一起，用各自的处理方式，通过模板引擎来处理。，而 `React` 则推荐使用 `JSX` 语法进行书写，`React` 的思路是 `all in js`，通过 `js` 生成 `html`
- `React` 中的 `state` 对象是不可变的，我们不能被直接改变 `state` 的值，而是需要通过使用 `setState()` 的方法去更新状态，在 `Vue` 中，`state` 并不是必须的，数据由 `data` 属性进行管理，我们可以直接修改 `data` 属性中的值



## 2. MVVM 模型框架

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220511195110.png)





### 2-1 什么是MVVM?

`MVVM` 分为 `Model`、`View`、`ViewModel`：

- `Model` 代表数据模型，数据和业务逻辑都在 `Model` 层中定义
- `View` 代表 `UI` 视图，负责数据的展示
- `ViewModel` 负责监听 `Model` 中数据的改变并且控制视图的更新，处理用户交互操作

`Model` 和 `View` 并无直接关联，而是通过 `ViewModel` 来进行联系的，`Model` 和 `ViewModel` 之间实现了双向数据绑定，当 `Model` 中的数据改变时会触发 `View` 层的刷新，`View` 中由于用户交互操作而改变的数据也会在 `Model` 中同步

这种模式实现了 `Model` 和 `View` 的数据自动同步，因此开发者只需要专注于数据的维护操作即可，而不需要自己操作 `DOM`



### 2-2 MVVM的优点

* 分离视图（`View`）和模型（`Model`），降低代码耦合，提⾼视图或者逻辑的重⽤性: ⽐如视图（`View`）可以独⽴于 `Model` 变化和修改，⼀个 `ViewModel` 可以绑定不同的 `View`上，当 `View` 变化的时候 `Model` 不可以不变，当 `Model` 变化的时候 `View` 也可以不变。你可以把⼀些视图逻辑放在⼀个`ViewModel` ⾥⾯，让很多 `view` 重⽤这段视图逻辑
* 提⾼可测试性: `ViewModel` 的存在可以帮助开发者更好地编写测试代码
* ⾃动更新 `DOM`: 利⽤双向绑定，数据更新后视图⾃动更新，让开发者从繁琐的⼿动 `DOM` 中解放



## 3. 单页面应用

`SPA（ single-page application ）`仅在 `Web` 页面初始化时加载相应的 `HTML`、`JavaScript` 和 `CSS`。一旦页面加载完成，`SPA` 不会因为用户的操作而进行页面的重新加载或跳转；取而代之的是利用路由机制实现 `HTML` 内容的变换，`UI` 与用户的交互，避免页面的重新加载

单页面应用优点：

- 用户体验好、快，内容的改变不需要重新加载整个页面，避免了不必要的跳转和重复渲染
- 基于上面一点，`SPA` 相对对服务器压力小
- 前后端职责分离，架构清晰，前端进行交互逻辑，后端负责数据处理

单页面应用缺点：

- 初次加载耗时多：为实现单页 `Web` 应用功能及显示效果，需要在加载页面的时候将 `JavaScript`、`CSS` 统一加载，部分页面按需加载
- 前进后退路由管理：由于单页应用在一个页面中显示所有的内容，所以不能使用浏览器的前进后退功能，所有的页面切换需要自己建立堆栈管理
- `SEO` 难度较大：由于所有的内容都在一个页面中动态替换显示，所以在 `SEO` 上其有着天然的弱势



## 4. 虚拟DOM

### 4-1 什么是虚拟DOM?

从本质上来说，`Virtual Dom` 是一个 `JavaScript` 对象，通过对象的方式来表示 `DOM` 结构。将页面的状态抽象为 `JS` 对象的形式，配合不同的渲染工具，使跨平台渲染成为可能。通过事务处理机制，将多次 `DOM` 修改的结果一次性的更新到页面上，从而有效的减少页面渲染的次数，减少修改 `DOM` 的重绘重排次数，提高渲染性能



虚拟 `DOM` 是对 `DOM` 的抽象，这个对象是更加轻量级的对 `DOM` 的描述。它设计的最初目的，就是更好的跨平台，比如 `Node.js` 就没有 `DOM`，如果想实现`SSR`，那么一个方式就是借助虚拟 `DOM`，因为虚拟 `DOM` 本身是 `js` 对象。 在代码渲染到页面之前，`vue` 会把代码转换成一个对象（虚拟 `DOM`）。以对象的形式来描述真实 `DOM` 结构，最终渲染到页面。在每次数据发生变化前，虚拟 `DOM` 都会缓存一份，变化之时，现在的虚拟 `DOM` 会与缓存的虚拟 `DOM` 进行比较。在 `vue` 内部封装了 `diff` 算法，通过这个算法来进行比较，渲染时修改改变的变化，原先没有发生改变的通过原先的数据进行渲染



另外现代前端框架的一个基本要求就是无须手动操作 `DOM`，一方面是因为手动操作 `DOM` 无法保证程序性能，多人协作的项目中如果 `review` 不严格，可能会有开发者写出性能较低的代码，另一方面更重要的是省略手动 `DOM` 操作可以大大提高开发效率



### 4-2 虚拟DOM的解析过程

虚拟 `DOM` 的解析过程：

- 首先对将要插入到文档中的 `DOM` 树结构进行分析，使用 `js` 对象将其表示出来，比如一个元素对象，包含 `TagName`、`props` 和 `Children` 这些属性。然后将这个 `js` 对象树给保存下来，最后再将 `DOM` 片段插入到文档中
- 当页面的状态发生改变，需要对页面的 `DOM` 的结构进行调整的时候，首先根据变更的状态，重新构建起一棵对象树，然后将这棵新的对象树和旧的对象树进行比较，记录下两棵树的的差异
- 最后将记录的有差异的地方应用到真正的 `DOM` 树中去，这样视图就更新了



### 4-3 为什么要用虚拟DOM

* 虚拟 `DOM` 可以解决 `MVVM` 框架视图和状态同步问题
* 虚拟 `DOM` 可以维护程序的状态，跟踪上一次的状态通过比较前后两次状态差异更新真实 `DOM`
* 跨平台 `Virtual DOM` 本质上是 `JavaScript` 的对象，它可以很方便的跨平台操作，比如服务端渲染、`uniapp`等

页面渲染的流程：解析`HTML` -> 生成 `DOM` -> 生成 `CSSOM` -> `Layout` -> `Paint` -> `Compiler`

对比一下修改 `DOM` 时真实 `DOM` 和 `Virtual DOM` 虚拟 `DOM` 操作时的过程，重排重绘的性能消耗∶

- 真实 `DOM`∶ 生成 `HTML` 字符串＋重建所有的 `DOM` 元素
- 虚拟 `DOM`∶ 生成 `vNode`+ `DOMDiff`＋必要的 `DOM` 更新



### 4-4 diff算法

* 当组件创建和更新时，`Vue` 会执行内部的 `update` 函数，该函数使用 `render` 函数生成的虚拟 `DOM` 树，将新旧两树进行对比，找到差异点，最终更新到真实 `DOM` 
* 对比差异的过程叫 `diff`，`Vue` 在内部通过一个叫 `patch` 的函数完成该过程 
* 在对比时，`Vue` 采用深度优先、同级比较的方式进行比对。同级比较就是说它不会跨越结构进行比较 在判断两个节点是否相同时，`Vue` 是通过虚拟节点的`key` 和 `tag` 来进行判断的 
* 具体来说，首先对根节点进行对比，如果相同则将旧节点关联的真实 `DOM` 的引用挂到新节点上，然后根据需要更新属性到真实 `DOM`，然后再对比其子节点数组；如果不相同，则按照新节点的信息递归创建所有真实 `DOM`，同时挂到对应虚拟节点上，然后移除掉旧的 `DOM` 
* 在对比其子节点数组时，`Vue` 对每个子节点数组使用了两个指针，分别指向头尾，然后不断向中间靠拢来进行对比，这样做的目的是尽量复用真实 `DOM`，尽量少的销毁和创建真实 `DOM`。如果发现相同，则进入和根节点一样的对比流程，如果发现不同，则移动真实 `DOM` 到合适的位置
* 这样一直递归的遍历下去，直到整棵树完成对比

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220511225215.png)



## 4. Vue安装引入

> **Tips：**`Vue` 不支持 `IE8` 及以下版本，因为 `Vue` 使用了 `IE8` 无法模拟的 `ECMAScript 5` 特性，但它支持所有兼容 `ECMAScript 5` 的浏览器

### 4-1 script引入

直接下载并用 `<script>` 标签引入，`Vue` 会被注册为一个全局变量

* 开发版本 —— 包含完整的警告和调试模式
* 生产版本 —— 删除了警告，`33.46KB min+gzip`

```html
<!-- 开发版本 -->
<script src="./config/vue.js"></script>

<!-- 生产版本 -->
<script src="./config/vue.min.js"></script>
```



### 4-2 CDN引入

* 对于制作原型或学习，可以使用最新版本
* 对于生产环境，推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的问题
* 支持 `ES6` 模块化引入

```html
<!-- 学习制作可以使用最新版本 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>

<!-- 生产环境建议使用明确版本号的构建文件 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.min.js"></script>

<!-- ES6 模块化引入 -->
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.esm.browser.js'
</script>
```



### 4-3 NPM 引入

`npm`（`Node 包管理器`）是 `JavaScript` 运行时 `Node.js` 的默认程序包管理器，可以通过 `npm` 管理项目依赖的下载

`npm` 由两个主要部分组成:

- 用于发布和下载程序包的 `CLI`（命令行界面）工具
- 托管 `JavaScript` 程序包的在线存储库

```shell
# 最新稳定版
npm install vue
```



### 4-4 vue-cli

`Vue` 提供了一个官方的 `CLI`，为单页面应用 (`SPA`) 快速搭建繁杂的脚手架

* `CLI` 插件是向你的 `Vue` 项目提供可选功能的 `npm` 包

* 为前端工作流提供了开箱即用的构建设置，热重载、保存时 `lint` 校验，以及生产环境可用的构建版本

```shell
npm install -g @vue/cli
```



### 4-5 不同构建版本

| 版本                     | UMD                | CommonJS              | ES Module(基于构建工具使用) | ES Module(直接用于浏览器) |
| ------------------------ | :----------------- | :-------------------- | :-------------------------- | :------------------------ |
| 完整版                   | vue.js             | vue.common.js         | vue.esm.js                  | vue.esm.browser.js        |
| 只包含运行时版           | vue.runtime.js     | vue.runtime.common.js | vue.runtime.esm.js          | -                         |
| 完整版(生产环境)         | vue.min.js         | -                     | -                           | vue.esm.browser.min.js    |
| 只包含运行时版(生产环境) | vue.runtime.min.js | -                     | -                           | -                         |

- 完整版 —— 同时包含编译器和运行时的版本
- 编译器 —— 用来将模板字符串编译成为 `JavaScript` 渲染函数的代码
- 运行时 —— 用来创建 `Vue` 实例、渲染并处理虚拟 `DOM` 等的代码。基本上就是除去编译器的其它一切
- `UMD`：`UMD` 版本可以通过 `<script>` 标签直接用在浏览器中
- `CommonJS`：`CommonJS` 版本用来配合老的打包工具比如 `Browserify` 或 `webpack 1`
- `ES Module`：从 `2.6` 开始 `Vue` 会提供两个 `ES Modules (ESM)` 构建文件
  - 为打包工具提供的 `ESM` —— 为诸如 `webpack 2` 或 `Rollup` 提供的现代打包工具，可以让打包工具进行 `tree-shaking` 并将用不到的代码排除出最终的包
  - 为浏览器提供的 `ESM` (2.6+) —— 用于在现代浏览器中通过 `<script type="module">` 直接导入

```html
<!-- CommonJS完整版开发环境 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.common.dev.js
<!-- CommonJS完整版 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.common.js
<!-- CommonJS完整版生产版 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.common.prod.js
<!-- CommonJS 运行时版 开发环境  -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.runtime.common.dev.js
<!-- CommonJS 运行时版  -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.runtime.common.js
<!-- CommonJS 运行时版 生产环境  -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.runtime.common.prod.js


<!-- ES Module基于浏览器 完整版  -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.esm.browser.js
<!-- ES Module基于浏览器 完整版 生产环境  -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.esm.browser.min.js

  
<!-- ES Module基于构建工具 完整版  -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.esm.js
<!-- ES Module基于构建工具 运行时版 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.runtime.esm.js

  
<!-- UMD 完整版 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"
<!-- UMD 完整版 生产环境 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.min.js
<!-- UMD 运行时版 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.runtime.js
<!-- UMD 运行时版 生产环境 -->
https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.runtime.min.js
```

