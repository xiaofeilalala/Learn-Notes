# 高级语法

## 1. Mixin混入

### 1-1 局部混入

`mixin` 混入用来分发 `Vue` 组件中的可复用功能，一个混入对象可以包含任意组件选项，当组件使用混入对象时，所有混入对象的选项将被混合进入该组件本身的选项

局部 `mixins` —— 通过 `mixins` 选项接受一个混入对象的数组，混入对象中选项会与组件中选项合并

如果你的混入包含一个 `created` 钩子，而创建组件本身也有一个，那么两个函数都会被调用

```vue
<div id="app">
  <h2>{{message}}</h2>
</div>
```

```js
// 定义局部mixin
const myMixin = {
  data() {
    return {
      message: '局部混入',
    }
  }
}
const vm = new Vue({
  el: '#app',
  // mixins属性局部混入
  mixins: [myMixin],
  data: function() {
    return {

    }
  },
  created() {
    console.log(this.$data)
  }
})
```



### 1-2 全局混入

全局 `mixin` —— 通过 `Vue.mixin` 全局注册，当使用全局混入时，它将影响每一个之后创建的 `Vue` 实例

> **Tips：**请谨慎使用全局混入，因为它会影响每个单独创建的 `Vue` 实例 (包括第三方组件)

```vue
<div id="app">
  <h2>{{message}}</h2>
  <my-component></my-component>
</div>

<!-- muComponent -->
<template id="my-component">
  <div>
    <h2>{{title}}</h2>
    <!-- 全局mixin混入，每个组件都可以访问到mixin定义的属性 -->
    <h2>{{message}}</h2>
  </div>
</template>
```

```js
// 定义全局混入
Vue.mixin({
  data() {
    return {
      message: '全局混入',
    }
  }
})

// 自定义组件
const myComponent = Vue.extend({
  // 引入template模版
  template: '#my-component',
  data() {
    return {
      title: '自定义组件'
    }
  }
})

// Vue实例
const vm = new Vue({
  el: '#app',
  data: function() {
    return {

    }
  },
  created() {
    console.log(this.$data)
  },
  // 局部注册组件
  components: {
    myComponent
  }
})
```



### 1-3 默认合并策略

`Vue` 针对每个规定的选项都有定义好的合并策略，例如 `data`，`component`，`mounted` 等。如果合并的子父配置都具有相同的选项，则只需要按照规定好的策略进行选项合并即可（混入的方法会被组件的方法覆盖掉，同名的钩子函数合并为一个数组，执行的顺序是先混入钩子在自身钩子）

由于 `Vue` 传递的选项是开放式的，所有也存在传递的选项没有自定义选项的情况，这时候由于选项不存在默认的合并策略，所以处理的原则是有子类配置选项则默认使用子类配置选项，没有则选择父类配置选项

> - `mixin` 很容易发生冲突：因为每个特性的属性都被合并到同一个组件中，所以为了避免属性名冲突和调试，你仍然需要了解其他每个特性
> - 可重用性是有限的：我们不能向 `mixin` 传递任何参数来改变它的逻辑，这降低了它们在抽象逻辑方面的灵活性

`Vue` 定义的默认自定义选项合并策略：

* 声明周期函数 —— 相同的选项将合并成数组，优先执行 `mixin` 混入中的生命周期函数，再执行组件内定义的生命周期函数
* `data` 选项 —— 组件中的 `data` 数据会覆盖 `mixin` 中数据
* `methods` 选项 —— 组件中的 `methods` 方法会覆盖 `mixin` 中的定义的 `methods`
* `watch` 选项 —— 相同的选项将合并成数组，优先执行 `mixin` 混入中的 `watch` 属性，再执行组件内定义的 `watch` 属性（`watch` 选项最终在合并的数组中可以是包含选项的对象，也可以是对应的回调函数，或者方法名）
* `props`, `methods`, `inject`, `computed` 选项 —— 组件中的 `props`, `methods`, `inject`, `computed` 选项会覆盖 `mixin` 中定义的

```vue
<div id="app">
  <h2>{{message}}</h2>
  <button @click="sayHello">say hello</button>
  <h2>{{getValue}}</h2>
  <button @click="changeMessage">改变message值</button>
</div>
```

```js
// 局部mixins定义
const myMixin = {
  // data选项
  data() {
    return {
      message: 'jsx'
    }
  },
  // methods选项
  methods: {
    sayHello() {
      consokle.log('say Hello Mixins')
    },
    changeMessage() {
      this.message = 'jsx-ljj'
    }
  },
  // 生命周期函数
  mounted() {
    console.log('mounted Mixins')
  },
  // 计算属性
  computed: {
    getValue() {
      return this.message + ' computed'
    }
  },
  // 监听器
  watch: {
    message: function(oldValue, newValue) {
      console.log('mixins', oldValue, newValue)
    }
  }
}
// Vue实例
const vm = new Vue({
  el: '#app',
  mixins: [myMixin],
  // data选项合并，组件中的data属性会覆盖mixin混入中定义的data
  data: function() {
    return {
      message: 'ljj'
    }
  },
  methods: {
    // methods 合并策略：组件同名methods方法覆盖mixins同名方法
    sayHello() {
      console.log('say Hello Vue')
    }
  },
  created() {
    // data 合并策略：组件同名data属性覆盖mixins同名属性
    console.log(this.message)
  },
  // 声明周期合并策略：相同的选项将合并成数组，优先执行 `mixin` 混入中的生命周期函数，再执行组件内定义的生命周期函数
  mounted() {
    console.log('mounted Vue')
  },
  // 计算属性合并策略：组件同名computed属性覆盖mixins同名属性
  computed: {
    getValue() {
      return this.message + ' computed'
    }
  },
  // 监听器属性合并策略：相同的选项将合并成数组，优先执行 mixin 混入中的 watch 属性，再执行组件内定义的 watch 属性
  watch: {
    message: function(value, oldValue) {
      console.log('Vue', value, oldValue)
    }
  }
})
```



### 1-4 自定义选项合并

在 `mixins` 混入中，`Vue` 中大部分选项都有自己定义好的合并策略，自定义选项则使用默认策略，即简单地覆盖已有值

如果想让某个自定义选项以自定义逻辑进行合并，可以在 `Vue.config.optionMergeStrategies` 中添加一个函数

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208252052112.png)

```js
Vue.config.optionMergeStrategies._my_option = function (parent, child, vm) {
  return ...
}
```



合并策略选项分别接收在父实例和子实例上定义的该选项的值作为第一个和第二个参数，`Vue` 实例上下文被作为第三个参数传入

```js
// vue中mixins混入有默认的合并选项方法，通过Vue.config.optionMergeStrategies查看
console.log(Vue.config.optionMergeStrategies)
// 自定义选项将使用默认策略，即简单地覆盖已有值
// 定义自定义选项合并策略
Vue.config.optionMergeStrategies.myOptions = function(parent, children) {
  console.log(parent, children)
  // => undefined 'jsx'
  // => jsx ljj
  // 合并后返回子实例中定义的选项
  return parent || children
}

// 定义局部mixin
const myMixin = {
  data() {
    return {
      message: 'hello world'
    }
  },
  // 自定义选项myOptions
  myOptions: 'jsx'
}
const vm = new Vue({
  el: '#app',
  mixins: [myMixin],
  data() {
    return {
      message: 'Hello world Vue'
    }
  },
  myOptions: 'ljj',
  created() {
    // 自定义选项使用默认合并策略
    console.log(this.$options.myOptions)
  }
})
```



自定义选项合并可以使用 `Vue` 默认定义好的合并策略，对于多数值为对象的选项，可以使用与 `methods` 相同的合并策略

```js
const strategies = Vue.config.optionMergeStrategies
strategies.myOption = strategies.methods
```



## 2. 插件使用

插件通常用来为 `Vue` 添加全局功能。插件的功能范围没有严格的限制:

1. 添加全局方法或者 `property` 属性
2. 添加全局资源：指令/过滤器/过渡等
3. 通过全局混入来添加一些组件选项
4. 添加 Vue 实例方法，通过把它们添加到 `Vue.prototype` 上实现
5. 一个库，提供自己的 `API`，同时提供上面提到的一个或多个功能



### 2-1 Vue.use()

通过全局方法 `Vue.use()` 使用插件，需要在你调用 `new Vue()` 启动应用之前完成

* 可以传入一个可选的选项对象，也可依次传入多个参数，然后 `install` 函数来接收
* 会自动阻止多次注册相同插件，届时即使多次调用也只会注册一次该插件

```js
// 调用 MyPlugin.install(Vue)
Vue.use(MyPlugin)

new Vue({
  // ...组件选项
})
```

```js
const myPlugin = {
  install: function(Vue, options) {
    // 第一个参数表示Vue实例
    // 第二个参数表示可选参数
    console.log(options)
    // 1.添加全局属性与方法
    Vue.sayBay = 'bay'
    Vue.sayHello = function() {
      console.log('hello')
    }
  }
}
// Vue.use可以传入可选参数，可传入多个参数install函数来接收
Vue.use(myPlugin, {
  name: 'jsx'
})
// 多次重复注册插件，只会注册一次该插件
Vue.use(myPlugin, {
  name: 'jsx'
})
// 为Vue实例对象添加全局属性与方法
Vue.sayHello()
console.log(Vue.sayBay)
const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  }
})
```



### 2-2 开发插件

`Vue` 插件其实就是一个 `js` 对象，然后这个插件对象有一个公开方法 `install ` 这个方法的第一个参数是 `Vue` 构造器，第二个参数是一个可选的选项对象

* 添加全局属性与方法
* 添加全局资源
* 混入组件选项
* 添加实例方法

```js
Vue.myGlobMethod = function () {} // 添加全局方法或者属性	
Vue.directive() // 添加全局指令
Vue.mixin() // 添加混入		
Vue.prototype.$xxx = function () {} // 添加实例方法
Vue.component()	// 注册全局组件	
Vue.filters() // 注册过滤                   
```

```vue
<div id="app">
  <!-- 使用插件中定义的指令 -->
  <h3 v-texts="message"></h3>

  <!-- 使用插件中定义的过滤器 -->
  <h3>{{title | my-filter}}</h3>

  <!-- 使用插件中定义的mixin -->
  <h3>{{name}}</h3>

  <!--  调用插件中定义的实例方法与属性 -->
  <button @click="$sayBye">实例方法与属性</button>
    
  <!-- 使用插件全局注册组件 -->
  <component-a></component-a>
</div>
```

```js
// 自定义插件
const myPlugin = {
  install: function(Vue) {
    // 全局方法与属性
    Vue.sayHello = function() {
      console.log('Hello')
    }
    // 自定义指令
    Vue.directive('texts', {
      // 指令第一次绑定元素时触发
      bind(el, binding) {
        console.log(binding)
        el.innerText = binding.value
      },
      // 组件虚拟DOM更新时调用
      update(el, binding) {
        el.innerText = binding.value
      }
    })
    // 过滤器
    Vue.filter('my-filter', function(value) {
      console.log(value)
      return value + '-自定义插件'
    })
    // 混入mixin
    Vue.mixin({
      data() {
        return {
          name: 'jsx'
        }
      }
    })
    // 实例属性与方法
    Vue.prototype.$sayBye = function() {
      console.log('bye')
    }
    // 注册全局组件
    Vue.component('component-a', {
      template: '<h3>{{message}}</h3>',
      data() {
        return {
          message: '全局组件'
        }
      }
    })
  }
}
// 注册插件
Vue.use(myPlugin)
// Vue定义的全局方法与属性
Vue.sayHello()
const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      message: '自定义插件',
      title: '过滤器'
    }
  }
})
```



## 3. render渲染函数

### 3-1 vue整体流程

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202209061047873.webp)



`Vue` 应用运行整体流程 —— 先模板通过编译生成 `AST`，再由 `AST` 生成 `Vue` 的 `render` 函数（渲染函数），渲染函数结合数据生成 `Virtual DOM` 树(虚拟 `DOM`)，`Diff` 和 `Patch` 后生成新的 `UI` 视图

> `render` 函数的左边可以称之为编译期，将 `Vue` 的模板转换为渲染函数。`render` 函数的右边是 `Vue` 的运行时，主要是基于渲染函数生成 `Virtual DOM` 树，`Diff` 和 `Patch`

* 模板：`Vue` 的模板基于纯 `HTML`，基于 `Vue` 的模板语法，我们可以比较方便地声明数据和 `UI` 视图的关系
* `AST`：`AST`是 `Abstract Syntax Tree` 的简称，`Vue`使用 `HTML` 的 `Parser` 将 `HTML` 模板解析为 `AST`，并且对 `AST` 进行一些优化的标记处理，提取最大的静态树，方便 `Virtual DOM` 时直接跳过 `Diff`
* 渲染函数：渲染函数是用来生成 `Virtual DOM` 的。`Vue` 推荐使用模板来构建我们的应用界面，在底层实现中 `Vue` 会将模板编译成渲染函数，当然我们也可以不写模板，直接写渲染函数，以获得更好的控制
* `Virtual DOM`：虚拟 `DOM` 树，`Vue` 的 `Virtual DOM Patching` 算法是基于**[Snabbdom](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fsnabbdom%2Fsnabbdom)**的实现，并在些基础上作了很多的调整和改进
* `Watcher`：每个Vue组件都有一个对应的 `watcher`，这个 `watcher` 将会在组件 `render` 的时候收集组件所依赖的数据，并在依赖有更新的时候，触发组件重新渲染。你根本不需要写 `shouldComponentUpdate`，`Vue` 会自动优化并更新要更新的 `UI` 视图

### 3-2 render函数

`Vue` 推荐在绝大多数情况下使用模板 `template` 来创建 `HTML`，特殊场景中也可以通过 `render` 渲染函数使用 `javascript` 来完成构建 `DOM`

> `render` 函数即渲染函数，它接收一个 `createElement` 方法作为第一个参数用来创建 `VNode`

* 使用 `template` 模版构建 `HTML`，`Vue` 在运行时会先对模版进行编译，再通过 `render` 函数结合数据生成虚拟 `DOM` 
* 使用 `render` 渲染函数构建，直接生成虚拟 `DOM` ，免去了模版编译的过程，大大提升了运行速度

```js
/*
* render: 渲染函数
* 参数: createElement
* 参数类型: Function
*/
render: function (createElement) {}
```

```js
// 模版template创建组件
Vue.component('my-component-a', {
  template: '<h3>{{message}}</h3>',
  data() {
    return {
      message: '组件A'
    }
  }
})

// render函数创建组件
Vue.component('my-component-b', {
  render: function(createElement) {
    return createElement('h3', this.message)
  },
  data() {
    return {
      message: '组件B'
    }
  }
})
```



### 3-3 createElement函数

`createElement` 函数是 `render` 函数的参数，接收三个参数，返回的是 `VNode` 虚拟节点 —— 告诉 `Vue` 页面上需要渲染什么样的节点，包括及其子节点的描述信息

> `Vue` 通过建立一个虚拟 `DOM` 来追踪自己要如何改变真实 `DOM`



`createElement` 第一个参数是必填的，可以是 `String | Object | Function`

* `String` —— 表示的是 `HTML` 标签名
* `Object` —— 一个含有数据的组件选项对象
* `Function` —— 返回了一个含有标签名或者组件选项对象的 `async` 函数

```
```



`createElement` 第二个参数是选填的，一个与模板中属性对应的数据对象

* `class` —— 类名，接受一个字符串、对象或字符串和对象组成的数组
* `style` —— 样式，接受一个字符串、对象，或对象组成的数组
* `attrs` —— 普通的 `HTML attribute`，例如 `HTML` 标签中 `src`, `id` 属性等
* `props` —— 组件 `props`
* `domProps` —— 原生的 `DOM` 属性
* `on` —— 事件监听器，不支持按键修饰符
* `navtiveOn` —— 仅用于组件，用于监听原生事件
* `directives` —— 自定义指令
* `scopedSlots` ——  作用域插槽
* `slot` —— 如果组件是其它组件的子组件，需为插槽指定名称
* `key` —— 唯一标识
* `ref` —— 给元素或子组件注册引用信息
* `refInFor` —— 多元素相同 `ref` 名，`$refs.ref` 为数组

```
```



`createElement` 第三个参数是选填的，代表子级虚拟节点 `VNodes`，由 `createElement()` 构建而成，正常来讲接收的是一个字符串或者一个数组，一般数组用的比较多（可以使用字符串来生成文本虚拟节点）

```
```



### 3-4 重复渲染





## 4. 代替模版功能

### 4-1 v-if和v-for



### 4-2 v-model



### 4-3 事件&按键修饰符



### 4-4 插槽



## 5. 函数式组件

### 5-1 使用函数式组件



### 5-2 传递属性与事件

