---
title: "MVC设计模式"
date: 2020-02-11T20:06:37+08:00
draft: false
categories:
  - mvc
tags:
  - mvc
featured_image:
---

### 本篇文章介绍 MVC 设计模式。主要内容有：

[1:MVC](#a1)

[2:关于 eventBus](#a2)

[3:表驱动编程](#a3)

[4:其他](#a4)

---

#### 1:MVC{#a1}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;关于经典的设计模式 MVC，M(model)数据模型，负责操作所有数据，V(view)视图，负责操作所有 ui 界面，C(controler)控制器，前两者之外的就放在这个模块。模块化之后，需要操作哪个部分，就可以直接去该部分修改。在实际操作中，会发现，view 模块和 controller 模块联系紧密，因此将其合为一个 view 模块，而现今流行的 vue 框架，则是将三个模块全部整合在了一起。因为比较简单的需求，这三个模块合在一起是完全 ok 的。掌握了 MVC 这个思想会让你对整个操作，心里有一个清晰的把握，知道是哪个部分，妥善处理数据视图之间的关系。也会更加清楚的理解这些流行框架的设计思想。对于 MVC，一般我们的设计是这样的：

**1》M 模块，有数据 data 以及对数据的操作。**

```
const model = {
    data:xxxx,//数据
    //对数据的操作，增删改查
    create(){/_对数据的增操作_/},
    delete(){/_对数据的删除操作_/},
    update(){/_对数据的修改操作_/},
    get(){/_对数据的获取操作_/},
};
```

将上述代码抽象成类，就是这样：

```
class Model{
    constructor(data){
        this.data = data;
        this.updateData = function(){
        /_对数据的修改操作_///每个实例对象的操作是不一样的
        }
    },
//对数据的操作，增删改查，这些是实例对象共有的方法
    create(){/_对数据的增操作_/},
    delete(){/_对数据的删除操作_/},
    update(){/_对数据的修改操作_/},
    get(){/_对数据的获取操作_/},
}
```

另外这里我们需要注意，当抽象成 Model 类，对于生成的每个实例对象的操作是不一样的，因此我们在定义 Model 类的时候，将其操作的方法也放在了 constructor 里面,如上面代码中的 updateData 方法。这样在生成实例对象的时候，用户就可以把对该实例对象的操作传过去。因为每个实例对象对数据的操作是不一样的。另外，也在 Model 类中保留了一些数据操作方法的原型 create，delete，update,get 方法。若所有生成的实例对象中都有些对数据相同的操作，即可放在这里面。

**2》V 模块，与界面相关。**

一般 view 模块，里面就是和视图相关的东西。像页面结构 html，还有对于页面的渲染初始化操作都会在这里。

```
const view = {
    el:xxxx,//存放的容器，即将布局结构要放置的地方
    html:``,//布局结构
    init(){/*初始化操作*/}
    render(){/*页面的渲染操作*/},
}
```

**3》C 模块，控制器模块。**

controller 模块，和上面两个模块无关的就放在这个模块。但严格意义上来说，该模块和 view 视图模块联系紧密，很多地方甚至将其合并，放在了一个模块里，统称 view 模块。

```
const controller = {
    events:{/*事件属性等*/},
    fun1(){/*用户需要进行的一些操作，类似属性可能不止一个，有可能还有fun2，fun3等*/},
    init(){/*初始化操作*/},
    bindEvents(){/*对页面元素进行绑定事件的操作*/}
}
```

在上面 controller 的 init 初始化模块，一般我们会把 view 模块的初始化 init 放在里面。因为只有视图初始化后，页面出现，用户再进行操作，我们才能在页面中捕获事件，获取到目标元素。因此这也就是为什么 view 模块和 controller 模块联系如此紧密。

所以当将 v 和 c 模块合并之后:

```
const View ={
    el:xxxx,//存放的容器，即将布局结构要放置的地方
    html:``,//布局结构
    events:{/*事件属性等*/},
    fun1(){/*用户需要进行的一些操作*/},
    bindEvents(){/*对页面元素进行绑定事件的操作*/}
    init(){/*初始化操作*/}
    render(){/*页面的渲染操作*/},
}
```

将其抽象成类 View，大致如下：

```
class View = {
    constructor({el,html,render....}){
        this.el = el;
        this.html =html;
        this.render = render;
        ......
    }
    //原型，共有的一些属性及操作放在这里
    ....
}
```

自己在学习 mvc 的过程中，一步步的从具体到抽象，最后发现，果然使用了 mvc 的思想，对代码进行抽象模块化之后，如果在后面想对某处进行修改，对应的模块，应该修改哪部分，并且关系涉及到哪个地方，这些都是很容易找到并修改的。

---

#### 2:关于 EventBus{#a2}

EventBus 用来管理事件的发布与订阅，在 Android 中很有名气。JS 里面，没有提供原生的事件管理机制的支持。我们把所有的对象都看成点，一个点和一个点，一个点和多个点，多个点和多个点怎么通信，最后找出一个专用的点负责通信，这个点就是 event bus（事件总线）。  
EventBus 一般要有 trigger on 以及 off 这三个方法。trigger 方法用来发布一个事件，on 方法是添加一个事件监听，off 取消一个事件监听。我们可以利用 jQuery 去实现一下 EventBus。
例子：假使现在一个页面有三个 tab，点击每一个 tab 切换一个页面，用事件通知来处理 tab 切换。
![](/images/task53/eventbus-example-1.png)
如果我们不采用 EventBus 的话，那我们一般是这样做的，每个按钮都对应于一个模块。传统的做法，在按钮的点击事件里，处理各个模块的逻辑，同时或许还要处理其他模块的显示隐藏清空等逻辑。可是这样做，试想，如果我们再新增加一个按钮，这个按钮又对应一个模块，那么我们需要在每个按钮的点击事件里，新增加一些判断逻辑。

```
//定义EventBus
const eventBus = \$(window);
//定义事件
function clickTab_1() {
    console.log("tab1");
    this.showTab1 = true;
    this.showTab2 = false;
    this.showTab3 = false;
  };
function clickTab_2() {
    console.log("tab2");
    this.showTab1 = false;
    this.showTab2 = true;
    this.showTab3 = false;
  };
function clickTab_3() {
    console.log("tab2");
    this.showTab1 = false;
    this.showTab2 = false;
    this.showTab3 = true;
  };
//添加事件监听
onLoad() {
  eventbus.on('clickTab_1', clickTab_1);
  eventbus.on('clickTab_2', clickTab_2);
  eventbus.on('clickTab_3', clickTab_3);
};
点击时触发发送事件:
clickTab(index) {
  const tabIndex = index - 0;
  if (tabIndex === 1) {
    eventbus.trigger('clickTab_1');
  } else if (tabIndex === 2) {
    eventbus.trigger('clickTab_2');
  } else if (tabIndex === 3) {
    eventbus.trigger('clickTab_3');
  }
},

```

场景》当一个页面中，有多个模块，就比如这个博客页面，每个菜单按钮都对应于一个模块，传统的做法，在按钮的点击事件里，处理各个模块的逻辑，同时或许还要处理其他模块的显示隐藏清空等逻辑。

```
menu.about.click = function() {
  aboutArea.loadAboutData("about date");
  homeArea.hiddenData("home data");
  archivesArea.hiddenDate("archives data");
  ......
}
```

如果页面模块不多，这样做完全是 ok 的，但假使要增加一个模块，就需要在上面的方法里额外在增加一些判断逻辑代码，这样有一个缺点，就是代码耦合比较紧，而使用事件就可以解决这个问题，点击菜单的时候，发送一个唯一的事件，在监听这个事件的方法里，实现对事件的处理。这样的话，每个模块监听到这个事件，会各自进行相关的操作，模块之间不相互影响。

```
menu.about.dispatch("aboutBtnClicked");
aboutArea.addEventListener("aboutBtnClicked", function() {
	// update code here
});
homeArea.addEventListener("aboutBtnClicked", function() {
	// update code here
});
```

可以看到，点击菜单按钮，不做任何逻辑，仅仅是发送了一个事件通知，具体的业务逻辑交由页面各个模块自己处理。只需要知道是哪个事件即可。【具体可以参考[这篇博客](https://mjd507.github.io/2017/08/20/Reading-EventBus-JS/)】

---

#### 3:表驱动编程{#a3}

表驱动编程，简单的来说就是用查表的方法获取指定的数据内容。它是一种编程模式，从表里面查找信息而不是使用逻辑语句（if…else…switch）。因为当是很简单的情况时，用逻辑语句很简单，但如果逻辑很复杂，再使用逻辑语句就很麻烦了。

比如查找一年中每个月份的天数，如果用表驱动法，完全不需要写一堆 if…else…语句，直接把每个月份的天数存到一个数组里就行了，取值的时候直接下标访问，最多针对二月判断一下闰年。表里可以存数据，也可以存指令，或函数指针等都可以。

使用表驱动的好处：提高了程序的可读性；减少了重复代码；可扩展性，容易修改维护（用数据代替逻辑）；降低了复杂度。

隐含在背后的思想,很多设计思路背后的原理其实都是相通的，隐含在数据驱动编程背后的实现思想包括：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1、控制复杂度。通过把程序逻辑的复杂度转移到人类更容易处理的数据中来，从而达到控制复杂度的目标。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、隔离变化。像上面的例子，每个消息处理的逻辑是不变的，但是消息可能是变化的，那就把容易变化的消息和不容易变化的逻辑分离。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3、机制和策略的分离。和第二点很像，本书中很多地方提到了机制和策略。上例中，我的理解，机制就是消息的处理逻辑，策略就是不同的消息处理

我在项目中使用到的表驱动编程的例子是：自己做的一个小 demo，页面中有 4 个按钮，每个按钮都需要添加一个点击事件。当然 4 个按钮，分别给每一个添加就好了，但是如果是 10 个呢？这时候，就体现出表驱动编程的好处了！！！

```
如let events = {
    "click #button1":"function1",
    "click #button2":"function2",
    "click #button3":"function3"...};
```

然后遍历循环这个 events 对象，根据 key 去获取到按钮，根据 value 去获取到函数，最后给每个 button 绑定相对应的事件处理函数；

```
for(let k in events){
    let index = k.indexOf(" ");
    let part1 = k.slice(index);//获取对应的事件
    let part2 = k.slice(index+1);//获取对应的元素
    let value = events[k];//获取对应的函数
    $(part2).on(part1,value);//添加事件
}
```

---

#### 4:其他{#a4}

1》什么是模块，阮一峰大佬说模块就是实现特定功能的一组方法。大佬早就在 12 年的博客中说到：“Javascript 模块化编程，已经成为一个迫切的需求。理想情况下，开发者只需要实现核心的业务逻辑，其他都可以加载别人已经写好的模块。但是，Javascript 不是一种模块化编程语言，它不支持"类"（class），更遑论"模块"（module）了。（正在制定中的 ECMAScript 标准第六版，将正式支持"类"和"模块"，但还需要很长时间才能投入实用。）”当然 2020 年的我们，如今可以方便的使用 es6 去模块化。

2》为什么要模块化》没有模块化前的项目，常常在一个 JS 文件中会有很多功能的代码，这使得文件很大，分类性不强，自然而然不易维护；那么我们将一个大的 JS 文件根据一定的规范拆分成几个小的文件的话将会便于管理，可以提高复用性，随之，可以起到分治的效果；一个复杂的项目肯定有很多相似的功能模块，如果每次都需要重新编写模块肯定既费时又耗力。同样，某个功能别人已经造好了轮子，我们就调来用用就好，这时就要引用别人编写模块，引用的前提是要有统一的「打开姿势」，如果每个人有各自的写法，那么肯定会乱套，所以会引出模块化规范；现在常用的 JavaScript 模块化规范有四种： Commonjs ， AMD , CMD , ES6 模块化 。个人理解，ES6 模块化才是主流。

3》模块的定义》将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起；块的内部数据相对而言是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信。所以，我们发现学习或建立模块就是抓住两点：如何引入模块？如何暴露模块？模块化的定义》编码时是按照模块一个一个编码的, 整个项目就是一个模块化的项目

4》模块化的优势》方便维护代码，更好的分离，按需加载；提高代码复用性；降低代码耦合度（降偶）；【也可以避免命名冲突(减少命名空间污染)；】分治思想——模块化不仅仅只是复用，不管你将来是否要复用某段代码，你都有充分的理由将其分治为一个模块。（我们在开发中有时候经常会出现一个模块，实则只用到了一次，但还是抽离出来作为单个独立的模块，这就是分而治之的软件工程的思想，在前端模块化同样适用）。

5》ES6 模块化》ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

(1)ES6 模块化语法》export 命令用于规定模块的对外接口，import 命令用于输入其他模块提供的功能。

```
/** 定义模块 math.js **/
var basicNum = 0;
var add = function (a, b) {
    return a + b;
};
export { basicNum, add };
/** 引用模块 **/
import { basicNum, add } from './math';
function test(ele) {
    ele.textContent = add(99 + basicNum);
}

```

如上例所示，使用 import 命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到 export default 命令，为模块指定默认输出。

```
// export-default.js
export default function () {
  console.log('foo');
}

// import-default.js
import customName from './export-default';
customName(); // 'foo'

```

模块默认输出, 其他模块加载该模块时，import 命令可以为该匿名函数指定任意名字。

(2)ES6 模块与 CommonJS 模块的差异
它们有两个重大差异：

① CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

② CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

第二个差异是因为 CommonJS 加载的是一个对象（即 module.exports 属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。
下面重点解释第一个差异，举 CommonJS 模块的加载机制例子:

```
// lib.js
export let counter = 3;
export function incCounter() {
counter++;
}
// main.js
import { counter, incCounter } from './lib';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```

复制代码 ES6 模块的运行机制与 CommonJS 不一样。ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

参考文章 :

1》[Javascript 模块化编程（一）](https://www.ruanyifeng.com/blog/2012/10/javascript_module.html)

2》[从前端模块化编程切入想聊聊前端的未来](https://juejin.im/post/5c82323ce51d453a5f22b281#heading-15)

3》[前端模块化详解](https://juejin.im/post/5c17ad756fb9a049ff4e0a62#heading-4)

4》[模块化的学习和理解](https://segmentfault.com/a/1190000019216621#item-4)
