---
title: "React学习笔记"
date: 2020-03-18T10:39:24+08:00
draft: false
categories:
  - React
tags:
  - React 框架
featured_image:
---

# 我的 React 学习笔记：

[1:浅析 React](#a1)

&nbsp;&nbsp;&nbsp;&nbsp;[1.1:React 简介](#a11)

&nbsp;&nbsp;&nbsp;&nbsp;[1.2:JSX 简介](#a12)

&nbsp;&nbsp;&nbsp;&nbsp;[1.3:使用 React](#a13)

[2:React 组件](#a2)

&nbsp;&nbsp;&nbsp;&nbsp;[2.1:React 之 函数 组件](#a21)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[2.1.1:函数组件 之 state 和 props](#a211)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[2.1.2:函数组件 之 生命周期](#a212)

&nbsp;&nbsp;&nbsp;&nbsp;[2.2:React 之 class 组件](#a22)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[2.2.1:class 组件 之 state 和 props](#a221)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[2.2.2:class 组件 之 生命周期](#a222)

[3:React 之 Hooks](#a3)

&nbsp;&nbsp;&nbsp;&nbsp;[3.1:Hooks 概览](#a31)

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

# 1:浅析 React{#a1}

## 1.1：React 简介{#a11}

React 是一个声明式，高效且灵活的用于构建用户界面的 JavaScript 库。使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”。一次学习，随处编写。

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 并没有采用将标记与逻辑进行分离到不同文件这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现关注点分离。

所以 React 建议使用 JSX 语法。

## 1.2：JSX 简介{#a12}

JSX 是一个 JavaScript 的语法扩展。建议在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模版语言，但它具有 JavaScript 的全部功能。

JSX 可以生成 React “元素”。

React 不强制要求使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。

使用 JSX，只需要在在页面中添加这个 script 标签：

```
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

现在，你可以在任何 script 标签内使用 JSX，方法是在为其添加 type="text/babel" 属性。

另外也可以使用 webpack 和 babel-loader 去引入 JSX。

## 1.3：使用 React{#a13}

使用 React，一可以考虑把 React 作为普通的 script 标记添加到 HTML 页面上，以及使用可选的 JSX。另外就是使用工具链。工具链的话，React 团队主要推荐这些解决方案：

    如果你是在学习 React 或创建一个新的单页应用，请使用 Create React App。
    如果你是在用 Node.js 构建服务端渲染的网站，试试 Next.js。
    如果你是在构建面向内容的静态网站，试试 Gatsby。
    如果你是在打造组件库或将 React 集成到现有代码仓库，尝试更灵活的工具链。

作为新手，建议直接使用官方推荐的工具链 create-react-app 开始练习。

---

# 2:React 组件{#a2}

组件可被拆分为不同的功能片段，这些片段可以在其他组件中使用。组件可以返回其他组件、数组、字符串和数字。

React 组件允许将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。从概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素。

React 组件是可复用的小的代码片段，它们返回要在页面中渲染的 React 元素。

React 的组件可以定义为 class 或函数的形式。如下所示。
需要注意的是： 组件名称必须以大写字母开头。React 会将以小写字母开头的组件视为原生 DOM 标签。例如，<div /> 代表 HTML 的 div 标签，而 <Welcome /> 则代表一个组件，并且需在作用域内使用 Welcome。

## 2.1：React 之函数组件{#a21}

React 组件的最简版本是，一个返回 React 元素的普通 JavaScript 函数，即函数形式：

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

该函数是一个有效的 React 组件，因为它接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

在函数组件中，外部数据是通过参数 props 传递的，但没有 state 和生命周期。React v16.8.0 推出了 Hooks API，其中的 useState 这个 API 可以解决了 state 问题，useEffect 解决了生命周期问题。函数组件中的 Hooks，使得目前的函数组件功能更加丰富。在后面的第 3 部分将详细介绍 React 的常用 Hooks。

### 2.1.1：函数组件 之 state 和 props{#a211}

声明一个函数，它就是组件。可以用函数声明，也可以用函数表达式的方法。使用 useState 去读写 state，使用 props 去获取外部数据

```
function Welcome(props){
  let [n,setn] = useState(10);//使用useState初始化了一个变量n，值为10。且使用setn修改n。
//也可以将n加10的操作写成一个函数
  let addTen(){
    setn(n+a10);
  };
  return <div>
  <h1>Hello,{props.name}</h1>//使用参数props去接收外面的传参
  <span>{n}</span}
  <button onClick={setn(n+10)}>给n加10</button>//使用setn修改n的值
  //使用addTen方法给n加10
  <button onClick={addTen}>给n加10</button>//使用setn修改n的值
  </div>
}
//使用方法：
<Welcome name="Jack"></Welcome>
```

### 2.1.2：函数组件 之 生命周期{#a212}

使用 useEffect 去模拟 class 组件的生命周期

函数组件执行的时候，就相当于 constructor。shouldComponentUpdate 这个生命周期可以用后面的 React.memo 和 useMemo 可以解决。函数组件的返回值就是 render 的返回值。

1：模拟 componentDidMount

useEffect(()=>{console.log("第一次渲染")},[])

2：模拟 componentDidUpdate

useEffect(()=>{console.log("任意属性改变")})
useEffect(()=>{console.log("n 变了")},[n])

3：模拟 componentWillUnmount

useEffect(()=>{
console.log("第一次渲染");
return ()=>{
console.log("组件要死了");
}})

但现在有个问题，在 class 组件中使用 componentDidUpdate 这个钩子时，首次渲染是不会执行的，但是我们在函数组件中用 useEffect 去模拟该生命周期的时候，首次渲染是会执行的。这个问题怎么解决？可以自定义 Hooks，这个在第三部分详细讲解。

## 2.2：React 之 class 组件{#a22}

也可以使用 ES6 的 class 编写，即 class 形式：

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

如需定义 class 组件，需要继承 React.Component，且在 React.Component 的子类中有个必须定义的 render() 函数。

### 2.2.1：class 组件之 state 和 props{#a221}

在 class 组件中，添加 state（内部数据），添加 props（外部数据），需要在 constructor 中进行初始化，如下图给组件 B 传递 props：

```
//声明了一个父组件Parent
class Parent extends React.component{
  constructor(){
    this.state = {name:'parent',n:100};
  }//如果要初始化数据，即Parent组件里要有一个对象{name:'parent-component'}，就要在costructor这里初始化
  onClick2 = ()=>{this.setState((state)=>({n:state.n+1}))};
  onclick3 = ()=>{this.state.n +=1;this.setState(this.state)}
  onClick= ()=>{console.log("这里纯属测试")};
  render(){
    return (<div onClick={onClick2}>hi,this is {this.state.name}。
    <Son name={this.state.name} onClick1={this.onClick} >hihihi</Son>
    </div>);
  }
}
//声明了一个子组件Son
class Son extends React.component{
  constructor(props){
    super(props);
    this.state = {name:'son'};
  }
  render(){
    return (<div onClick={this.props.onClick1}>hi,this is {this.state.name},my father is {this.props.name}
    <div>{this.state.children}</div>
    </div>);
  }
}
```

在上面 Son 组件中，通过在 constructor 中 super(props)来初始化接收外部数据。通过 this.props.XXX 来读取。但是注意 props 由于是外部传进来的，因此理论上是不允许进行修改的。这需要自己写代码的时候规范。

在 constructor 中初始化 state，this.state 后面要跟对象。对 state 的读写，读用 this.state，写使用 this.setState(newState,fn)，注意 setState 不会立刻改变 this.state，会在当前代码运行完后，再去更新 this.state，从而触发 UI 更新。this.setState((state,props)=>newState,fn)这种方式的 state 更容易理解，fn 会在写入成功后执行。

请注意 parent 组件中的 onClick2 和 onClick3 函数。一般是将一个新的 state 传递给 setState，但是如果将原来的 state 传进去，也是可以运行的。即 onClick3 也是可以的，但原则上并不提倡这样做。（在函数组件中，这样的话，React 会认为该对象没有变，是没有效果的，具体可以看下面函数组件的分析）

在 parent 组件中，state 里既有 name，又有 n，但是在设置点击事件改变 state 的时候，onClick2 函数中只传了 n，没有传 name，React 是会默认 shallow merge，自动将新 state 和旧 state 进行一级合并。（只会进行一级合并，但是在函数组件中，一级合并也不会，具体可以看下面函数组件的分析）

### 2.2.2：class 组件之 生命周期{#a222}

生命周期钩子：所谓钩子其实就是一个函数，在特定的时间被调用。

挂载：当组件实例被创建并插入 DOM 中时，其主要的生命周期调用顺序如下：constructor()-->render()-->componentDidMount()。
注意:下述生命周期方法即将过时，在新代码中应该避免使用它们：UNSAFE_componentWillMount()

更新：当组件的 props 或 state 发生变化时会触发更新。组件更新的主要的生命周期调用顺序如下：shouldComponentUpdate()-->render()-->componentDidUpdate()
当 shouldComponent 这个生命周期钩子 return false 的时候，不会调用 render 和 componentDidUpdate，后续可能会有改变，详细的可去 React 官网查询。
注意:下述方法即将过时，在新代码中应该避免使用它们：
UNSAFE_componentWillUpdate()UNSAFE_componentWillReceiveProps()

卸载：当组件从 DOM 中移除时会调用如下方法：componentWillUnmount()

常见的生命周期方法介绍：

1：constructor()
用途:初始化 props，初始化 state，但此处不能调用 setState，用来写 bind this

```
constructor(props) {
  super(props);
  // 不要在这里调用 this.setState()
  this.state = { counter: 0 };
  this.handleClick = this.handleClick.bind(this);
}
//还可以用新的语法：
constructor(props) {
  super(props);
  // 不要在这里调用 this.setState()
  this.state = { counter: 0 };
}
  this.handleClick = ()=> {};
```

如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。

2：render()

用途:展示视图。只能有一个根元素，如果有多个根元素的话，要用 React.Fragment 包起来，该标签可以缩写成<></>。render 里面可以写如 if-else，?:表达式，数组.map 循环等任意的 js 语法表达式。
render() 方法是 class 组件中唯一必须实现的方法。
当 shouldComponent 这个生命周期钩子 return false 的时候，不会调用 render。

3：componentDidMount()

用途:在元素插入页面后执行代码，这些代码依赖 DOM。比如如果想要获取元素的高度，最好在这里写。官方推荐在此处发起加载数据的 AJAX 请求。首次渲染会执行此钩子。这里也适合添加订阅，如果添加了订阅，请不要忘记在 componentWillUnmount() 里取消订阅。

4：componentWillReceiveProps()
当组件接收新的 props 时，会触发此钩子。该钩子已经被弃用，更名为 UNSAFE_componentWillReceiveProps。该钩子之前是推荐使用的，这里简单介绍一下。

5：ShouldComponentUpdate()和 pureComponent 组件

用途:根据 shouldComponentUpdate() 的返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。默认行为是 state 每次发生变化组件都会重新渲染。

当 props 或 state 发生变化时，shouldComponentUpdate() 会在渲染执行之前被调用。返回值默认为 true。

首次渲染或使用 forceUpdate() 时不会调用该方法。此方法仅作为性能优化的方式而存在。不要企图依靠此方法来“阻止”渲染，因为这可能会产生 bug。

你应该考虑使用内置的 PureComponent 组件，而不是手动编写 shouldComponentUpdate()。PureComponent 会对 props 和 state 进行浅层比较，并减少了跳过必要更新的可能性。

6：componentDidUpdate()
用途:在视图更新后执行代码。此处也可以发起 AJAX 请求，用于更新数据。首次渲染不会执行此钩子。在此处 setState 可能会引起无限循环，除非放在 if 里。若 shouldComponentUpdate 返回 false，则不触发此钩子。

7：componentWillUnmount()

用途:组件将要被移出页面然后被销毁时执行代码。unmount 过的组件不会再次 mount。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 componentDidMount() 中创建的订阅等。componentWillUnmount() 中不应调用 setState()，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。

# 3:React 之 Hooks{#a3}

Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

## 3.1：Hooks 概览{#a31}

Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数。Hook 不能在 class 组件中使用 —— 这使得我们不使用 class 也能使用 React。Hook 在 class 内部是不起作用的。我们可以使用它们来取代 class 。

基础的 Hook 有 useState，useEffect，useContext，额外的 Hook 有 useReducer，useCallback，useMemo，useRef，useImperativaHandle，useLayoutEffect，useDebugValue。

## 3.2：Hooks 详解{#a32}

### 3.2.1：Hooks 之 useState{#a321}

看这个例子以及解释：

```
  import React, { useState } from 'react';
  function Example() {
    const [count, setCount] = useState(0);
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
                 Click me
        </button>
      </div>
    );
  }
```

    第一行: 引入 React 中的 useState Hook。它让我们在函数组件中存储内部 state。
    第四行: 在 Example 组件内部，我们通过调用 useState Hook 声明了一个新的 state 变量。它返回一对值给到我们命名的变量上。我们把变量命名为 count，因为它存储的是点击次数。我们通过传 0 作为 useState 唯一的参数来将其初始化为 0。第二个返回的值本身就是一个函数。它让我们可以更新 count 的值，所以我们叫它 setCount。
    第九行: 当用户点击按钮后，我们传递一个新的值给 setCount。React 会重新渲染 Example 组件，并把最新的 count 传给它。

注意事项：

如果 state 是一个对象，不能部分 setState，这一点和 class 组件的 this.setState 不一样。在 calss 组件中，是会自动合并更新 state 的。

如果更新函数返回值与当前 state 完全相同，则随后的重渲染会被完全跳过。即在改变 state 的时候，如果是引用，那么地址要改变，否则 就不会重渲染。

函数式更新：如果新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 setState。该函数将接收先前的 state，并返回一个更新后的值。建议最好使用这种形式进行更新，即如果想对 n 进行加 1 操作，最好写成这种形式：setn(n=>n+1)，不过 setn(n+1)也是可以的。

具体可以参考[使用 state Hook](https://zh-hans.reactjs.org/docs/hooks-state.html)和[Hook API 索引](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate)

### 3.2.2：Hooks 之 useReducer{#a322}

useReducer 可以看作是 useState 的复杂版，使用方法：

&nbsp;&nbsp;1》创建初始值 initialData

&nbsp;&nbsp;2》创建所有操作 reducer(state,action)

&nbsp;&nbsp;3》传给 useReducer，得到读和写 api

&nbsp;&nbsp;4》使用读写 api 进行具体操作({type:'操作类型'})

具体例子，页面上有三个按钮，点击不同的按钮，n 的值会进行改变：

![](/images/tsak57React/useReducer1.png)

### 3.2.3：Hooks 之 useContext{#a323}

useContext 上下文， 使用方法：

&nbsp;&nbsp;1》使用 C=React.createContext(null)创建上下文

&nbsp;&nbsp;2》使用<C.provider>圈定作用域
&nbsp;&nbsp;3》在作用域内使用 useContext(c)去使用上下文

&nbsp;&nbsp;4》使用读写 api 进行具体操作({type:'操作类型'})

具体例子：

![](/images/tsak57React/useContext1.png)

&nbsp;&nbsp;但需要注意的是 useContext 不是响应式的，在一个模块中将值改变，另外一个模块是不会感知到的。

### 3.2.4：Hooks 之 useEffect&&useLayoutEffect{#a324}

useEffect 副作用，可以理解为 afterRender，每次 render 之后会调用的函数。可以代替之前的三种钩子。作为 componentDidMount 使用，[]作第二个参数；作为 componentDidUpdate 使用，可指定依赖；作为 componentWillUnmount 使用，通过 return。这三种用途可同时存在。如果同时存在多个 useEffect，会按照出现次序执行。

具体例子：
![](/images/tsak57React/useEffect1.png)
下面三张图是使用 useEffect 作为 componentWillUnmount 使用。
![](/images/tsak57React/useEffect2.png)
![](/images/tsak57React/useEffect3.png)
![](/images/tsak57React/useEffect4.png)

此处还有一个 useLayoutEffect 布局副作用，它是在浏览器渲染前执行。但因为大部分时候，我们很少去改变 DOM。为了用户体验，优先使用 useEffect。

### 3.2.5：Hooks 之 useMemo{#a325}

首先理解 React.memo，看下面的例子，点击 n 的时候，Child 组件虽然没有变化，但也会重新执行。
![](/images/tsak57React/usMemo1.png)
这是个多余的操作，如何消除，使用 React.memo。
![](/images/tsak57React/useMemo2.png)
但是有 bug，即添加了监听函数之后，就又不行了。原因在于》点击 n 之后，app 会重新执行，导致 onClickChild 也重新执行，虽然都是空函数，但每次生成的地址不一样。这样在 Child2 组件中，onClick 这个 props 会被认为变化了，因此 Child 组件还是会重新渲染。
![](/images/tsak57React/useMemo3.png)
如何解决这个问题？使用 useMemo：
![](/images/tsak57React/useMemo4.png)
useMemo 可以使用 useCallback 作为语法糖。即 useMemo(()=>x=>console.log(x),[m])和 useCallBack(x=>console.log(x),[m])效果是一样的。

### 3.2.6：Hooks 之 useRef{#a326}

如果需要一个值，在组件不断 render 的时候保持不变，这时就要用到 useRef。初始化 const count = React.useRef(0);读取 count.current。另外 useRef 变化的时候不会自动 render。
![](/images/tsak57React/useRef1.png)
接下来说一下 forwardRef，我们可以像下图那样使用一个 ref 去直接得到 DOM 对象：
![](/images/tsak57React/forwardRef1.png)
但在函数组件中，props 无法传递 ref 的值，所以会有如下报错：
![](/images/tsak57React/forwardRef2.png)
使用 React.forwardRef 去解决：
![](/images/tsak57React/forwardRef3.png)
此外和 ref 相关的还有一个 useImperativeHandle,但不常用，就不多赘述了。

### 3.2.7：自定义 Hook{#a327}

可以在自定义 Hook 里使用 Context，而且 useState 只说了不能在 if 里，但是能在函数组件里运行。下面是一个自定义 Hook 的例子。自定义了一个 useList 的 Hook，返回一个 list 和 setList 的读和写操作。
![](/images/tsak57React/hook1.png)
接下来就可以使用自定义的 useList，
![](/images/tsak57React/hook2.png)

### 3.2.8：stale closure{#a328}
