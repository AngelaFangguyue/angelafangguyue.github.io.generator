---
title: "Vue浅析之构造选项"
date: 2020-02-26T20:27:40+08:00
draft: false
categories:
  - vue
tags:
  - vue
featured_image:
---

### 本篇文章介绍 Vue 实例的构造选项。主要内容有：

[1:关于 options](#a1)

[2:options 之数据](#a2)

[3:options 之 DOM](#a3)

[4:options 之生命周期钩子](#a4)

[5:options 之资源](#a5)

[6:options 之组合](#a6)

[7:options 之其他](#a7)

[8:vue 数据响应式的理解](#a8)

[9:关于vue选项数据之computed和watch的区别](#a9)

---

#### 1:options{#a1}

关于 Vue 的选项 options 分为六大类，分别是**数据，DOM，生命周期钩子，资源，组合，其他**。每类中具体又有很多属性。

数据中有 data，props，propsData，computed，methods，watch；DOM 中有 el，template，render，renderError；

生命周期钩子中有 beforeCreate，created，beforeMount，mounted，beforeUpdate，updated，activated，deactivated，beforeDestroy，destroyed，errorCaptured；

资源中有 directives，filters，components；

组合中有 parent，mixins，extends，provide / inject；

其它中有 name，delimiters，functional，model，inheritAttrs，comments。

---

#### 2：options 之数据{#a2}

选项数据之 data：可以是一个对象或函数，但是组件的定义只接受函数。它就是 vue 实例的数据对象。大概来说，data 应该只能是数据。实例创建完成之后，可以直接使用 vm.$data 去访问原始数据对象。而且vm.$data.a 也可以写成 vm.a。在创建实例的时候，直接给实例添加这个 data 属性，属性值就为需要的数据。另外该属性可以用\$.mount 去代替。

选项数据之 props：可以是数组或对象，用于接收来自父组件的数据。props 可以是简单的数组，或者使用对象作为替代。

选项数据之 methods：methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。注意，不应该使用箭头函数来定义 method 函数，因为箭头函数绑定了父级作用域的上下文。

完整版：
![](/images/task56Vue/vuejs.png)
非完整版：
![](/images/task56Vue/vuejs.png)

查看 [vuejs](https://cn.vuejs.org/v2/guide/installation.html)相关文档。

---

#### 3：options 之 DOM{#a3}

选项 DOM 之 el：可以是一个字符串或元素对象，只在用 new 创建实例时生效。提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
在实例挂载之后，元素可以用 vm.$el 访问。如果在实例化时存在这个选项，实例将立即进入编译过程，否则，需要显式调用 vm.$mount() 手动开启编译。

---

#### 4：options 之 生命周期钩子{#a4}

选项生命周期钩子：可以是一个字符串或元素对象，只在用 new 创建实例时生效。提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
在实例挂载之后，元素可以用 vm.\$el 访问。如果在实例化时存在

---

#### 5：options 之 资源{#a5}

选项资源之 components：对象，包含 Vue 实例可用组件的哈希表。组件是可复用的 Vue 实例，且带有一个名字

---

#### 6：options 之 组合{#a6}

选项组合：

---

#### 7：options 之 其他{#a7}

选项其他：

---

下面是一个具体的例子，主要用到了 data，props，methods，el，components 这些属性。
在 index.html 中引入了 vue.js 和 main.js 文件。如下代码是 main.js 中的代码：

```
//定义了一个全局的组件demo21
Vue.component("demo21", {
  template: `
  <div id="test">
    这是demo2组件里的数据demo2_data:{{ demo2_data }}
    <button @click="surp">点击数字加1</button>
    <hr />
    <span>这是在demo2中使用props给message传值:{{ message }}</span>
  </div>`,
  props: ["message"],
  data() {
    return { demo2_data: 101 };
  },
  methods: {
    surp() {
      this.demo2_data += 1;
    }
  }
});

//创建vue实例
new Vue({
  el: "#myel",
  data: {
    main_data: 100,
    main_arr: [1, 2, 3, 4, 5, 6, 7, 8]
  },
  template: `<div id="main_test">
  <demo21 message="这是外部传值"/><hr>
  这是main_data：{{main_data}}
  <button @click="main_add">点击加1</button>
  </div>`,
  methods: {
    main_add() {
      this.main_data += 1;
    }
  },
  updated() {
    alert("更新啦");
  }
});
```

---

#### 8：vue 数据响应式的理解{#a8}

是时候谈谈 vue 的数据响应式了，这是 vue 最独特的特性之一。下面我用自己的话，谈谈对 vue 数据响应式的理解。当然更权威的还是要参考[深入响应式原理](https://cn.vuejs.org/v2/guide/reactivity.html)官方权威指南。

vue 的数据响应式是当我们对数据进行操作，数据变化后，会对用到数据地方的视图进行更新。

<span style="font-size:24px;font-weight:bold">第一：那首先肯定的是 vue 对这些数据进行了监听，这样数据一变化，vue 就会知道，然后触发视图更新。那么 vue 是怎么实现监听的呢？</span>

<span style="color:red">Vue 会递归将 data 的属性转换为 getter/setter，从而让 data 的属性能够响应数据变化。使用 Object.defineProperty
把（选项 data）中（传入的参数对象）的（属性）全部转化为 getter setter。</span>对于选项 data，可以参考上面的介绍以及[官方文档](https://cn.vuejs.org/v2/api/#data)。

<span style="color:red">这里简单介绍一下 Object.defineProperty,getter,setter 的作用原理。</span>下面举例说明：

对象 mydata，有一个属性 age，值为 16，我们可以直接通过 mydata.age = 12,将其属性值进行更改。代码：

```
let mydata = {age:16};
//对name属性进行修改
mydata.age = 12;//此时mydata的age属性就被更改为12
console.log(mydata.age);console.log(mydata.age);
```

![](/images/task56Vue/data1.png)

可是将 mydata 作为参数（并不是直接作为参数），通过执行下面的 proxy 函数之后，

![](/images/task56Vue/data4.png)

![](/images/task56Vue/data5.png)

![](/images/task56Vue/data6.png)

![](/images/task56Vue/data2.png)

![](/images/task56Vue/data3.png)

这时我们会发现：无论是通过 mydata.age,还是通过 mydata2.age，都不能对 age 属性进行任意更改。对 age 属性的修改必须符合小于 20 才可以。也就是说在我们对 age 属性进行操作时，会被监听到。（这也就是 Vue 实现数据监听的原理。Vue 会递归的将 data 的属性转换为 getter/setter，从而让 data 的属性能够响应数据变化。并且创建的 Vue 实例也代理了 data 对象上所有的属性）

<span style="font-size:24px;font-weight:bold">第二：同时 Vue 会对每个组件实例设置一个 watcher 实例。这个 watcher 实例负责监听在渲染过程中接触到的数据属性。当数据属性变化的时候，也就是 seter 的时候，watcher 实例监听到，然后使相关联的组件重新渲染。</span>

![](/images/task56Vue/render.png)

<span style="font-size:24px;font-weight:bold">第三：上面就是 Vue 数据响应式的基本原理，但这里有几个地方需要注意。</span>

&nbsp;&nbsp;<strong style="font-size:20px;color:red">检测变化的注意事项</strong>：Vue 无法检测到对象属性的添加或删除；Vue 不能检测以下数组的变动》当直接利用索引直接设置一个数组项时或修改数组的长度时。

&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:red">Vue 无法检测到对象属性的添加或删除：Vue 会在初始化实例时对属性执行 getter/setter 转化，所以属性必须在 data 对象上存在才能让 Vue 将它转换为响应式。对于已经创建的实例，Vue 不允许动态添加根级别的响应式属性。如下面例子所示 vm.b。</span>

```
var vm = new Vue({
  data:{
    a:1,
    obj:{name:'gg',age:18}
  }
})
// `vm.a` `vm.obj`是响应式的
vm.b = 2
// `vm.b` 是非响应式的
```

但是，可以使用 Vue.set(object, propertyName, value)方法向嵌套对象添加响应式属性,如 Vue.set(vm.someObject, 'b', 2)。具体还拿上面的例子，可以给 obj 增加一个响应式属性 hobby，还可以使用 vm.\$set 实例方法，这也是全局 Vue.set 方法的别名：

```
Vue.set(vm.someObject, 'b', 2)
this.$set(this.someObject,'b',2);//这两种都可以
//具体举例
Vue.set(vm.obj, 'hobby', "running");
this.$set(this.obj, 'hobby', "running");
```

有时可能需要为已有对象赋值多个新属性，比如使用 Object.assign() 或 \_.extend()。但是，这样添加到对象上的新属性不会触发更新。在这种情况下，用原对象与要混合进去的对象的属性一起创建一个新的对象。

```
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`，
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:red">Vue 不能检测以下数组的变动：当直接利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
或修改数组的长度时，例如：vm.items.length = newLength。举个例子：</span>

```
var vm = new Vue({
data: {
items: ['a', 'b', 'c']
}
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果，同时也将在响应式系统内触发状态更新：

```
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

你也可以使用 vm.\$set 实例方法，该方法是全局方法 Vue.set 的一个别名：vm.\$set(vm.items, indexOfItem, newValue)

为了解决第二类问题，你可以使用 splice：vm.items.splice(newLength)。

&nbsp;&nbsp;<strong style="font-size:20px;color:red">声明响应式属性：</strong>Vue 不允许动态添加根级响应式属性，所以必须要在初始化实例前声明所有根级响应式属性，哪怕只是一个空值。

```
var vm = new Vue({
  data: {
    // 声明 message 为一个空值字符串
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// 之后设置 `message`
vm.message = 'Hello!'
```

如果你未在 data 选项中声明 message，Vue 将警告你渲染函数正在试图访问不存在的属性。

&nbsp;&nbsp;<strong style="font-size:20px;color:red">异步更新队列：</strong>Vue 在更新 DOM 时是异步执行的。只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。为了在数据变化之后等待 Vue 完成更新 DOM，可以在数据变化之后立即使用 Vue.nextTick(callback)。这样回调函数将在 DOM 更新完成后被调用。例如：

```
<div id="example">{{message}}</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

&nbsp;&nbsp;&nbsp;&nbsp;在组件内使用 vm.\$nextTick() 实例方法特别方便，因为它不需要全局 Vue，并且回调函数中的 this 将自动绑定到当前的 Vue 实例上：

```
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: '未更新'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = '已更新'
      console.log(this.$el.textContent) // => '未更新'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => '已更新'
      })
    }
  }
})
```
---
#### 9：关于vue选项数据之computed和watch的区别{#a9}

computed是计算属性，用的时候，直接当属性去使用，不必写括号去调用。会依赖数据的变化去计算属性。计算属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。


watch是监听侦听。当在数据发生变化时，执行函数。watch提供了两个选项，immediate，默认是false，true的话表示设置监听之后被立即调用。deep，默认是false，true的话表示监听会在任何被侦听的对象的property改变时被调用，不论其被嵌套多深。

如果一个数据依赖于其他数据，那么把这个数据设计为computed；如果需要在某个数据变化时做一些事情，使用watch来观察这个数据变化。

下面的例子，有两个展示板块，上面用到的是computed计算属性，下面用到的是watch监听。
![](/images/task56Vue/com.png)
![](/images/task56Vue/comm1.png)
![](/images/task56Vue/comm2.png)
