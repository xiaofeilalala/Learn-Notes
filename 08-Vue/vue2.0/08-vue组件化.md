# vue组件化

## 1. 了解组件

### 1-1 组件化

组件 —— 把重复的代码提取出来合并成为一个个组件，组件最重要的就是重用（复用），位于框架最底层，其他功能都依赖于组件，可供不同功能使用，独立性强，，多个组件可以组合成组件库，方便调用和复用，组件间也可以嵌套，小组件组合成大组件

* 传统 `web` 应用开发方式都是采用插件化形式开发，一个页面引入多个依赖文件（`css` 文件、`js` 文件），依赖关系紊乱不好维护并且代码复用性不高
* `vue` 则实现了组件化开发，将页面拆分成多个组件，每个组件依赖的 `CSS`、`JS`、模板、图片等资源放在一起开发和维护，并且组件是独立的在系统内部可复用，组件和组件之间可以嵌套，可以极大简化代码量，并且对后期的需求变更和维护也更加友好



<img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220523195541.png" style="zoom:80%;" />



### 1-2 模块化

模块 —— 分属同一功能/业务的代码进行隔离（分装）成独立的模块，可以独立运行，以页面、功能或其他不同粒度划分程度不同的模块，位于业务框架层，模块间通过接口调用，目的是降低模块间的耦合，由之前的主应用与模块耦合，变为主应用与接口耦合，接口与模块耦合。独立的功能和项目（如淘宝：注册、登录、购物、直播...），可以调用组件来组成模块，多个模块可以组合成业务框架



<img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220523194830.png" style="zoom:90%;" />



### 1-3 组件化与模块化优点

* 开发和调试效率高 —— 随着功能越来越多，代码结构会越发复杂，要修改某一个小功能，可能要重新翻阅整个项目的代码，把所有相同的地方都修改一遍，重复劳动浪费时间和人力，效率低；使用组件化，每个相同的功能结构都调用同一个组件，只需要修改这个组件，即可全局修改
* 可维护性强 —— 便于后期代码查找和维护
* 避免阻断 —— 模块化是可以独立运行的，如果一个模块产生了 `bug`，不会影响其他模块的调用
* 版本管理更容易 —— 如果由多人协作开发，可以避免代码覆盖和冲突

<img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220524130712.png" style="zoom: 67%;" />



### 1-4 .extend构造器

`.extend` 是 `Vue` 中的构造器，用于创建一个子组件，参数是一个包含组件选项的对象

组件是可复用的 `Vue` 实例的，它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项

> `Vue.component` 注册组件时可以传入一个选项对象，并自动调用 `Vue.extend`。当使用 `component` 注册组件时，会默认去找传入的对象并执行 `extend` 方法，所以可以简化 `extend` 构造器写法，直接省略以对象形式书写

```html
<div id="app">
  <head-title></head-title>
</div>
```

```js
// Vue.extend组件构造器 —— 用于创建子组件，可传入与实例相同的options选项，除了el外
// 创建组件Vue.extend是一个VueComponent组件构造函数， title则为组件实例
// let headTitle = Vue.extend({
//   // template 组件模版
//   template: '<h3>{{title}}</h3>',
//   data() {
//     return {
//       title: 'Hello extend'
//     }
//   }
// })

// 可以省略Vue.extend 当注册组件时传入对象component会自动调用 Vue.extend
let headTitle = {
  // template 组件模版
  template: '<h3>{{title}}</h3>',
  data() {
    return {
      title: 'Hello extend'
    }
  }
}

const vm = new Vue({
  el: '#app',
  data: function() {
    return {

    }
  },
  // components 局部注册组件
  components: {
    headTitle
  }
})
```



### 1-5 组件data必须是函数

`Vue` 源码中判断了 `data` 是否是个函数，如果是函数，就会使用 `getData()` 方法执行以下，并将执行后的结果作为最后的值，否则呢，就会直接使用用户自己设置的 `data`，如果 `data`是一个对象，那么就会走源码中的 `else` 代码，直接将用户传入的 `data` 进行使用，如果有多个组件，就会造成 `data` 数据共享，这样会形成数据污染

* 为了使每一个组件的状态相互不干扰，不形成数据污染，`data` 必须是一个函数

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220523203044.png)



创建根实例的时候使用 `new` 的方式只能创建一个，是一个单例，不会像 `vue` 组件实例可以创建很多个，就不会发生 `vue` 组件实例中的数据污染，相互干扰

首先判断下是否是 `vue` 组件，那么执行 `mergeDataOrFn` 方法的时候就不会传第三个参数 `vm` 实例，所以就会进行 `function` 的校验，但是如果使根实例的时候，在执行 `mergeDataOrFn` 方法的时候传第三个参数，也就是 `vm` 实例，所以就会躲避了 `function` 的校验

* `Vue` 中根实例的 `data` 是没有限制的，可以是函数也可以是对象

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220523204546.png)



### 1-6 template选项

* `template` 选项 —— 一个字符串模板作为 `Vue` 实例的标识使用。模板将会替换挂载的元素，挂载元素的内容都将被忽略，除非模板的内容有分发插槽
  * 如果 `Vue` 实例中有 `template` 属性，会将该属性值进行编译，将编译后的虚拟 `DOM` 直接替换掉 `Vue` 实例绑定的元素
  
  * `template` 选项中的 `DOM` 结构只能有一个根元素，如果有多个根元素需要使用 `v-if`、`v-else`、`v-else-if` 设置成只显示其中一个根元素
  * 在该属性对应的属性值中可以使用 `Vue` 实例 `data`、`methods` 中定义的数据
  
* `el` 选项 —— 提供一个在页面上已存在的 `DOM` 元素作为 `Vue` 实例的挂载目标。可以是 `CSS` 选择器，也可以是一个 `HTMLElement` 实例，如果 `render` 函数和 `template` 属性都不存在，挂载 `DOM` 元素的 `HTML` 会被提取出来用作模板

> `el` 选项只在用 `new` 创建实例时生效

```html
<div id="app">
  <h3>Hello World</h3>
  <head-title></head-title>
</div>
```

```js
let headTitle = Vue.extend({
  // template 组件模版
  // template选项只能有一个根元素  多个根元素会报错
  // 可以在template中使用data methods computed等定义的数据
  template: '<h3>{{title}}</h3><p>Hello jsx</p>',
  data() {
    return {
      title: 'Hello extend'
    }
  }
})
const vm = new Vue({
  // el提供实例挂在的元素目标 可以是选择器或者元素对象
  el: '#app',
  // Vue实例中有template属性会替换掉Vue实例绑定的元素 挂载元素的内容都将被忽略
  // template: '<h3>{{title}}</h3>',
  data: function() {
    return {
      title: 'hello vue'
    }
  },
  components: {
    headTitle
  }
})
```



### 1-7 模版定义X-Template

模版定义 `X-Template` —— 定义模板的方式是在一个 `<script>` 元素中，并为其带上 `text/x-template` 的类型，然后通过一个 `id` 将模板引用过去

> `x-template` 需要定义在 `Vue` 所属的 `DOM` 元素外

```html
<div id="app">
  <title-head></title-head>
</div>
<script type="text/x-template" id="title-head">
  <h3>{{title}}</h3>
</script>
```

```js
let titleHead = Vue.extend({
  // template通过id属性引用模版
  template: '#title-head',
  data() {
    return {
      title: 'Hello Vue'
    }
  }
})
const vm = new Vue({
  el: '#app',
  data: function() {
    return {

    }
  },
  components: {
    titleHead
  }
})
```



### 1-8 模版定义template

通过 `<template>` 标签定义模版，`template` 标签不是 `Vue` 特有的，而是 `html` 中定义的，`template` 里的内容不会被浏览器渲染，通过 `id` 来确定组件的模版信息，`template` 标签必须在实例挂载的元素外

> 使用 `div` 也可以实现模版定义，在页面加载时会渲染出来，使用 `template` 标签时，在页面加载时不会渲染

```html
<div id="app">
  <title-head></title-head>
</div>
<!-- 通过template标签定义模版 template指定引入模版 -->
<!-- 必须在实例挂载的元素外 -->
<template id="title-head">
  <h3>{{title}}</h3>
</template>
```

```js
let titleHead = Vue.extend({
  // template通过id属性引用模版
  template: '#title-head',
  data() {
    return {
      title: 'Hello Vue'
    }
  }
})
const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  components: {
    titleHead
  }
})
```



## 2. 组件注册

### 2-1 组件名

`Vue-component` 与 `components` 注册组件始终需要给它一个名字，组件名避免和当前以及未来的 HTML 元素相冲突

```js
Vue.component('my-component-name', { /* ... */ })
```

* 使用 `kebab-case` 短横线分隔命名，必须在引用这个自定义元素时使用 `kebab-case` 形式
* 使用 `PascalCase` 首字母大写命名，在非字符串的模板中使用时只有 `kebab-case` 是有效的，`PascalCase` 需要在 `vue-cli` 支持下使用

```html
<!-- 组件书写形式 1. 首字母大写  2. 短横线分隔-->
<component-a></component-a>
<component-b></component-b>
<!-- 在非字符串模版下不能使用首字母大写情况 -->
<!-- <ComponentA></ComponentA> -->
```

```js
// 注册组件时需要定义组件名 定义的组件名不能是HTML中的元素
let componentA = Vue.extend({
  name: 'ComponentA',
  template: `<h3>{{message}}</h3>`,
  data() {
    return {
      message: 'hello componentA'
    }
  }
})

let componentB = Vue.extend({
  // 可以通过name属性指定在vue-devtools中组件名
  name: 'ComponentB',
  template: `<h3>{{message}}</h3>`,
  data() {
    return {
      message: 'hello componentB'
    }
  }
})

const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  components: {
    // components中注册组件
    // 属性名是组件的名称 属性值则是组件选项对象
    // 组件名书写形式 1. 首字母大写 2. 短横线分隔
    ComponentA: componentA,
    'component-b': componentB
  }
})
```



### 2-2 name属性

`name` 属性选项，组件在全局用 `Vue.component()` 注册时，全局 `ID` 自动作为组件的 `name`，指定 `name` 选项的另一个好处是便于调试，会在 `vue-devtools` 工具显示定义的 `name` 组件名

```js
let componentA = Vue.extend({
  name: 'ComponentA',
  template: `<h3>{{message}}</h3>`,
  data() {
    return {
      message: 'hello componentA'
    }
  }
})
```



### 2-2 全局注册

在 `Vue` 创建全局组件，通常先使用 `Vue.extend` 方法构建模版对象，再通过 `Vue.component` 方法来注册组件，并以组件名作为 `HTML` 标签来使用

`Vue.component` —— 注册或获取全局组件。注册还会自动使用给定的 `id` 设置组件的名称

> 全局注册的行为必须在根 `Vue` 实例 (通过 `new Vue`) 创建之前发生

```js
<!-- 全局注册的组件可以在任何组件中使用 -->
<component-a></component-a>
<component-b></component-b>
```

```js
// 全局注册通过Vue.component()
// 第一个参数是组件名name，第二个参数组件构造器
Vue.component('component-a', Vue.extend({
  name: 'ComponentA',
  // 全局注册的组件可以在任何组件中直接使用
  template: `<div>
        <h3>{{message}}</h3>
        <component-b></component-b>
      </div>`,
  data() {
    return {
      message: 'hello componentA'
    }
  }
}))

// 第二个参数是一个对象选项，默认会调用Vue.extend方法
Vue.component('component-b', Vue.extend({
  name: 'ComponentB',
  template: `<h3>{{message}}</h3>`,
  data() {
    return {
      message: 'hello componentB'
    }
  }
}))

// 传入组件名 获取注册的组件返回构造器
let componentA = Vue.component('component-a')
console.dir(componentA)
```



### 2-3 局部注册

通过在 `components` 选项中定义局部组件，`components` 对象中的每个属性，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象

当需要在一个组件中使用另一个局部组件时，需要通过 `components` 选项引入再使用，并且引入的局部组件要在创建当前组件的前面

> 局部注册的组件只能在当前作用域下使用，其子组件中不可用

```html
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
</div>
```

```js
let componentB = Vue.extend({
  name: 'ComponentB',
  template: `<h3>{{message}}</h3>`,
  data() {
    return {
      message: 'hello componentB'
    }
  }
})

let componentA = Vue.extend({
  name: 'ComponentA',
  // 局部注册的组件只能在当前注册的组件中使用
  // 要在其它组件使用需要通过components选项引入组件
  template: `<div>
        <h3>{{message}}</h3>
        <component-b></component-b>
      </div>`,
  data() {
    return {
      message: 'hello componentA'
    }
  },
  components: {
    componentB
  }
})

const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  // 局部注册通过components选项
  components: {
    ComponentA: componentA,
    'component-b': componentB
  }
})
```



### 2-4 模块化局部注册

通过 `import`/`require` 使用一个模块系统，通过 `import` 引入组件并在当前组件中 `components` 选项中注册为局部组件

```js
import ComponentA from './ComponentA.vue'
import ComponentB from './ComponentB.vue'

export default {
  components: {
    ComponentA,
    ComponentB
  }
}
```



### 2-5 模块化全局注册

在模块化中，组件在 `components` 文件目录下，并将每个组件放置在其各自的文件中，注册全局组件需要在应用入口文件 `src/main.js` 中全局导入

必须在 `new Vue` 创建实例前通过 `Vue.component` 注册全局组件

```js
// main.js
import Vue from 'vue'
import ComponentA from './ComponentA.vue'
import ComponentB from './ComponentB.vue'

Vue.component('ComponentA')
Vue.component('ComponentB')

new Vue({
  el: '#app',
  router,
  store,
  render: h => h(App)
})
```

```js
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // 其组件目录的相对路径
  './components',
  // 是否查询其子目录
  false,
  // 匹配基础组件文件名的正则表达式
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // 获取组件配置
  const componentConfig = requireComponent(fileName)

  // 获取组件的 PascalCase 命名
  const componentName = upperFirst(
    camelCase(
      // 获取和目录深度无关的文件名
      fileName
        .split('/')
        .pop()
        .replace(/\.\w+$/, '')
    )
  )

  // 全局注册组件
  Vue.component(
    componentName,
    // 如果这个组件选项是通过 `export default` 导出的，
    // 那么就会优先使用 `.default`，
    // 否则回退到使用模块的根。
    componentConfig.default || componentConfig
  )
})
```



## 3. 组件实例

### 3-1 VueComponent构造函数

* 组件是由 `Vue.extend` 构造器生成的 `VueComponent` 构造函数，每次调用会生成不同的构造函数
* 在使用定义的组件时，`Vue` 会模版解析组件，并将 `VueComponent` 构造函数实例化对象
* 由于 `VueComponent` 继承了 `Vue` 构造函数的所有原型方法，以及 `Vue.use` / `Vue.mixin` / `Vue.extend` / `Vue.component` / `Vue.directive` / `Vue.filter` 静态方法，在 `VueComponent` 实例的 `options` 选项中的 `this` 则指向 `VueComponent` 构造函数生成的实例对象

```js
Vue.extend = function(extendOptions) {
  extendOptions = extendOptions || {};
  var Super = this;
  var SuperId = Super.cid;
  var cachedCtors = extendOptions._Ctor || (extendOptions._Ctor = {});
  if (cachedCtors[SuperId]) {
    return cachedCtors[SuperId]
  }

  var name = extendOptions.name || Super.options.name;
  if (name) {
    validateComponentName(name);
  }
  
  // 通过将 VueComponent 的原型指向 Vue 构造函数的原型的方式继承 Vue 构造函数原型上所有的属性和方法
  var Sub = function VueComponent(options) {
    this._init(options);
  };
  Sub.prototype = Object.create(Super.prototype);
  Sub.prototype.constructor = Sub;
  Sub.cid = cid++;
  Sub.options = mergeOptions(
    Super.options,
    extendOptions
  );
  Sub['super'] = Super;

  // For props and computed properties, we define the proxy getters on
  // the Vue instances at extension time, on the extended prototype. This
  // avoids Object.defineProperty calls for each instance created.
  // 检查传入的 extendOptions 是否拥有 props 和 computed 属性，如果有就进行初始化
  if (Sub.options.props) {
    initProps$1(Sub);
  }
  if (Sub.options.computed) {
    initComputed$1(Sub);
  }

  // allow further extension/mixin/plugin usage
  Sub.extend = Super.extend;
  Sub.mixin = Super.mixin;
  Sub.use = Super.use;

  // create asset registers, so extended classes
  // can have their private assets too.
  ASSET_TYPES.forEach(function(type) {
    Sub[type] = Super[type];
  });
  // enable recursive self-lookup
  if (name) {
    Sub.options.components[name] = Sub;
  }

  // keep a reference to the super options at extension time.
  // later at instantiation we can check if Super's options have
  // been updated.
  Sub.superOptions = Super.options;
  Sub.extendOptions = extendOptions;
  Sub.sealedOptions = extend({}, Sub.options);

  // cache constructor
  cachedCtors[SuperId] = Sub;
  return Sub
}
```

* 新建一个 `VueComponent` 构造函数 命名为 `Sub`，通过将 `VueComponent` 的原型指向 `Vue` 构造函数的原型的方式继承 `Vue` 构造函数 原型上所有的属性和方法，再将 `Vue.use` / `Vue.mixin` / `Vue.extend` / `Vue.component` / `Vue.directive` / `Vue.filter` 方法定义给它，让 `VueComponent` 也拥有这些静态方法
* 接着检查传入的 `extendOptions` 是否拥有 `props` 和 `computed` 属性，如果有就进行初始化。如果检测出传入的 `extendOptions` 中含有 `name` 属性，则将其自动放入 `VueComponent` 的全局组件中 ( 不要和 `Vue` 的全局组件混淆 )
* 然后将 `Vue.options` 保存到 `VueComponent.superOptions` 属性中，将传入的 `extendOptions` 保存到 `VueComponent.extendOptions` 属性中，并将传入的 `extendOptions` 封存一份保存到 `VueComponent.sealedOptions` 中
* 最后将 `VueComponent` 返回，这就是继承了 `Vue` 构造函数 的 `Vue` 组件 的构造函数



### 3-2 组件实例与Vue实例

* 组件的构造函数是 `VueComponent`，模版解析组件时会自动执行 `new VueComponent(options)` 组件实例化对象
* `Vue` 的构造函数就是 `Vue`，通过手动 `new Vue` 实例化 `Vue`
* 组件是可复用的 `Vue` 实例，与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项

```html
<div id="app">
  <component-a></component-a>
</div>
```

```js
let componentA = Vue.extend({
  template: '<h1>{{message}}</h1>',
  data() {
    return {
      message: 'Hello Vue'
    }
  },
  mounted() {
    // 选项中的this指向Vuecomponent实例对象
    console.log(this)
  },
})

// 组件的实例
let component = new componentA()
console.log(component)

const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  methods: {
    getThis() {
      // 根Vue实例中this指向vm 也就是Vue实例
      console.log(this === vm); // true
    }
  },
  components: {
    componentA
  }
})
// vm是Vue实例对象
console.log(vm)
```



### 3-3 内置继承关系

组件 `Vuecomponent` 构造函数的原型继承了 `Vue` 构造函数的原型上的属性与方法，所以组件的实例对象可以与 `new Vue` 接收相同的选项

`Vuecomponent.prototype.__proto__ === Vue.prototype`



![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220524220744.png)



```html
<div id="app">
  <component-a></component-a>
</div>
```

```js
let componentA = Vue.extend({
  template: '<h1>{{message}}</h1>',
  data() {
    return {
      message: 'Hello Vue'
    }
  }
})

let component = new componentA()

const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  components: {
    componentA
  }
})

console.log(Vue)
console.log(componentA)
console.log(vm)
console.log(component)
// Vue构造函数的prototype 指向实例对象的原型
console.log(Vue.prototype === vm.__proto__) // true
// Vuecomponent构造函数的prototype 指向实例对象的原型
console.log(componentA.prototype === component.__proto__) // true
// Vue实例对象的原型的原型指向Object
console.log(vm.__proto__.__proto__ === Object.prototype) // true
// Vuecomponent实例原型的原型指向Vue实例对象的原型对象
console.log(component.__proto__.__proto__ === Object.prototype); // false
console.log(component.__proto__.__proto__ === vm.__proto__); // true
// null
console.log(vm.__proto__.__proto__.__proto__); // null
console.log(component.__proto__.__proto__.__proto__.__proto__); // null
```

