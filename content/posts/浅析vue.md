---
title: "浅析vue"
date: 2020-02-17T17:38:54+08:00
draft: false
categories:
  - vue
tags:
  - vue
featured_image:
---

本篇文章是关于 vue 的介绍之一，主要讲一下 Vue 两个版本的区别和使用方法。

---

#### 1：两个版本对应的文件名

完整的 vue 版本名字是 vue.js,非完整版本也就是仅运行时版本名字是 vue.runtime.js，加了一个 runtime。

当我们在生产环境的时候，即把代码打包上线上正式环境使用，无论用的是完整版本还是非完整版本，都要使用加了 mini 的版本（即 vue.mini.js 或者 vue.runtime.mini.js）。因为加了 mini 的，会去掉注释，体积变小。

完整版：
![](/images/task56Vue/vuejs.png)
非完整版：
![](/images/task56Vue/vuejs.png)

建议我们在开发的时候，使用非完整版本，当然这个是视实际开发情况来看，仅仅是建议。

使用 vue 的时候，我们可以直接 cdn 引入，即在 html 文件中使用 script 标签去引入。也可以使用 webpack 引入，默认使用 webpack 引入的是非完整版，如果要使用完整版本的话，需要配置 alias。还可以使用@vue/cli 引入，这种方法默认也是非完整版，想要使用完整版本，可以去查看 [vuejs](https://cn.vuejs.org/v2/guide/installation.html)相关文档。

---

#### 2：template 和 render 怎么用

2.1》template：string 类型；一个字符串模板；作为 Vue 实例的标识使用。

&nbsp;&nbsp;&nbsp;&nbsp;如果引用的是完整版的 vue，那么在 new Vue 的时候，里面的参数对象可以有一个 template 的属性，值就是 html 内容。如下：

```
  new vue({
    template: `<div id="app1" class="app">
    <div>
      <span id="number">HELLO WORLD</span>
    </div>
    </div>`
  });

```

&nbsp;&nbsp;&nbsp;&nbsp;如果引用的是非完整版的 vue，那么是在新建的 vue 文件中，将 html 内容写在 template 的标签中。如下：

```
<template>
  <div id="app1" class="app">
    <div>
      <span id="number">HELLO WORLD</span>
    </div>
    </div>
</template>
```

2.2》render：函数类型，字符串模板的代替方案。该渲染函数接收一个 createElement 方法作为第一个参数用来创建 VNode。（createElement 到底会返回什么呢？其实不是一个实际的 DOM 元素。它更准确的名字可能是 createNodeDescription，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“VNode”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。）

&nbsp;&nbsp;&nbsp;&nbsp;使用完整版本，在创建 Vue 实例的时候，是不需要写 render 属性的。使用非完整版本的话，如果使用了 vue 单文件组件模板的话，也是不需要使用 render。只有在使用非完整版 vue，并且是用 js 构建视图的时候，在 new Vue 的时候，要添加 render 属性，如下图：

```
 new Vue({
   render(h) {
     return h("div", [
    '先写一些文字',
    h('span', 'HELLO WORLD')] );
  },
 });
```

---

#### 3：用 codesandbox.io 写 Vue 代码

1：进入[codesandbox](https://codesandbox.io/);看到如下界面,点击红线标注的地方；
![](/images/task56Vue/cs1.png)
2：如下图，第一个红线标注的是创建一个 Sandbox,根据自己需要选择合适的框架，图中点击第二个红线标注的 vue，表示创建一个 vue 项目，
![](/images/task56Vue/cs2.png)
3：如下图，进入创建的 vue 项目中，可以进行编辑，如果需要的话，，点击图中的 file，选择 Export to ZIP 导出项目到本地：
![](/images/task56Vue/cs3.png)
