# vue实例与模版语法

## 1. Vue实例

### 1-1 创建实例

可以通过使用 `Vue` 函数 `new` 关键字实例化创建一个新的 `Vue` 实例

一个 `Vue` 应用由一个通过 `new Vue` 创建的根 `Vue` 实例以及可选的、嵌套的、可复用的组件树组成

```js
const vm = new Vue({
    // options选项
})
```



### 1-2 options选项

当创建一个 `Vue` 实例时，你可以传入一个 `options` 选项对象

* `el` —— 用于指定一个页面中已经存在的 `DOM` 元素来挂在 `Vue` 实例，可以是 `HTMLElement` 和 `CSS` 选择器
* `data` —— 声明应用内需要双向绑定的数据
  * 它将 `data` 对象中的所有的属性加入到 `Vue` 的响应式系统中，当这些属性的值发生改变时，视图将会产生响应，即匹配更新为新的值
  * 只有当实例被创建时就已经存在于 `data` 中的属性才是响应式的
  * `Object.freeze()` 可以冻结一个对象，这个被冻结的对象将不能被修改（不能添加新属性，不能删除已有属性，不能修改对象的特性描述可枚举、可配置、可写性、属性值，不能修改原型） 

* `methods` —— 用于定义 `Vue` 实例对象的属性方法，可以直接通过 `VM` 实例访问这些方法，或者在指令表达式中使用。方法中的 `this` 自动绑定为 `Vue `实例

```html
<div id="app">
  <h1>{{title}}</h1>
  <h1>{{lesson.name}}</h1>
  <button type="button" @click="changeValue">修改内容</button>
</div>

<script src="../vue.js"></script>

<script>
  let obj = {
    name: 'vue'
  }
  // Object.freeze()可以冻结一个对象
  Object.freeze(obj)
  // 通过Vue函数实例化对象
  let vm = new Vue({
    // el 用于将Vue实例挂在页面中的DOM元素 可以是HtmlElement和css选择器
    el: '#app',
    // data声明应用内需要双向绑定的数据
    // 这些属性的值发生改变时，视图将会产生响应，即匹配更新为新的值
    // 只有当实例被创建时就已经存在于data中的属性才是响应式的
    data: function () {
      return {
        title: 'jsx',
        lesson: obj 
      }
    },
    // methods用于定义Vue实例属性方法，可以通过vm实例访问
    methods: {
      // 方法中的this为Vue实例
      changeValue() {
        this.title = 'Hello Vue',
        this.lesson.name = 'Vue options选项'
      }
    }
  })
</script>
```





### 1-3 el与data选项



### 1-4 vue实例属性

`Vue` 实例还暴露了一些有用的实例属性与方法。它们都有前缀 `$`，以便与开发者定义的属性区分开来

* `$data` —— `Vue` 实例定义的数据对象
* `$props` —— 当前组件接收到的 `props` 对象
* `$el` —— Vue 实例使用的根 DOM 元素
* `$options` —— 用于当前 Vue 实例的初始化选项
* `$parent` —— 父实例，如果当前实例有的话
* `$root` —— 当前组件树的根 `Vue` 实例，如果当前实例没有父实例，此实例将会是其自己
* `$children` —— 当前实例的直接子组件
* `$slots` —— 用来访问被插槽分发的内容
* `$scopedSlots` —— 用来访问作用域插槽
* `$refs` —— 持有注册过 `ref` 属性的所有 `DOM` 元素和组件实例
* `$isServer` —— 当前 `Vue` 实例是否运行于服务器
* `$attrs` —— 包含了父作用域中不作为 `prop` 被识别且获取的属性绑定 (`class` 和 `style` 除外)
* `$listeners` —— 包含了父作用域中的不含 `.native` 修饰器的 `v-on` 事件监听器

```html
<div id="app"></div>

<script src="../vue.js"></script>

<script>
  // 通过Vue函数实例化对象
  let vm = new Vue({
    // options选项
    el: '#app',
    data: function () {
      return {
        title: 'Hello Vue',
        lesson: {
          name: 'Vue'
        }
      }
    },
    mounted() {
      // Vue实例定义的数据对象
      console.log(this.$data);
      // Vue实例挂在的DOM元素
      console.log(this.$el)
      // Vue实例初始化options
      console.log(this.$options)
      // 父实例
      console.log(this.$parent); // undefined 没有父实例
      // 当前组件树的根Vue实例
      console.log(this.$root)
      // 当前实例的子组件
      console.log(this.$children); // []
      // 当前Vue实例是否运行于服务器
      console.log(this.$isServer); // false
    }
  })
</script>
```



### 1-5 定义多个实例

实例化多个对象与实例化单个 `Vue` 对象方法一样，只是绑定操控的 `el` 元素不一样

如果两个不同的实例想互相访问，可以在其中一个实例中定义方法，通过事件触发来调用另一个实例对象的属性，并改变对象属性

```html
<!-- 挂在vue1元素 -->
<div class="vue1">
  <h1>{{title}}</h1>
</div>

<!-- 挂在vue2元素 -->
<div class="vue2">
  <h1>{{title}}</h1>
  <button type="button" @click="changeVue1Value()">修改vue1内容</button>
</div>

<script src="../vue.js"></script>

<script>
  // 通过Vue函数实例化对象
  let vm1 = new Vue({
    // options选项
    el: '.vue1',
    data: function() {
      return {
        title: 'Hello Vue1'
      }
    }
  })

 let vm2 = new Vue({
    // options选项
    el: '.vue2',
    data: function() {
      return {
        title: 'Hello Vue2'
      }
    },
    methods: {
      changeVue1Value() {
        vm1.title = 'Hello JSX'
      }
    }
  })
</script>
```



## 2. 生命周期

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220512145915.png)



```html
<div id="app">
  <h1>{{title}}</h1>
  <button type="button" @click="changeHeadTitle()">更新数据</button>
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    data: function () {
      return {
        title: 'Hello Vue'
      }
    },
    methods: {
      changeHeadTitle() {
        this.title = 'JSX ♥ LJJ'
      }
    },
    // beforeCreate 初始化界面
    beforeCreate() {
      console.log('beforeCreate');
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      // data与$el都没初始化
      console.log("------------------------");
    },
    // created 初始化界面后
    created() {
      console.log('created')
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      // data初始化完成 $el都没初始化
      // 挂载阶段还没开始
      console.log("------------------------");
    },
    // beforeMounted 渲染DOM前
    beforeMount() {
      console.log('beforeMounted')
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      // data与$el均已初始化完成, DOM为虚拟DOM未完全加载
      console.log(document.querySelector('h1'));
      // <h1>{{title}}</h1>
      console.log("------------------------");
    },
    // mounted 渲染DOM后
    mounted() {
      console.log('mounted')
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      // data与$el均已初始化完成, 并且DOM加载完成, 完成挂载
      console.log(document.querySelector('h1'));
      // <h1>Hello Vue</h1>
      console.log("------------------------");
    },
    // beforeUpdate 更新数据前
    beforeUpdate() {
      console.log('beforeUpdate')
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      // 在数据发生改变后，DOM 被更新之前被调用
      console.log("------------------------");
    },
    // updated 更新数据后
    updated() {
      console.log('updated')
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      // 在数据更改导致的虚拟 DOM 重新渲染和更新完毕之后被调用
      console.log("------------------------");
    },
    // beforeDestory 组件销毁前
    beforeDestroy() {
      console.log('beforeDestory')
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      console.log("------------------------");
    },
    // destoryed 组件销毁后
    destroyed() {
      console.log('destoryed')
      console.log("message:", this.title);
      console.log("$el:", this.$el);
      console.log("------------------------");
    },
  })

</script>
```



### 2-1 beforeCreate(初始化界面前)

在实例初始化之后，数据观测(`data observer`) 和 `event/watcher` 事件配置之前被调用

> `data` 和 `$el` 都没有初始化，全部为 `undefined`



### 2-2 created(初始化界面后)

实例已经创建完成之后被调用，在这一步，实例已完成以下的配置：数据观测(`data observer`)，属性和方法的运算， `watch/event` 事件回调。然而，挂载阶段还没开始，`$el` 属性目前不可见，若在此阶段进行 `DOM` 操作一定要放在 `Vue.nextTick()` 的回调函数中

挂载阶段还没开始，什么叫还没开始挂载，也就是说，模板还没有被渲染成 `html`，也就是这时候通过 `id` 什么的去查找页面元素不能保证一定能找到。所以，一般 `creadted` 钩子函数主要是用来初始化数据

> `data` 初始化完成，但 `$el` 没有初始化



### 2-3 beforeMount(渲染DOM前)

在挂载开始之前被调用：相关的 `render` 函数首次被调用

> `data` 和 `$el` 均已存在，但 `DOM` 为虚拟 `DOM` 仍未完全加载 

> 该钩子在服务器端渲染期间不被调用



### 2-4 mounted(渲染DOM后)

`el` 被新创建的 `vm.$el` 替换，并挂载到实例上去之后调用该钩子

一般是用来向后端发起请求拿到数据以后做一些业务处理，`DOM` 操作一般是在 `mounted` 钩子函数中进行的

第一次页面加载会触发以上四个钩子，即 `beforeCreate` ， `created` ， `beforeMount` ，`mounted` 这四个钩子

> `data` 和 `$el` 均已存在，并且 `DOM` 加载完成 ，完成挂载

> 该钩子在服务器端渲染期间不被调用



### 2-5 beforeUpdate(更新数据前)

数据更新时调用，发生在虚拟 `DOM` 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程，这也是重新渲染之前最后修改数据的机会

这里适合在更新之前访问现有的 `DOM`，比如手动移除已添加的事件监听器

> 该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务器端进行



### 2-6 updated(更新数据后)

由于数据更改导致的虚拟 `DOM` 重新渲染和打补丁，在这之后会调用该钩子。 当这个钩子被调用时，组件 `DOM` 已经更新，所以你现在可以执行依赖于 `DOM` 的操作

应该避免在此期间更改状态，因为这可能会导致更新无限循环，最好使用计算属性或 `watcher` 取而代之

>
> 该钩子在服务器端渲染期间不被调用



### 2-7 beforeDestroy(组件销毁前)

实例销毁之前调用。在这一步，实例仍然完全可用

> 该钩子在服务器端渲染期间不被调用



### 2-8 destroyed(组件销毁后)

`Vue` 实例销毁后调用。调用后 `Vue` 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用

另外还有 `keep-alive` 独有的生命周期，分别为 `activated` 和 `deactivated` 。用 `keep-alive` 包裹的组件在切换时不会进行销毁，而是缓存到内存中并执行 `deactivated` 钩子函数，命中缓存渲染后会执行 `activated` 钩子函数

> 该钩子在服务器端渲染期间不被调用



## 3. 声明周期扩展

### 3-1 使用场景

- `beforeCreate` —— 可以在此时加一些 `loading` 效果，在 `created` 时进行移除
- `created` —— 需要异步请求数据的方法可以在此时执行，完成数据的初始化
- `beforeMount` —— 挂载前，虽然得不到具体的 `DOM` 元素，但vue挂载的根节点已经创建，可以围绕这个根元素继续进行一些操作
- `mounted` —— 可在这发起后端请求，拿回数据，配合路由钩子做一些事情
- `beforeUpdate` —— 可在更新前访问现有的 `DOM`，如手动移出添加的事件监听器
- `updated` —— 当数据更新需要做统一业务处理的时候使用
- `beforeDestroy` —— 可做一些删除提示
- `destroyed` —— 销毁监听事件，这时组件已经没有了，无法操作里面的任何东西了

从上面可以看到，既可以在 `created` 周期内，也可以在 `beforeMount`、`mounted` 周期内执行异步请求，因为在这三个钩子函数中，`data` 已经创建，可以将服务端端返回的数据进行赋值

既然异步函数并不会阻塞 `vue` 生命周期整个进程，那么在哪个阶段请求都可以。如果考虑到用户体验方面的影响，希望用户今早感知页面已加载，减少空白页面时间，建议就放在 `created` 阶段了，然后再处理会出现 `null`、`undefined` 这种情况就好。毕竟越早获取数据，在 `mounted` 实例挂载的时候渲染也就越及时

`SSR`(服务器渲染)不支持 `beforeMount` 、`mounted` 钩子函数，所以放在 `created` 中有助于一致性



### 3-2 父子组件的生命周期

- 父组件开始执行到 `beforeMount`，然后开始子组件执行，最后是父组件 `mounted` 周期
- 如果有兄弟组件，父组件开始执行到 `beforeMount`，然后兄弟组件依次执行到 `beforeMount`，然后按照顺序执行 `mounted`，最后执行父组件的 `mounted`

- 当子组件挂载完成后，父组件才会挂载
- 当子组件完成挂载后，父组件会主动执行一次 `beforeUpdated/updated` 钩子函数（仅首次）
- 父子组件在 `data` 变化中是分别监控的，但是更新 `props` 中的数据是关联的
- 销毁父组件时，先将子组件销毁后才会销毁父组件
- 兄弟组件的初始化（`mounted` 之前）是分开进行，挂载是从上到下依次进行
- 当没有数据关联时，兄弟组件之间的更新和销毁是互不关联的



### 3-3 加载渲染过程

`Vue` 的父组件和子组件生命周期钩子函数执行顺序可以归类为以下 4 部分：

* 父子组件加载过程

```tex
父组件 beforeCreate -> 父组件 created -> 父组件 beforeMount -> 子组件 beforeCreate -> 子组件 created -> 子组件 beforeMount -> 子组件 mounted -> 父组件 mounted
```

- 子组件更新过程

```tex
父组件 beforeUpdate -> 子组件 beforeUpdate -> 子组件 updated -> 父组件 updated
```

- 父组件更新过程

```tex
父组件 beforeUpdate -> 父组件 updated
```

- 销毁过程

```tex
父组件 beforeDestroy -> 子组件 beforeDestroy -> 子组件 destroyed -> 父组件 destroyed
```



### 3-4 禁止使用箭头函数

`Vue` 的所有生命周期函数都是自动绑定到 `this` 的上下文上

使用箭头函数的话，就会出现 `this` 指向的父级作用域，就会报错

```js
// 错误
mounted:() => {
  console.log(this)
}
```



## 3. 模版语法

`Vue.js` 使用了基于 `HTML` 的模板语法，允许开发者声明式地将 `DOM` 绑定至底层 `Vue` 实例的数据

所有 `Vue.js` 的模板都是合法的 `HTML`，所以能被遵循规范的浏览器和 `HTML` 解析器解析

在底层的实现上，`Vue` 将模板编译成虚拟 `DOM` 渲染函数。结合响应系统，`Vue` 能够智能地计算出最少需要重新渲染多少组件，并把 `DOM` 操作次数减到最少



## 4. 插值语法

### 4-1 文本

* 数据绑定最常见的形式就是使用 `Mustache` 语法 (双大括号) 的文本插值 `{{title}}`

* `Mustache` 语法中的内容将会被替代为对应数据对象上属性的值。无论何时，绑定的数据对象上属性发生了改变，插值处的内容都会更新

```html
<div id="app">
  <!-- Mustache语法 将文本值插入到{{}}双大括号中 -->
  <h1>{{title}}</h1>
  <!-- Mustache标签对应data中的属性值 当绑定的数据发生改变时，插值处的内容会更新 -->
  <button type="button" @click="changeHeadTitle()">更新数据</button>
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    data: function () {
      return {
        title: 'Hello Vue'
      }
    },
    methods: {
      // 更新内容
      changeHeadTitle() {
        this.title = 'Hello JSX'
      }
    }
  })
</script>
```



### 4-2 原始HTML

> **Tips：**站点上动态渲染的任意 `HTML` 可能会非常危险，因为它很容易导致 `XSS` 攻击。请只对可信内容使用 `HTML` 插值，绝不要对用户提供的内容使用插值

* 双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 `HTML`，你需要使用 `v-html` 指令

```html
<div id="app">
  <!-- v-html指令 插入HTML标签元素 -->
  <div v-html="htmlValue"></div>
  <!-- Mustache中的值会解析为普通文本 -->
  <h3>{{htmlValue}}</h3>
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    data: function () {
      return {
        htmlValue: '<h3>原生HTML插值</h3>'
      }
    }
  })
</script>
```



### 4-3 元素属性

`Mustache` 语法不能作用在 `HTML` 元素属性上，遇到这种情况应该使用 `v-bind` 指令

对于布尔类型的属性只要值存在就意味着值为 `true`，设置 `null` 和 `undefined` 以及 `false` 则值为 `false`

```html
<div id="app">
  <!-- v-bind绑定元素属性 -->
  <div v-bind:class="attr">v-bind绑定元素属性</div>
  <!-- 对于布尔类型属性只要值存在则为true  -->
  <!-- false null undefined 或者则为 false -->
  <div v-bind:hidden="bool">v-bind布尔类型</div>
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    data: function () {
      return {
        attr: 'box',
        bool: false
      }
    }
  })
</script>
```



### 4-4 表达式

`Mustache` 语法支持表达式形式，支持一元表达式，二元表达式以及常用的 `API` 方法

只支持 `JavaScript` 表达式每个绑定都只能包含单个表达式，不支持 `JavaScript` 语句形式

```html
<div id="app">
  <!-- Mustache语法支持表达式形式的值 -->
  <h3>{{num + 1}}</h3> 
  <h3>{{bool ? 'Yes' : 'No'}}</h3>
  <h3>{{arr.join(' ')}}</h3>
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    data: function () {
      return {
        num: 100,
        arr: ['html', 'css', 'js']
      }
    }
  })
</script>
```



## 5. 指令

指令 (`Directives`) 是带有 `v-` 前缀的特殊属性。指令属性的值预期是单个 `JavaScript` 表达式

当表达式的值改变时，将其产生的连带影响，响应式地作用于 `DOM`



### 5-1 指令参数

一些指令能够接收一个参数，在指令名称之后以冒号表示

* `v-bind` 指令可以用于对元素属性的绑定以及响应式地更新 `HTML` 元素属性
* `v-on` 指令用于完成事件的绑定以及监听 `DOM` 事件，`:` 后面的参数是监听的事件名

```html
<div id="app">
  <!-- v-bind用于绑定HTML元素的属性 -->
  <!-- 可以用于响应的更新HTML元素属性与绑定 -->
  <a v-bind:href="url">百度</a>
  <button type="button" v-on:click="toBaidu">百度</button>
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    data: function () {
      return {
        url: 'https://www.baidu.com'
      }
    },
    methods: {
      toBaidu() {
        window.location.href = 'https://www.baidu.com'
      }
    }
  })
</script>
```



### 5-2 动态参数

从 `Vue2.6.0` 版本开始，可以用方括号 `[]` 括起来的 `JavaScript` 表达式作为一个指令的参数

方括号 `[]` 里的参数表达式会进行动态求值，求得的值将会作为最终的参数来使用

* 动态参数的值预期是一个字符串类型，如果值为 `null` 则会等于直接移除绑定
* 动态参数表达式某些字符，空格和引号，放在 `HTML` 元素属性名里是无效的
* 动态参数方式下避免使用大写字符来命名键名，浏览器会把 `HTML` 元素属性名全部强制转为小写

```html
<div id="app">
  <!-- 可以使用方括号作为指令绑定动态参数 -->
  <a v-bind:[hrefAttr]="base">360</a>
  <!-- 动态参数值预期为字符串不能为null null则表示移除属性 -->
  <img v-bind:[srcAttr]="imgUrl" alt="gril">
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    data: function () {
      return {
        // HTML元素属性大写会转换为小写
        // data里的属性名需要小写
        hrefattr: 'href',
        imgUrl: 'https://tupian.qqw21.com/article/UploadPic/2022-5/202251219401537182.jpg',
        srcattr: null
      }
    }
  })
</script>
```



### 5-3 修饰符 

修饰符 (`modifier`) 是以点字符 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定

```html
<div id="app">
  <!-- 修饰符 通过点字符. 指明的特殊后缀 -->
  <a href="http://www.baidu.com" @click.prevent="stopTo">禁止跳转</a>
</div>

<script src="../vue.js"></script>

<script>
  let vm = new Vue({
    el: '#app',
    methods: {
      stopTo() {
        console.log('禁止跳转')
      }
    }
  })
</script>
```



## 6. 缩写

`Vue` 中 `v-bind` 和 `v-on` 这两个最常用的指令有缩写指令

* `:` —— 冒号就是 `v-bind` 指令的缩写
* `@` —— `@` 符号就是 `v-on` 指令的缩写

```html
<!-- v-bind指令缩写为: -->
<h3 :class="classname">v-bind指令缩写为</h3>
<!-- 相当于 -->
<!-- <h3 v-bind:class="classname">v-bind指令缩写为</h3> -->

<!-- v-on指令缩写为@ -->
<h3 @click="alertValue">v-on指令缩写为@</h3>
<!-- 相当于 -->
<!-- <h3 v-on:click="alertValue">v-on指令缩写为@</h3> -->
```

