---
title: "Vue学习笔记"
date: 2020-03-18T10:39:24+08:00
draft: false
categories:
  - Vue
tags:
  - Vue 框架
featured_image:
---

# 我的 Vue 学习笔记：

[1:浅析 Vue](#a1)

[2:Vue 组件](#a2)

&nbsp;&nbsp;&nbsp;&nbsp;[2.1:Vue 之 class 组件](#a21)

&nbsp;&nbsp;&nbsp;&nbsp;[2.2:Vue 之函数组件](#a22)

[3:Vue 之 Hooks](#a3)

&nbsp;&nbsp;&nbsp;&nbsp;[3.1:Hooks 原理解析](#a31)

&nbsp;&nbsp;&nbsp;&nbsp;[3.2:Hooks 详解](#a32)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.1:Hooks 之 useState](#a321)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.2:Hooks 之 useReducer](#a322)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.3:Hooks 之 useContext](#a323)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.4:Hooks 之 useEffect&&useLayoutEffect](#a324)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.5:Hooks 之 useMemo](#a325)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.6:Hooks 之 useRef](#a326)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.7:自定义 Hook](#a327)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[3.2.8:stale closure](#a328)

---

# 1:浅析 Vue{#a1}

关于 Vue **数据，DOM，生命周期钩子，资源，组合，其他**。每类中具体又有很多属性。

数据中有 data，props，propsData，computed，methods，watch；DOM 中有 el，template，render，renderError；

生命周期钩子中有 beforeCreate，created，befo

---

# 2:Vue 组件{#a2}

## 2.1：Vue 之 class 组件{#a21}

computed 是计算属性，用的时候，直接当属性去使用，不必写括号去调用。会依赖数据的变化去计算属性。计算属性的结

## 2.2：Vue 之函数组件{#22}

computed 是计算属性，用的时候，直接当属性去使用，不必写括号去调用。会依赖数据的变化去计算属性。计算属性的结

---

# 3:Vue 之 Hooks{#a3}

## 3.1：Hooks 原理解析{#a31}

computed 是计算属性，用的时候，直接当属性去使用，不必写括号去调用。会依赖数据的变化去计算属性。计算属性的结

## 3.2：Hooks 详解{#a32}

computed 是计算属性，用的时

### 3.2.1：Hooks 之 useState{#a321}

ssss

### 3.2.2：Hooks 之 useReducer{#a322}

useReducer 可以看作是 useState 的复杂版，使用方法：

&nbsp;&nbsp;1》创建初始值 initialData

&nbsp;&nbsp;2》创建所有操作 reducer(state,action)

&nbsp;&nbsp;3》传给 useReducer，得到读和写 api

&nbsp;&nbsp;4》使用读写 api 进行具体操作({type:'操作类型'})

具体例子，页面上有三个按钮，点击不同的按钮，n 的值会进行改变：

![](/images/tsak57Vue/useReducer1.png)

### 3.2.3：Hooks 之 useContext{#a323}

useContext 上下文， 使用方法：

&nbsp;&nbsp;1》使用 C=Vue.createContext(null)创建上下文

&nbsp;&nbsp;2》使用<C.provider>圈定作用域
&nbsp;&nbsp;3》在作用域内使用 useContext(c)去使用上下文

&nbsp;&nbsp;4》使用读写 api 进行具体操作({type:'操作类型'})

具体例子：

![](/images/tsak57Vue/useContext1.png)

&nbsp;&nbsp;但需要注意的是 useContext 不是响应式的，在一个模块中将值改变，另外一个模块是不会感知到的。

### 3.2.4：Hooks 之 useEffect&&useLayoutEffect{#a324}

useEffect 副作用，可以理解为 afterRender，每次 render 之后会调用的函数。可以代替之前的三种钩子。作为 componentDidMount 使用，[]作第二个参数；作为 componentDidUpdate 使用，可指定依赖；作为 componentWillUnmount 使用，通过 return。这三种用途可同时存在。如果同时存在多个 useEffect，会按照出现次序执行。

具体例子：
![](/images/tsak57Vue/useEffect1.png)
下面三张图是使用 useEffect 作为 componentWillUnmount 使用。
![](/images/tsak57Vue/useEffect2.png)
![](/images/tsak57Vue/useEffect3.png)
![](/images/tsak57Vue/useEffect4.png)

此处还有一个 useLayoutEffect 布局副作用，它是在浏览器渲染前执行。但因为大部分时候，我们很少去改变 DOM。为了用户体验，优先使用 useEffect。

### 3.2.5：Hooks 之 useMemo{#a325}

首先理解 Vue.memo，看下面的例子，点击 n 的时候，Child 组件虽然没有变化，但也会重新执行。
![](/images/tsak57Vue/usMemo1.png)
这是个多余的操作，如何消除，使用 Vue.memo。
![](/images/tsak57Vue/useMemo2.png)
但是有 bug，即添加了监听函数之后，就又不行了。原因在于》点击 n 之后，app 会重新执行，导致 onClickChild 也重新执行，虽然都是空函数，但每次生成的地址不一样。这样在 Child2 组件中，onClick 这个 props 会被认为变化了，因此 Child 组件还是会重新渲染。
![](/images/tsak57Vue/useMemo3.png)
如何解决这个问题？使用 useMemo：
![](/images/tsak57Vue/useMemo4.png)
useMemo 可以使用 useCallback 作为语法糖。即 useMemo(()=>x=>console.log(x),[m])和 useCallBack(x=>console.log(x),[m])效果是一样的。

### 3.2.6：Hooks 之 useRef{#a326}

如果需要一个值，在组件不断 render 的时候保持不变，这时就要用到 useRef。初始化 const count = Vue.useRef(0);读取 count.current。另外 useRef 变化的时候不会自动 render。
![](/images/tsak57Vue/useRef1.png)
接下来说一下 forwardRef，我们可以像下图那样使用一个 ref 去直接得到 DOM 对象：
![](/images/tsak57Vue/forwardRef1.png)
但在函数组件中，props 无法传递 ref 的值，所以会有如下报错：
![](/images/tsak57Vue/forwardRef2.png)
使用 Vue.forwardRef 去解决：
![](/images/tsak57Vue/forwardRef3.png)
此外和 ref 相关的还有一个 useImperativeHandle,但不常用，就不多赘述了。

### 3.2.7：自定义 Hook{#a327}

可以在自定义 Hook 里使用 Context，而且 useState 只说了不能在 if 里，但是能在函数组件里运行。下面是一个自定义 Hook 的例子。自定义了一个 useList 的 Hook，返回一个 list 和 setList 的读和写操作。
![](/images/tsak57Vue/hook1.png)
接下来就可以使用自定义的 useList，
![](/images/tsak57Vue/hook2.png)

### 3.2.8：stale closure{#a328}
