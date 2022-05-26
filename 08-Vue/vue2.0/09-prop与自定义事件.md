# prop与自定义事件

## 1. prop

### 1-1 什么是prop

`prop` 是你可以在组件上注册的一些自定义属性。当一个值传递给一个 `prop` 属性的时候，它就变成了那个组件实例的一个属性

* 组件中 `prop` 通常用于将父组件的值传递给子组件使用，或者将 `Vue` 实例里的属性值传递到组件中使用
* 一个组件默认可以拥有任意数量的 `prop`，任何值都可以传递给任何 `prop`，在组件实例中访问这个值，就像访问 `data` 中的值一样

```html
<!-- prop就是在组件上的属性称为prop prop会传递到组件内部 -->
<component-a message="Hello Vue"></component-a>
<!-- prop可以是任何类型 prop不限制数量 -->
```

```js
// 创建组件
let componentA = {
  name: 'ComponentA',
  // 传入的prop可以直接使用
  template: `<h3>{{message}}</h3>`,
  props: ['message']
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  // 注册组件
  components: {
    componentA
  }
})
```



### 1-2 prop大小写

`HTML` 中的元素属性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符

当你使用 `DOM` 中的模板时，`camelCase` (驼峰命名法) 的 `prop` 名需要使用其等价的 `kebab-case` (短横线分隔命名) 命名

> 使用字符串模板不存在这个限制

```html
<!-- 在html中需要使用短横线分隔形式 -->
<!-- <conponent-a headTitle="Hello Vue"></conponent-a> -->
<component-a head-title="Hello Vue"></component-a>
```

```js
// html中大小写不敏感所有大写都会转为小写
// 驼峰命名的prop在html中需要短横线分隔形式命名
let componentA = {
  template: `<h3>{{headTitle}}</h3>`,
  props: ['headTitle']
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  components: {
    componentA
  }
})
```



### 1-3 prop类型

* 数组形式 —— 当期望只传入 `prop` 不指定值类型时，可以字符串数组形式列出的 `prop`

* 对象形式 —— 当期望每个 `prop` 都需要指定值的类型时，可以以对象形式列出 `prop`，这些属性的名称和值分别是 `prop` 各自的名称和类型

> 在 `prop` 绑定组件时，不使用 `v-bind` 指令绑定的 `prop` 都为字符串，只有通过 `v-bind` 绑定后 `prop` 才能传入其它类型的数据

```html
<div id="app">
  <component-a message="Hello Vue" info="prop类型"></component-a>
  <component-b 
    :str="'字符串'" 
    :bool="true" 
    :num="100" 
    :arr="['html', 'css', 'js']" 
    :obj="{name: 'jsx'}"
    :func="function() { return 'function函数' }">
  </component-b>
</div>

<template id="box">
  <div>
    <h3>{{message}}</h3>
    <h3>{{info}}</h3>
  </div>
</template>

<template id="pannel">
  <div>
    <h3>{{str}} {{bool}} {{num}}</h3>
    <h3 v-for="list of arr">{{list}}</h3>
    <h3>{{obj.name}}</h3>
    <h3>{{func()}}</h3>
  </div>
</template>
```

```js
let componentA = {
  template: '#box',
  // 不需要指定传入prop的类型时，可以使用数组字符串形式
  props: ['message', 'info']
}

let componentB = {
  template: '#pannel',
  props: {
    str: String,
    num: Number,
    bool: Boolean,
    arr: Array,
    obj: Object,
    func: Function
  }
}

const vm = new Vue({
  el: '#app',
  data: function() {
    return {}
  },
  components: {
    componentA,
    componentB
  }
})
```



### 1-4 动态或者静态prop

* 静态 `prop` —— 通过直接在组件上使用 `prop` 并设置静态的值
* 动态 `prop` —— 通过 `v-bind` 指定在组件上绑定 `prop` 动态赋值

当想要一个对象的所有属性作为 `prop` 传入时，可以通过 `v-bind` 不带参数（取代 `v-bind:prop-name`），对象内的属性必须在 `props` 选项中时指定

> 即便对象是静态的，我们仍然需要 `v-bind` 来告诉 `Vue` ，这是一个 `JavaScript` 表达式而不是一个字符串

```html
<!-- 组件静态prop prop是给定的不能修改的值 -->
<component-a
  :str="'hello vue'"
  :num="100"
  :bool="true"
  :arr="['html', 'css', 'js']"
  :obj="{ name: 'jsx' }"
  v-bind="{study: '50%'}">
</component-a>

<!-- 组件动态prop 通过传入设置的数据动态赋值 -->
<component-a
  :str="dataStr"
  :num="dataNum"
  :bool="dataBool"
  :arr="dataArr"
  :obj="dataObj"
  v-bind="lessonObj">
</component-a>

<template id="box">
  <div>
    <h3>{{str}}</h3>
    <h3>{{num}}</h3>
    <h3>{{bool}}</h3>
    <h3 v-for="list of arr">{{list}}</h3>
    <h3>{{obj.name}}</h3>
    <h3>{{study}}</h3>
  </div>
</template>
```

```js
let componentA = {
  template: '#box',
  data() {
    return {}
  },
  props: {
    str: String,
    num: Number,
    bool: Boolean,
    arr: Array,
    obj: Object,
    study: String
  }
}

const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      dataStr: 'string',
      dataNum: 520,
      dataBool: true,
      dataArr: ['html', 'css', 'js'],
      dataObj: {
        name: 'jsx'
      },
      lessonObj: {
        name: 'vue',
        study: '50%'
      }
    }
  },
  components: {
    componentA
  }
})
```



### 1-5 单向数据流

所有的 `prop` 都使得其父子 `prop` 之间形成了一个单向下行绑定，父级 `prop` 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解

每次父级组件发生变更时，子组件中所有的 `prop` 都将会刷新为最新的值，不能在子组件直接修改传入的 `prop`，`Vue` 会在浏览器的控制台中发出警告

* 在子组件定义一个 `data` 用来接受传入的 `prop` 用作其初始值
* 当 `prop` 以一种原始的值传入且需要进行转换时，可以给这个 `prop` 的值来定义一个计算属性

```html
<div id="app">
  <button @click="changeTitle">修改title</button>
  <component-a :message="title"></component-a>
</div>
```

```js
// 在vue中prop会使其父子prop形成向下绑定
// 父组件的prop更新会响应到子组件中，反过来则不可以，不能直接修改传入的prop的值
let componentA = {
  template: `
      <div>
        <h3>{{message}}</h3>
        <h3>{{value}}</h3>
        <h3>{{getValue}}</h3>
        <button @click="changeProp">修改value</button>  
      </div>
      `,
  props: ['message'],
  data() {
    return {
      // 在子组件定义一个data用来接受传入的prop用作其初始值
      value: this.message
    }
  },
  methods: {
    changeProp() {
      // 不要在子组件中修改prop值 vue是单向数据流的
      this.value = 'Hello JSX'
    }
  },
  // 通过computed来定义prop 当外部prop发生改变时内部响应更新
  // 可以将原始prop值转换为当前组件需要的属性
  computed: {
    getValue() {
      return this.message
    }
  }
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      title: 'hello vue'
    }
  },
  components: {
    componentA
  },
  methods: {
    changeTitle() {
      this.title = 'Hello Vue'
    }
  }
})
```



### 1-6 prop验证

可以为组件的 `prop` 指定验证要求，`prop` 可以是数组或对象，用于接收来自父组件的数据。`prop` 可以是简单的数组，或者使用对象作为替代，对象允许置高级选项，如类型检测、自定义验证和设置默认值

- `type`：可以是下列原生构造函数中的一种：`String`、`Number`、`Boolean`、`Array`、`Object`、`Date`、`Function`、`Symbol`、任何自定义构造函数、或上述内容组成的数组。会检查一个 `prop` 是否是给定的类型，否则抛出警告
- `default`：`any`
  为该 `prop` 指定一个默认值。如果该 `prop` 没有被传入，则换做用这个值。对象或数组的默认值必须从一个工厂函数返回
- `required`：`Boolean`
  定义该 `prop` 是否是必填项。在非生产环境中，如果这个值为 `truthy` 且该 `prop` 没有被传入的，则一个控制台警告将会被抛出
- `validator`：`Function`
  自定义验证函数会将该 `prop` 的值作为唯一的参数代入。在非生产环境下，如果该函数返回一个 `falsy` 的值 (也就是验证失败)，一个控制台警告将会被抛出

`type` 属性可以自定义构造函数，并且通过 `instanceof` 来进行检查确认，检查 `prop` 传入的值是否是通过自定义构造函数 `new` 创建的

> 需要注意的是 `null` 和 `undefined` 会通过任何类型验证

```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    },
    // 自定义构造函数用于类型验证
    propG: {
      type: User
    }
    // propG传入的值必须是User构造函数new创建的
  }
})

function User(name, age) {
  this.name = name;
  this.age = age;
}
// 自定义构造函数验证通过instanceof来验证
// 验证传入的prop值是否是通过构造函数 new创建的
let user = new User('jsx', 22)
console.log(user);
```



### 1-7 非prop的组件属性

非 `prop` 的属性是指该组件并没有相应 `prop` 定义的属性（组件的属性 `prop` 未定义）

组件可以接受任何属性，当属性在组件 `props` 中未定义时，这些属性会被添加到这个组件的根元素上

```html
<!-- 非prop的属性 —— 组件上的属性没有相应的prop定义 -->
<component-a :title="'Hello Vue'"></component-a>
<!-- 会将这些属性添加到组件的根元素上 -->
```

```js
Vue.component('component-a', {
  template: `
  <div class="box">
    <h3>Hello Vue</h3>
  </div>`
})
```



### 1-8 替换合并组件属性

* 外部传入的属性会替换组件模版内部设置的属性，外部提供给组件的值会替换掉组件内部设置好的值
* `class` 与 `style` 属性不会被外部替换，而是将外部传来的值合并起来得到最终的值

```html
<!-- 外部传入的属性会替换组件内部属性 -->
<component-a :title="'Hello JSX'"></component-a>
<!-- 外部传入的class与style会与组件内部的合并 -->
<component-a :class="'main'" :style="'font-size: 20px'"></component-a>
```

```js
Vue.component('component-a', {
  template: `
  <div class="box" title="Hello Vue" style="color: #ff0000">
    <h3>Hello Vue</h3>
  </div>`
})
```



### 1-9 禁止属性继承

当不希望组件的根元素继承属性时，可以在创建组件的 `options` 选项中设置 `inheritAttrs: false`

> 注意 `inheritAttrs: false` 选项**不会**影响 `style` 和 `class` 的绑定

```html
<!-- 禁止根元素继承非prop属性设置 inheritAttrs: false -->
<component-a :title="'Hello Vue'"></component-a>
```

```js
Vue.component('component-a', {
  // div不会继承title属性
  template: `
  <div class="box">
    <h3>Hello Vue</h3>
  </div>`,
  inheritAttrs: false
})
```



### 1-10 $attrs

`prop` 只适用于父子组件的传值，当组件中有多层嵌套的组件父组件想传递给子孙组件时，通过 `prop` 一层一层非常繁琐

非 `prop` 属性的值会在组件的根元素上，组件设置 `inheritAttrs: false` 可以让组件的根元素不继承这些属性，通过 `$attrs` 来决定嵌套组件中哪个组件接受传递的数据

```html
<div id="app">
  <component-a :title="message"></component-a>
</div>
```

```js
let componentB = {
  // $attrs手动决定这些属性会被赋予哪个元素
  template: `
      <div class="component-b">
        <h3>componentB</h3>
        <h3>{{title}}</h3>  
      </div>
      `,
  props: {
    title: {
      type: String,
      default: 'Hello Vue'
    }
  }
}

// $attrs可以配合inheritAttrs:false属性向内部组件传递数据
let componentA = {
  template: `
      <div class="component-a">
        <h3>componentA</h3>  
        <component-b v-bind="$attrs"></component-b>
      </div>
      `,
  components: {
    componentB
  },
  inheritAttrs: false
}

const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      message: 'Hello JSX'
    }
  },
  components: {
    componentA
  }
})
```



## 2. 自定义事件

### 2-1 事件名



### 2-2 自定义组件model



### 2-3 .native修饰符



### 2-4 组件事件监听器$listeners



### 2-5 .sync修饰符
