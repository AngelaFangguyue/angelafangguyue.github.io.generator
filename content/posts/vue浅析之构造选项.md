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

---

#### 1:options{#a1}

关于 Vue 的选项 options 分为六大类，分别是**数据，DOM，生命周期钩子，资源，组合，其他**。每类中具体又有很多属性。

数据中有 data，props，propsData，computed，methods，watch；DOM 中有 el，template，render，renderError；

生命周期钩子中有 beforeCreate，created，beforeMount，mounted，beforeUpdate，updated，activated，deactivated，beforeDestroy，destroyed，errorCaptured；

资源中有 directives，filters，components；

组合中有 parent，mixins，extends，provide / inject；

其它中有 name，delimiters，functional，model，inheritAttrs，comments。

---

#### 2：options 之数据

选项数据之 data：可以是一个对象或函数，但是组件的定义只接受函数。它就是 vue 实例的数据对象。大概来说，data 应该只能是数据。实例创建完成之后，可以直接使用 vm.$data 去访问原始数据对象。而且vm.$data.a 也可以写成 vm.a。在创建实例的时候，直接给实例添加这个 data 属性，属性值就为需要的数据。另外该属性可以用\$.mount 去代替。

选项数据之 props：可以是数组或对象，用于接收来自父组件的数据。props 可以是简单的数组，或者使用对象作为替代。

选项数据之 methods：methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。注意，不应该使用箭头函数来定义 method 函数，因为箭头函数绑定了父级作用域的上下文。

完整版：
![](/images/task56Vue/vuejs.png)
非完整版：
![](/images/task56Vue/vuejs.png)

查看 [vuejs](https://cn.vuejs.org/v2/guide/installation.html)相关文档。

---

#### 3：options 之 DOM

选项 DOM 之 el：可以是一个字符串或元素对象，只在用 new 创建实例时生效。提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
在实例挂载之后，元素可以用 vm.$el 访问。如果在实例化时存在这个选项，实例将立即进入编译过程，否则，需要显式调用 vm.$mount() 手动开启编译。

---

#### 4：options 之 生命周期钩子

选项生命周期钩子：可以是一个字符串或元素对象，只在用 new 创建实例时生效。提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
在实例挂载之后，元素可以用 vm.\$el 访问。如果在实例化时存在

---

#### 5：options 之 资源

选项资源之 components：对象，包含 Vue 实例可用组件的哈希表。组件是可复用的 Vue 实例，且带有一个名字

---

#### 6：options 之 组合

选项组合：

---

#### 7：options 之 其他

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
