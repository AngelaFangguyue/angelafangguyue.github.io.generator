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

# 本篇文章介绍 Vue 实例的构造选项。主要内容有：

[1:关于 options](#a1)

[2:options 之数据](#a2)

&nbsp;&nbsp;&nbsp;&nbsp;[2.1:关于 vue 选项数据之 computed 和 watch 的区别](#a9)

[3:options 之 DOM](#a3)

[4:options 之生命周期钩子](#a4)

[5:options 之资源](#a5)

&nbsp;&nbsp;&nbsp;&nbsp;[5.1:options 之资源-directives 指令以及修饰符](#a10)

&nbsp;&nbsp;&nbsp;&nbsp;[5.2:Vue 模板的特点](#a11)

[6:options 之组合](#a6)

&nbsp;&nbsp;&nbsp;&nbsp;[6.1:options 之组合-mixins](#a12)

&nbsp;&nbsp;&nbsp;&nbsp;[6.2:options 之组合-extends](#a13)

&nbsp;&nbsp;&nbsp;&nbsp;[6.3:options 之组合-provide/inject](#a14)

[7:options 之其他](#a7)

[8:vue 数据响应式的理解](#a8)

[10:关于 vue 选项资源之 directives 指令](#a10)

---

# 1:options{#a1}

关于 Vue 的选项 options 分为六大类，分别是**数据，DOM，生命周期钩子，资源，组合，其他**。每类中具体又有很多属性。

数据中有 data，props，propsData，computed，methods，watch；DOM 中有 el，template，render，renderError；

生命周期钩子中有 beforeCreate，created，beforeMount，mounted，beforeUpdate，updated，activated，deactivated，beforeDestroy，destroyed，errorCaptured；

资源中有 directives，filters，components；

组合中有 parent，mixins，extends，provide / inject；

其它中有 name，delimiters，functional，model，inheritAttrs，comments。

---

# 2：options 之数据{#a2}

选项数据之 data：可以是一个对象或函数，但是组件的定义只接受函数。它就是 vue 实例的数据对象。大概来说，data 应该只能是数据。实例创建完成之后，可以直接使用 vm.$data 去访问原始数据对象。而且vm.$data.a 也可以写成 vm.a。在创建实例的时候，直接给实例添加这个 data 属性，属性值就为需要的数据。另外该属性可以用\$.mount 去代替。

选项数据之 props：可以是数组或对象，用于接收来自父组件的数据。props 可以是简单的数组，或者使用对象作为替代。

选项数据之 methods：methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。注意，不应该使用箭头函数来定义 method 函数，因为箭头函数绑定了父级作用域的上下文。

## 2.1：关于 vue 选项数据之 computed 和 watch 的区别{#a9}

computed 是计算属性，用的时候，直接当属性去使用，不必写括号去调用。会依赖数据的变化去计算属性。计算属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。

watch 是监听侦听。当在数据发生变化时，执行函数。watch 提供了两个选项，immediate，默认是 false，true 的话表示设置监听之后被立即调用。deep，默认是 false，true 的话表示监听会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深。

如果一个数据依赖于其他数据，那么把这个数据设计为 computed；如果需要在某个数据变化时做一些事情，使用 watch 来观察这个数据变化。

下面的例子，有两个展示板块，上面用到的是 computed 计算属性，下面用到的是 watch 监听。
![](/images/task56Vue/com.png)
![](/images/task56Vue/comm1.png)
![](/images/task56Vue/comm2.png)

---

# 3：options 之 DOM{#a3}

选项 DOM 之 el：可以是一个字符串或元素对象，只在用 new 创建实例时生效。提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
在实例挂载之后，元素可以用 vm.$el 访问。如果在实例化时存在这个选项，实例将立即进入编译过程，否则，需要显式调用 vm.$mount() 手动开启编译。

---

# 4：options 之 生命周期钩子{#a4}

选项生命周期钩子：可以是一个字符串或元素对象，只在用 new 创建实例时生效。提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
在实例挂载之后，元素可以用 vm.\$el 访问。如果在实例化时存在

---

# 5：options 之 资源{#a5}

选项资源之 components：对象，包含 Vue 实例可用组件的哈希表。组件是可复用的 Vue 实例，且带有一个名字

## 5.1：options 之资源-directives 指令以及修饰符{#a10}

<strong style="font-size:16px;color:blue">在使用 Vue 的时候，我们经常在 template 中会看到这样的模板语句：\<a v-bind:href="path">\</a>或者\<div v-html="x">\</div>;语句中带有 v-的这些就叫做指令。它的语法是：v-指令名:参数=值，如 v-on:click=add。如果值里没有特殊符号，就可以不加引号，就像上面的 add。有些指令没有参数和值，如 v-pre。有些指令可以没有值，如 v-on:click.prevent（这个指令的意思后面介绍修饰符的时候具体说）。常见的指令有 v-bind,v-on,v-if,v-for。</strong>具体可以查看官方文档：[模板语法之指令](https://cn.vuejs.org/v2/guide/syntax.html#%E6%8C%87%E4%BB%A4)以及[指令](https://cn.vuejs.org/v2/api/#%E6%8C%87%E4%BB%A4)

**1>>>v-text:**表达式{{XXX}}，XXX 可以是表达式，调用函数，任何运算,如{{n+1}},{{fn()}}。如果里面的值是 undefined 或 null，那么就不显示。用法：\<div>{{x}}\</div>该句话还有另一种写法\<div v-text="x">\</div>

**2>>>v-html:**像上面\<div v-text="x">\</div>，若 x 为\<strong>hi\</strong>,即想在 div 中输出一个加粗的 hi。此时使用 v-html 指令，即\<div v-html="x">\</div>

**3>>>v-pre:**如果想要在页面上输出{{x}}的字样，那要用到指令 v-pre，即\<div v-pre>{{x}}\</div>。v-pre 的意思是不会对模板进行编译

**4>>>v-bind:**绑定属性,如\<img v-bind:src=path/>,也可写成\<img :src=path/>

**5>>>v-on:**绑定事件,如\<div v-on:click=fn>\</div>,也可写成\<div @click=fn>\</div>

**6>>>v-if:**条件判断,如\<div v-if="x>0">x 大于 0\</div>\<div v-else-if="x===0">x 等于 0\</div>\<div v-if="x<0">x 小于 0\</div>。若 x 大于 0，则显示 x 大于 0。

**7>>>v-for:**循环,for (value,key) in 对象或数组。如

```
<ul><li v-for="(u,index) in users" :key="index">索引：{{index}} 值：{{u.name}}</li></ul>
//////////////////////////
<ul><li v-for="(value,key) in obj" :key="key">索引：{{key}} 值：{{value}}</li></ul>
```

**8>>>v-show:**显示隐藏,如\<div v-show="n%2===0">n 是偶数\</div>

**9>>>v-model:**

**10>>>v-slot:**

**11>>>v-cloak:**

**12>>>v-once:**

**13>>>v-once:**

**14>>>自定义指令:**指令的作用主要是用来进行 DOM 操作。如果某个 DOM 操作需要经常使用，或者莫格 DOM 操作比较复杂，那么就可以封装成为指令，减少重复。

可以定义一个全局的自定义指令，也可以定义一个只在组件内使用的局部指令。例如定义一个全局的自定义指令 v-sh,它所实现的功能是组件绑定该指令后，点击会输出字符‘hi’：

```
Vue.directive('sh',{
  inserted(el){
    el.addEventListener("click",()=>{console.log("hi");});
  }
});
```

如果想注册局部指令，组件中也接受一个 directives 的选项：

```
directives:{
  sh:{
      inserted(el){
      el.addEventListener("click",()=>{console.log("hi");});
    }
  }
}
```

这样就可以在模板上的任何元素中使用该自定义组件：

```
<div v-sh>点击会打印：hi</div>
```

Vue 实例/组件主要是用来进行数据绑定，事件监听，DOM 更新（这里 DOM 事件更新用到的是 Vue 里内带的事件监听，自动更新 UI）。

<strong style="font-size:16px;color:blue">修饰符是由点开头的指令后缀来表示的。</strong>具体可以参考官方的这几篇文章：[事件&按键修饰符](https://cn.vuejs.org/v2/guide/render-function.html#%E4%BA%8B%E4%BB%B6-amp-%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6),[事件修饰符](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6),[按键修饰符](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6),[系统修饰键](https://cn.vuejs.org/v2/guide/events.html#%E7%B3%BB%E7%BB%9F%E4%BF%AE%E9%A5%B0%E9%94%AE)

**1>>>事件修饰符：**Vue 为 v-on 提供了事件修饰符，常见的事件修饰符有.stop，.prevent，.capture，.self，.once，.passive。

.stop，与处理函数中的操作 event.stopPropagation()是等价的，即阻止事件冒泡。.prevent，与处理函数中的操作 event.event.preventDefault()是等价的，即阻止默认行为。如\<a @click.prevnet href="https://www.baidu.com/"> 百度\</a>,添加一个.prevent 修饰符就会阻止点击 a 标签，跳转到百度网页。还有下面的例子：

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
```

**2>>>按键修饰符：**Vue 为 v-on 在监听键盘事件时添加了按键修饰符,在使用按键修饰符的时候，我们可以直接写按键的 ascii 码，也可以使用别名。如\<input @keypress.13="fn"/>也可以写成\<input @keypress.enter="fn"/>即在 input 输入框中按下回车键会触发 fn 函数。为了在必要的情况下支持旧浏览器，Vue 提供了绝大多数常用的按键码的别名：.enter，.tab，.delete (捕获“删除”和“退格”键)，.esc，.space，.up，.down，.left，.right。

```
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
//你可以直接将 KeyboardEvent.key 暴露的任意有效按键名转换为 kebab-case 来作为修饰符。
<input v-on:keyup.page-down="onPageDown">
//在上述示例中，处理函数只会在 $event.key 等于 PageDown 时被调用。
```

你还可以通过全局 config.keyCodes 对象自定义按键修饰符别名：

```
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

**3>>>系统修饰键：**可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器，.ctrl，.alt，.shift，.meta。例如：

```
<!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

请注意修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。而单单释放 ctrl 也不会触发事件。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17。

**4>>>自定义事件之.sync 修饰符：**在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。但真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。具体参考[.sync 修饰符](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6)。

就像下面图片中的例子，父组件 App 中有一个 num 属性，初始值为 0。另在父组件中使用了\<Child :sonnum.sync="num"/>。

子组件 Child 有一个 sonnum 的 prop，它接收父组件的 num，所以初始值也为 0。子组件里有两个按钮，都设置的有点击事件。第一个按钮是：点击之后，将 sonnum 加 10，在点击事件中使用了 this.\$emit。第二个是点击之后，将 sonnum 加 20，就是一个简单函数。

点击第二个加 20 的按钮，子组件里的数据会从 0 变为 20，但父组件里的 num 仍是 0。同时控制台会报警。点击第一个加 10 的按钮，这时子组件里的数据加 10，变为 30，同时父组件接收到这个 num 的值在子组件里变化，变为 30，它也会更新为 30。且没有任何报错或者警告信息。

这是父组件:注意在父组件中使用了\<Child :sonnum.sync="num"/>
![](/images/task56Vue/sync.png)
![](/images/task56Vue/sync1.png)
这是子组件：
![](/images/task56Vue/sync2.png)
![](/images/task56Vue/sync8.png)
![](/images/task56Vue/sync9.png)
这是 main.js：
![](/images/task56Vue/sync4.png)
点击第二个加 20 的按钮，子组件里的数据会从 0 变为 20，但父组件里的 num 仍是 0。同时控制台会报警。
![](/images/task56Vue/sync6.png)
点击第一个加 10 的按钮，这时子组件里的数据加 10，变为 30，同时父组件接收到这个 num 的值在子组件里变化，变为 30，它也会更新为 30。
![](/images/task56Vue/sync7.png)

上面会变化的原因就是使用了.sync 修饰符。该修饰符作为编译时的语法糖存在，会扩展成为一个自动更新父组件属性的 v-on 监听器，即当一个子组件改变了一个 prop 的值时，父组件能够监听到该变化，并且这个变化也会同步到父组件中。父组件可以监听\$emit 触发的事件并根据需要更新一个本地的数据属性。

## 5.2：Vue 模板的特点{#a11}

使用 XML 语法，使用{{}}插入表达式，使用 v-html v-on v-bind 等指令操作 DOM，使用 v-if v-for 等指令实现条件判断和循环。

# 6：options 之 组合{#a6}

## 6.1：options 之组合-mixins {#a12}

mixins 选项接收一个混入对象的数组。这些混入对象可以像正常的实例对象一样包含实例选项，这些选项将会被合并到最终的选项中。像上面例子，created 选项是会合并的，使用的是和 Vue.extend() 一样的选项合并逻辑。也就是说，如果你的混入包含一个 created 钩子，而创建组件本身也有一个，那么两个函数都会被调用。Mixin 钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用。

```
var mixin = {
  data(){
    return {name:"在混入组件中设置的name"}
  },
created: function () { console.log(this.name) }
}
var vm = new Vue({
  created: function () { console.log(2) },
  mixins: [mixin]
})
// => 在混入组件中设置的name
// => 2
```

也可以使用全局混入即 Vue.mixin();具体可以参考官方文档[全局混入](https://cn.vuejs.org/v2/guide/mixins.html#ad)，但要谨慎使用，一般不推荐。

## 6.2：options 之组合-extends {#a13}

允许声明扩展另一个组件(可以是一个简单的选项对象或构造函数)，而无需使用 Vue.extend。这主要是为了便于扩展单文件组件。这和 mixins 类似。

```
var CompA = { ... }
// 在没有调用 `Vue.extend` 时候继承 CompA
var CompB = {
  extends: CompA,
  ...
}
```

另外还可以使用全局方法 Vue.extend(),即基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。data 选项是特例，需要注意 - 在 Vue.extend() 中它必须是函数。

```
<div id="mount-point"></div>
// 创建构造器
var Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})
// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
结果如下：
<p>Walter White aka Heisenberg</p>
```

## 6.3：options 之组合-provide/inject {#a14}

这是一个前任栽树，后人乘凉的操作。可以大范围，隔 n 代共享信息。这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。provide 选项应该是一个对象或返回一个对象的函数。inject 选项应该是：一个字符串数组，或一个对象。

```
// 父级组件提供 'foo'
var Provider = {
  provide: {
    foo: 'bar'
  },
  // ...
}
// 子组件注入 'foo'
var Child = {
  inject: ['foo'],
  created () {
    console.log(this.foo) // => "bar"
  }
  // ...
}
```

# 7：options 之 其他{#a7}

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

# 8：vue 数据响应式的理解{#a8}

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

&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-weight:bold">1》Vue 无法检测到对象属性的添加或删除：Vue 会在初始化实例时对属性执行 getter/setter 转化，所以属性必须在 data 对象上存在才能让 Vue 将它转换为响应式。对于已经创建的实例，Vue 不允许动态添加根级别的响应式属性。如下面例子所示 vm.b。</span>

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

&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-weight:bold">2》Vue 不能检测以下数组的变动：当直接利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
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
