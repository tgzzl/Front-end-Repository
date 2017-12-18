# Vue.js 2.x

#### [生命周期](https://cn.vuejs.org/v2/guide/instance.html#生命周期图示)

![image](https://cn.vuejs.org/images/lifecycle.png)

#### [过滤器](https://cn.vuejs.org/v2/guide/syntax.html#过滤器)

由于最初计划过滤器的使用场景，是用于文本转换，所以 Vue 2.x 过滤器只能用于双花括号插值(mustache interpolation)和 v-bind 表达式中（后者在 2.1.0+ 版本支持）。对于复杂数据的转换，应该使用计算属性。

过滤器函数总接收表达式的值 (之前的操作链的结果) 作为第一个参数。

过滤器可以串联：
```
{{ message | filterA | filterB }} 
```

#### [Computed 属性 vs Watch 属性 vs Methods](https://cn.vuejs.org/v2/guide/computed.html#计算属性)

- Computed

计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter:


```
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```
- Methods

相比而言，只要发生重新渲染，method 调用总会执行该函数。

- Watch

watch 选项提供一个更通用的方法，来响应数据的变化。当你想要在数据变化响应时，执行异步操作或开销较大的操作，这是很有用的。


#### [自定义事件](https://cn.vuejs.org/v2/guide/components.html#自定义事件)

- .sync 修饰符

 Vue 1.x 中的 .sync修饰符可以对一个 prop 进行『双向绑定』。因为它破坏了『单向数据流』的假设，在 2.0 中移除了 .sync。
 
 Vue 2.3.0+ 重新引入了 .sync 修饰符，但是这次它只是作为一个编译时的语法糖存在。它会被扩展为一个自动更新父组件属性的 v-on 侦听器。
 
 如下代码

```
<comp :foo.sync="bar"></comp>
```

会被扩展为：

```
<comp :foo="bar" @update:foo="val => bar = val"></comp>
```

当子组件需要更新 foo 的值时，它需要显式地触发一个更新事件：

```
this.$emit('update:foo', newValue)
```

- 定制组件的 v-model (2.2.0+)

默认情况下，一个组件的 v-model 会使用 value 属性和 input 事件，但是诸如单选框、复选框之类的输入类型可能把 value 属性用作了别的目的。model 选项可以回避这样的冲突：


```
Vue.component('my-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    // this allows using the `value` prop for a different purpose
    value: String
  },
  // ...
})

<my-checkbox v-model="foo" value="some value"></my-checkbox>

//上述代码等价于：

<my-checkbox :checked="foo" @change="val => { foo = val }" value="some value"></my-checkbox>
```

- 作用域插槽(2.1.0+)

作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项：


```
<my-awesome-list :items="items">
  <!-- 作用域插槽也可以是具名的 -->
  <template slot="item" scope="props">
    <li class="my-fancy-item">{{ props.text }}</li>
  </template>
</my-awesome-list>

<ul>
  <slot name="item"
    v-for="item in items"
    :text="item.text">
    <!-- 这里写入备用内容 -->
  </slot>
</ul>

```

#### [深入响应式原理](https://cn.vuejs.org/v2/guide/reactivity.html)

- 如何追踪变化

把一个普通 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 ***==Object.defineProperty==*** 把这些属性全部转为 getter/setter。Object.defineProperty 是仅 ES5 支持，且无法 shim 的特性，这也就是为什么 Vue 不支持 IE8 以及更低版本浏览器的原因。

每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。

![image](https://cn.vuejs.org/images/data.png)

- 异步更新队列

Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会一次推入到队列中。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际（已去重的）工作。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MutationObserver，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。

当设置 vm.someData = 'new value' ，该组件不会立即重新渲染。当刷新队列时，组件会在事件循环队列清空时的下一个“tick”更新。
为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用 Vue.nextTick(callback) 。这样回调函数在 DOM 更新完成后就会调用。


```
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: '没有更新'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = '更新完成'
      console.log(this.$el.textContent) // => '没有更新'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => '更新完成'
      })
    }
  }
})
```

