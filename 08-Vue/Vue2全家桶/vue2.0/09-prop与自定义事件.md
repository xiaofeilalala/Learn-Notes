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

> **Tips：**`$emit`、`$on`、`$off`、`$once` 实例方法事件中回调函数中 `this` 都是指向调用事件方法的实例

### 2-1 事件名

自定义事件的事件名与 `prop` 不同，事件名不会自动化大小写转换，触发的事件名必须完全匹配监听这这个事件的名称

如果触发一个 `camelCase` 名字的事件，则监听这个名字的 `kebab-case` 形式是不会有任何效果的

`v-on` 事件监听器在 DOM 模板中会被自动转换为全小写，所以 `v-on:myEvent` 将会变成 `v-on:myevent`——导致 `myEvent` 不可能被监听到

```html
<h3>{{message}}</h3>
<component-a v-on:get-component="changValue"></component-a>
```

```js
let componentA = {
  template: '<button @click="sendMessage">修改父组件数据</button>',
  data() {
    return {
      value: 'Hello JSX'
    }
  },
  methods: {
    sendMessage() {
      // $emit触发事件名始推荐终使用 kebab-case 的事件名
      this.$emit('get-component', this.value)
      // $emit 为驼峰式 v-on 则无法绑定
    }
  },
  props: ['getComponent']
}
```



### 2-2 $emit

`vm.$emit(eventName, ...args)`

`$emit` —— 触发当前实例上的事件，附加参数都会传给监听器回调函数

`$emit` 传入的参数可以通过 `$event` 访问，也可以通过事件处理方法中访问，方法中的参数则是 `$emit` 传入的参数

* 通过父组件给子组件传递函数类型的 `prop` ，实现子组件传递数据给父组件

* 通过 `v-on` 监听子组件实例的任意事件，同时子组件可以通过调用内建的 `$emit` 方法并传入事件名称来触发一个事件，实现子组件传递数据给父组件

* 通过在子组件上定义 `ref` 获取当前组件实例，同时通过 `$on` 监听当前实例上的自定义事件，实现子组件传递数据给父组件

> 当自定义事件传递多个参数时可以包装成对象再传递，自定义事件也可以一个一个传递参数，父组件可以通过 `...rest` 剩余参数形式来获取多个传递的参数 

```html
<div id="app">
  <h3>{{message}}</h3>
  <component-a :prop-value="changeMessage" @emit-value="changeMessage" @ref-value="changeMessage" ref="refComponent"></component-a>
</div>

<template id="box">
  <div>
    <button @click="propEvent">prop修改</button>
    <button @click="emitEvent">$emit修改</button>
    <button @click="refEvent">ref修改</button>
  </div>
</template>
```

```js
let componentA = {
  template: '#box',
  props: ['propValue'],
  methods: {
    // 1. 通过传递一个函数类型prop来实现子传递父
    propEvent() {
      this.propValue(this.prop)
    },
    // 2. 通过v-on与$emit实现子传递父
    emitEvent() {
      // 传递多个参数时可包装为对象
      this.$emit('emit-value', this.emit, 'jsx', 'ljj')
    },
    // 3. 通过ref与$on实现子传递父
    refEvent() {
      // 监听组件上的ref-value自定义事件触发来传递数据
      this.$emit('ref-value', this.ref, 'html', 'vue')
    }
  },
  data() {
    return {
      prop: 'Hello Prop',
      emit: 'Hello Emit',
      ref: 'Hello Ref'
    }
  }
}

const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      message: 'Hello Vue'
    }
  },
  components: {
    componentA
  },
  methods: {
    // 自定义事件传递对个参数时可以使用剩余参数
    changeMessage(val, ...args) {
      console.log(args)
      this.message = val
    }
  },
  mounted() {
    this.$refs.refComponent.$on('ref-value', this.changeMessage)
    // $on 监听实例上的自定义事件
    // vm.$on 监听Vue实例
    // this.$refs.refComponent 监听ref所定义组件实例
  },
})
```



### 2-3 $on

`vm.$on(event, callback)`

`$on` —— 监听当前实例上的自定义事件，`$emit` 触发自定义事件，回调函数会接收所有传入事件触发函数的额外参数

```html
<div id="app">
  <h3>{{title}}</h3>
  <component-a @component-event="getTitle" ref="component"></component-a>
</div>

<template id="box">
  <div class="box">
    <button type="button" @click="changeTitle">$on</button>
  </div>
</template>
```

```js
// $on用于监听当前实例上的自定义事件
let componentA = {
  template: '#box',
  methods: {
    changeTitle() {
      this.$emit('component-event', 'Hello JSX')
    }
  }
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      title: 'Hello Vue'
    }
  },
  components: {
    componentA
  },
  methods: {
    getTitle(val) {
      console.log(val)
      this.title = val
    }
  },
  mounted() {
    this.$refs.component.$on('component-event', this.getTitle)
  },
})
```



### 2-4 $off与$destroy

`vm.$off(event, callback)`



`$off` —— 移除自定义事件监听器

* 如果没有提供参数，则移除所有的事件监听器
* 如果只提供了事件，则移除该事件所有的监听器
* 如果同时提供了事件与回调，则只移除这个回调的监听器

`$destory` —— 完全销毁一个实例，清理它与其它实例的连接，解绑它的全部指令及事件监听器

* 触发 `beforeDestroy` 和 `destroyed` 的钩子



组件解除事件绑定的两种方式：

1. 通过 `$off`

2. 通过 `$destroy` 声明周期实例方法

```html
<div id="app">
  <h3>{{title}}</h3>
  <h3>{{message}}</h3>
  <component-a @title-event="getTitle" @message-event="getMessage"></component-a>
</div>

<template id="box">
  <div class="box">
    <button type="button" @click="changeTitle">$on title</button>
    <button type="button" @click="changeMessage">$on message</button>
    <button type="button" @click="removeEvent">$off</button>
  </div>
</template>
```

```js
// 取消自定义事件绑定
// 1. 通过$off 
// 2. 通过destory声明周期函数

let componentA = {
  template: '#box',
  mounted() {
    // 移除这个监听器
    this.$on('message-event', this.callback)
  },
  methods: {
    changeTitle() {
      this.$emit('title-event', 'Hello JSX')
    },
    changeMessage() {
      this.$emit('message-event', 'Hello World')
    },
    removeEvent() {
      // 只提供自定义事件，则移除该自定义事件的所有监听器
      this.$off('message-event')
      // 数组形式移除多个自定义事件的所有监听器
      this.$off(['title-event', 'message-event'])
      // 不传入参数 移除组件所有的事件监听器
      this.$off();
      // 提供自定义事件与回调，则只移除回调的监听器
      this.$off('message-event', this.callback)
      // 组件声明周期实例来实现移除事件监听器
      this.$destroy()
    },
    callback(val) {
      console.log(val)
    }
  }
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      title: 'Hello Vue',
      message: 'Hello JS'
    }
  },
  components: {
    componentA
  },
  methods: {
    getTitle(val) {
      console.log(val)
      this.title = val
    },
    getMessage(val) {
      console.log(val)
      this.message = val
    }
  }
})
```



### 2-5 $once

`vm.$once(event, callback)`

`$once` —— 监听当前实例的一个自定义事件，但是只触发一次，一旦触发之后，监听器就会被移除销毁

可以为同一事件绑定多个回调，触发时回调函数按照绑定顺序依次执行

```html
<div id="app">
  <h3>{{title}}</h3>
  <component-a @change-title="getTitle"></component-a>
</div>

<template id="box">
  <div>
    <button @click="onceEvent">$once title</button>
  </div>
</template>
```

```js
// $once 监听一个自定义事件 只触发一次 触发后监听器移除
let componentA = {
  template: "#box",
  data() {
    return {
      value: 'Hello World'
    }
  },
  methods: {
    onceEvent() {
      this.$emit('change-title', 'Hello JSX')
    }
  },
  mounted() {
    // 只监听一次chang-title自定义事件
    this.$once('change-title', (val) => {
      console.log(val)
    })
  },
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      title: 'Hello Vue'
    }
  },
  components: {
    componentA
  },
  methods: {
    getTitle(val) {
      this.title = val
      console.log(val)
    }
  }
})
```



### 2-6 自定义组件model

组件上的 `v-model` 会默认使用名为 `value` 的 `prop` 和为 `input` 的事件

```html
<component-a v-model="title"></component-a>
<!-- 组件上使用v-model相当于 -->
<component-a v-on:input="title = $event.target.value" v-bind:value="title"></component-a>

<!-- 子组件模版接受默认value prop -->
<!-- 子组件通过 $emit 派发默认 input 事件 -->
<template id="box">
  <input type="text" :value="value" @input="changInput" >
</template>
```

```js
let componentA = {
  template: '#box',
  props: ['value'],
  methods: {
    changInput(e) {
      this.$emit('input', e.target.value)
    }
  }
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      title: 'Hello Vue'
    }
  },
  components: {
    componentA
  }
})
```



`model` 选项允许一个自定义组件在使用 `v-model` 时定制 `prop` 和 `event`

单选框和复选框按钮可能想使用 `value` `prop` 来达到不同的目的。使用 `model` 选项可以回避这些情况产生的冲突

> 在 `model` 选项中声明 `prop` 后，仍然需要在组件的 `props` 选项里声明 `prop`

```html
<component-b v-model="message"></component-b>
<!-- 当组件为单选框、复选框等类型的输入控件value用于不同目的 -->

<template id="pannel">
  <div>
    <h3>{{checked}}</h3>
    <input type="checkbox" :checked="checked" @change="changeCheckbox" true-value="Hello Vue" false-value="Hello React">
  </div>
</template>
```

```js
let componentB = {
  template: '#pannel',
  // 通过model改变组件v-mdoel默认prop与事件
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: ['checked'],
  methods: {
    changeCheckbox(e) {
      this.$emit('change', e.target.checked)
    }
  }
}

const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      message: true
    }
  },
  components: {
    componentB
  }
})
```



### 2-7 .native修饰符

在组件上通过 `v-on` 绑定的事件都是自定义事件，当想要在组件的根元素上直接监听一个元素事件时，可以使用 `.native` 修饰符

```html
<h3>{{title}}</h3>
<!-- 直接在组件上使用v-on都会被认为自定义事件 -->
<component-a @click="changeTitle"></component-a>
<!-- 当想直接在组件根元素绑定原生事件时可以使用.navtive修饰符 -->
<component-a @click.native="changeTitle"></component-a>

<template id="box">
  <div @click="sendTitle" style="margin: 10px 0; width: 100px; height: 100px; background-color: #7852f3;"></div>
</template>
```

```js
let componentA = {
  template: '#box',
  methods: {
    sendTitle() {
      this.$emit('click', 'Hello JSX')
    }
  }
}
const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      title: 'Hello Vue'
    }
  },
  components: {
    componentA
  },
  methods: {
    changeTitle(val) {
      this.title = val
      console.log(val)
      // 1. 未设置.native修饰符 此时点击事件为自定义事件 val为回调的参数
      // 2. 设置.native修饰符 此时组件根元素绑定元素事件触发 val 为事件对象event
    }
  }
})
```



### 2-8 $listeners

`$listeners` 包含父作用域中不含 `.navtive` 修饰符的 `v-on` 事件监听器

可以通过 `v-on="$listeners"` 传入内部组件，将所有的事件监听器指向这个组件的某个特定的子元素

```html
<div id="app">
  <h3>{{title}}</h3>
  <component-a @change-title="getTitle"></component-a>
</div>

<template id="box">
  <div>
    <div>
      <h3>组件A</h3>
      <button @click="componentAEvent">component-A</button>
    </div>
    <!-- $listeners 传递到内部嵌套组件 -->
    <component-b v-on="$listeners"></component-b>
  </div>
</template>

<template id="pannel">
  <div>
    <h3>组件B</h3>
    <button @click="componentBEvent">component-b</button>
  </div>
</template>
```

```js
let componentB = {
  template: '#pannel',
  methods: {
    componentBEvent() {
      // 孙组件也能接受到事件监听器
      console.log(this.$listeners)
      this.$emit('change-title', 'Love')
    }
  }
}

let componentA = {
  template: '#box',
  components: {
    componentB
  },
  methods: {
    componentAEvent() {
      // 子组件也能接受到事件监听器
      this.$emit('change-title', 'Me')
    }
  }
}

const vm = new Vue({
  el: '#app',
  data: function() {
    return {
      title: 'Hello Vue'
    }
  },
  components: {
    componentA
  },
  methods: {
    getTitle(val) {
      this.title = val
    }
  }
})
```



### 2-9 .sync修饰符

子组件不能直接修改父组件传递过来的 `prop`，一般情况下，想要实现父子组件间值的传递，通常使用的是外部数据 `props` 和自定义事件 `$emit`

父组件通过 `props` 将值传给子组件，子组件再通过 `$emit` 将值传给父组件，父组件通过事件监听获取子组件传过来的值。`.sync` 修饰符可将以上过程简化，实际上就是一个语法糖

可以通过 `update:myPropName` 的模式触发事件，`update` 是 `vue` 规定的语法书写格式 `myPropName` 是被绑定事件的属性，`.sync` 可以简化属性绑定与事件绑定操作 `v-bind:myPropName.sync` 即可实现

* `.sync` 修饰符 —— 可实现子组件与父组件的双向绑定，且可实现子组件同步修改父组件的值
* `$event` 可以获取 `$emit` 的参数

> 注意带有 `.sync` 修饰符的 `v-bind` 不能和表达式一起使用

```html
<label for="">父组件</label>
<input type="text" v-model="message">
<!-- <component-a :message="message" v-on:update:message="message = $event"></component-a> -->
<!-- 父组件则通过$event 来接收经过子组件修改后的值 -->
<!-- v-on:update:message update是vue规定的语法书写格式 message是被绑定事件的属性-->
<component-a :message.sync="message"></component-a>

<template id="box">
  <div>
    <label for="">{{message}}</label>
    <button @click="changMessage">$emit title</button>
  </div>
</template>
```

```js
let componentA = {
  template: '#box',
  props: ['message'],
  methods: {
    changMessage() {
      this.$emit('update:message', 'Hello JSX')
      // 在子组件使用$emit('update:money', money-100) 来通知父组件去响应
    }
  },
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
      message: 'Hello Vue'
    }
  },
  components: {
    componentA
  },
  methods: {
    getMessage($event) {
      console.log($event)
      this.message = $event
    }
  }
})
```

