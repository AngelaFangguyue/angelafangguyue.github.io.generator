---
title: "谈谈对this，原型链，继承，闭包等的理解"
date: 2020-04-26T15:39:37+08:00
draft: false
categories:
  - js
tags:
  - js
featured_image:
---

本篇文章主要总结梳理对 js 中常见的一些疑难点知识的学习理解，主要内容：

[1:this 到底指的是什么](#a1)

[2:原型链到底是什么](#a2)

[3:原型和类实现继承](#a3)

[4:闭包到底是什么](#a4)

[5:DOM 事件委托](#a5)

[6:js 数组以及字符串常用的方法](#a6)

[7:js 数组去重，js 排序算法](#a7)

[8:其他](#a8)

---

### 1:this 到底指的是什么{#a1}

<strong style="font-size:16px">首先，为什么要有 this？</strong>

请看下面的例子，定义了一个 person 对象，有 name 和 age 属性，还定义了一个 sayHi 方法。试想，如果现在没有 this 的话，我想在 syaHi 中打印出 person 的 name 和 age，那么就需要把这个对象的 name 和 age 属性作为参数传递进去。如图一。
![](/images/this/1.png)

进行第一次改进，将对象 person 作为参数直接传进去，如图二。
![](/images/this/2.png)

进行第二次改进，能不能在调用的时候，不传这个 person，因为毕竟前面使用 person.sayHi()，就是要用这个 person 的 name 和 age。

有两个方法：

1》依然把第一个参数 self 当作 person，形参要始终比实参多一个，self

2》隐藏 self，用关键字 this 来访问 self

JS 之父选择了方法 2，Python 之父选择了方法 1，留下 self 作为第一个参数
![](/images/this/3.png)

实际上 this 是隐藏的第一个形参。在调用 person.sayHi() 时，这个 person 会「变成」 this。

即 person.sayHi()其实等价于 person.sayHi.call(person)。.call 的第一个参数就是显式的 person，没有任何语法糖。因此你可以使用 obj.fn.call(null,1,2,3)来手动禁用 this。这样一来，person.sayHi.call 的参数其实可以是任何对象，也就是说虽然 person.sayHi 虽然是 person 的方法，但其实是可以调用在任何对象上的。

<strong style="font-size:16px">接下来看一下函数调用：</strong>

一般函数调用有这三种情况
1》fn(p1,p2)
2》obj.fn(p1,p2)
3》fn.call(context,p1,p2)
只有第三种调用形式，才是正常的调用形式，其余两种都可以等价的变为 call 形式：
fn(p1,p2) fn.call(undefined,p1,p2)
obj.fn(p1,p2) obj.fn.call(undefined,p1,p2)
到此，我们的函数调用只有一种形式：
fn.call(context,p1,p2)
所以 this 就是 context！！！

<strong style="font-size:16px">this，就是上面代码中的 context。就这么简单。
即换句话说就是 this 永远指向最后调用它的那个对象</strong>

<strong style="font-size:16px">先看 func(p1, p2) 中的 this 如何确定：</strong>

```
function func(){
console.log(this)
}
func()
```

用「转换代码」把它转化一下，得到

```
function func(){
console.log(this)
}
func.call(undefined) // 可以简写为 func.call()
```

按理说打印出来的 this 应该就是 undefined 了吧，但是浏览器里有一条规则：
如果你传的 context 是 null 或 undefined，那么 window 对象就是默认的 context（严格模式下默认 context 是 undefined）
因此上面的打印结果是 window。
如果你希望这里的 this 不是 window，很简单：
func.call(obj) // 那么里面的 this 就是 obj 对象了

<strong style="font-size:16px">再看 obj.child.method(p1, p2) 的 this 如何确定</strong>

```
var obj = {
foo: function(){
console.log(this)
}
}
obj.foo()
```

按照「转换代码」，我们将 obj.foo() 转换为
obj.foo.call(obj)
好了，this 就是 obj。搞定。

<strong style="font-size:16px">[ ] 语法中 this 怎么确定</strong>

```
function fn (){ console.log(this) }
var arr = [fn, fn2]
arr[0]() // 这里面的 this 又是什么呢？
```

我们可以把 arr[0]() 想象为 arr.0( )，虽然后者的语法错了，但是形式与转换代码里的 obj.child.method(p1, p2) 对应上了，于是就可以愉快的转换了：
arr[0]()
假想为 arr.0()
然后转换为 arr.0.call(arr)
那么里面的 this 就是 arr 了 :)

<strong style="font-size:16px">箭头函数</strong>

实际上箭头函数里并没有 this，如果你在箭头函数里看到 this，你直接把它当作箭头函数外面的 this 即可。外面的 this 是什么，箭头函数里面的 this 就还是什么，因为箭头函数本身不支持 this。
有人说「箭头函数里面的 this 指向箭头函数外面的 this」，这很傻，因为箭头函数内外 this 就是同一个东西，并不存在什么指向不指向。

<strong style="font-size:16px">总结
this 就是你 call 一个函数时，传入的第一个参数。（「this 就是 call 的第一个参数」）
this 永远指向最后调用它的那个对象
如果你的函数调用形式不是 call 形式，请按照「转换代码」将其转换为 call 形式。</strong>

<strong style="font-size:16px">Event Handler 中的 this</strong>
btn.addEventListener('click' ,function handler(){
console.log(this) // 请问这里的 this 是什么
})
handler 中的 this 是什么呀，看了你的文章我还是不懂啊？
那是因为你没有看懂，我们说过 this 都是由 call 或 apply 指定的，那么你只需要找到 handler 被调用时的代码就行了。
可是我哪知道 addEventListener 的源代码呀。是呀，addEventListener 是浏览器内置的方法，我们看不见源代码。
所以，你只能看文档了，MDN 这样说：通常来说 this 的值是触发事件的元素的引用，这种特性在多个相似的元素使用同一个通用事件监听器时非常让人满意。当使用 addEventListener() 为一个元素注册事件的时候，句柄里的 this 值是该元素的引用。其与传递给句柄的 event 参数的 currentTarget 属性的值一样。由于浏览器知道你不方便看源码里是怎么 call handler 的，所以直接在文档里告诉你了，你可以假想浏览器的源码是这样写的：
// 当事件被触发时
handler.call(event.currentTarget, event)
// 那么 this 是自然就知道了

<strong style="font-size:16px">jQuery Event Handler 中的 this</strong>
那么下面代码中的 this 是什么呢：
\$ul.on('click', 'li' , function(){
console.log(this)
})

看 jQuery 源码是怎么 call 这个函数的，或者看 jQuery 文档。jQuery 文档是这样写的：当 jQuery 的调用处理程序时，this 关键字指向的是当前正在执行事件的元素。对于直接事件而言，this 代表绑定事件的元素。对于代理事件而言，this 则代表了与 selector 相匹配的元素。(注意，如果事件是从后代元素冒泡上来的话，那么 this 就有可能不等于 event.target。)若要使用 jQuery 的相关方法，可以根据当前元素创建一个 jQuery 对象，即使用 \$(this)。

总结一下如何确定 this 是值
看源码中对应的函数是怎么被 call 的（这是最靠谱的办法）;看文档;console.log(this);千万不要瞎猜，猜不到的

如何强制指定 this 的值？自己写 call / apply 即可

### 2:原型链到底是什么]{#a2}

每个对象都有一个隐藏属性，这个隐藏属性指向了该类对象的共有属性，就是这类对象的原型。比如说数组对象有数组对象的原型，我们定义一个数组对象 var arr1 = new Arrar();或者是 var arr2 = [1,2,3]。arr1 和 arr2 都会有一些 push，pop 等方法，这些方法就是它们的原型上自带的。对象的原型也是对象。

每个构造函数都有一个 protoype 属性，该属性指向的就是构造函数构造出来的这一类对象的原型。比如说 Array 这个构造函数，它的 prototype 属性指向的对象，就是 Array 这个构造函数构造出的所有函数对象的原型。还拿上面的例子来说，arr1 对象的隐藏属性对应的就是数组对象的原型，即 Array 这个构造函数的 prototype 指向的对象。

另外要注意：构造函数本身也是对象。因此它也有隐藏属性。按照上面的说法，它的隐藏属性指向的就是它这一类对象的原型。它这一类对象就是一类函数对象。函数对象的原型就是 Function 的 prototype 所指向的对象。

拿 Object 这个构造函数来说，它有 prototype 属性，也有隐藏属性。它的 prototype 属性指向的就是 Object 函数构造出来的这类对象所共有的属性，即所有普通对象原型。而它的隐藏属性，指向的是 Object 本身这类构造函数对象所对应的原型。下面我们可以知道，函数对象的原型就是 Function 构造函数的 prottotype 所指向的对象，即函数的原型。

Array 构造函数，prototype 属性指向的是数组这类对象所共同拥有的属性即数组的原型，隐藏属性则指向了它本身这个构造函数对象的原型。它本身是个函数，它的隐藏属性指向的就是这一类函数对象的原型，即 Function 的 prototype 属性指向的对象。

要记住：所有构造函数的原型都是 Function 的 prototype 属性指向的对象。Function 它自身也是个对象，是个构造函数对象，它的隐藏属性指向的也是。这就是函数对象的原型。也就是说 Function 构造函数对象的 prototype 属性和隐藏属性对应的是同一个对象，都是函数对象的原型。

构造函数的 prototype 属性指向的是构造出来的这一类对象的原型，也是一个对象，也有隐藏属性，那么它指向的就是它这一类对象的原型。它这一类是普通的对象，普通对象的原型自然就是 Object 构造函数的 prototype 属性指向的对象，即普通对象的原型。

Object 的 prototype 属性所指向的普通对象的原型也是一个对象。因此它也有隐藏属性，规定为 null。

要记住原型也是对象。任何一个原型都有一个 constructor 属性，指向它的构造函数。

每个普通对象也有一个 constructor 属性，默认调用的就是 prototype 对象的 constructor 属性。

其实在 js 中，一切皆对象。构造函数是对象；构造函数构造出来的东西也是对象；构造函数的 prototype 属性对应的原型也是对象；所有对象的隐藏属性对应的是该类对象所共同拥有的属性，就是原型，还是对象。规定 Object 这个构造函数的 prototype 属性指向的对象，即普通对象的原型，它的隐藏属性为 null。

### 3:原型和类实现继承{#a3}

原型实现继承：

```
function Parent(name1){
  this.name1 = name1
}
Parent.prototype.pMethod = ()=>{
  console.log("在Parent中:",this.name1);
}
function Son(name2,name1){
  Parent.call(this,name1);
  this.name2 = name2;
}
Son.prototype.__proto__ = Parent.prototype;
Son.prototype.sMethod = ()=>{
  console.log("在Son中:",this.name2);
};
```

基于类的继承：

```
class Parent{
  constructor(name1){
    this.name1 = name1;
  }
  pMethod(){
    console.log("在Parent中:",this.name1);
  }
}
class Son extends Parent{
  constructor(name2,name1){
    super(name1);
    this.name2 = name2;
  }
  cMethod(){
    console.log("在Son中:",this.name2);
  }
}
```

### 4:闭包到底是什么{#a4}

在 js 中，函数外部是无法访问在函数内部定义的变量的。而闭包是连接函数内部和外部的桥梁。通过它，我们可以访问函数内部的变量。为什么经常说闭包是一个定义在函数内部的函数呢？因为这样的话，把定义在内部的函数 return 出来，我们通过 return 出来的这个函数可以访问到函数内部的变量。闭包起到了什么作用？第一个就是让我们可以在外面访问到函数内部的变量，起到了隐藏变量的作用，又可以通过这个闭包去实现去内部变量的修改等操作。另外就是可以使得变量一直保存在内存中。但是闭包也有缺点，造成内存泄漏，解决方法是在退出操作之前，将不使用的局部变量全部删除掉。闭包会在父函数的外部改变父函数内部变量的值，如果把父函数当作对象来用，把内部变量当作它的私有属性，把闭包当作它的公有方法，不要随便改变父函数内部变量的值。

### 5:DOM 事件委托{#a5}

网页其实是一棵树。浏览器向 window 上加一个 document 就可以使得 js 操作这棵树。js 使用 document 操作网页，这就是文档对象模型（Document Object Model）。

要理解 DOM 相关事件，先理解“事件流”这个概念，事件流描述的是从页面中接收事件的顺序。

DOM2 级事件规定的事件流包括三个阶段：事件捕获、目标阶段、事件冒泡。

事件冒泡：事件开始由最具体的元素接收，然后逐级向上传播到较为不具体的节点或文档。

事件捕获：事件开始由不太具体的节点接收，然后逐级向下传播到最具体的节点。它与事件冒泡是个相反的过程。

事件委托，通俗的说就是将元素的事件委托给它的父级或者更外级的元素处理，它的实现机制就是事件冒泡。

假设有一个列表，要求点击列表项弹出对应的字段：

```
<ul id="myLink">
  <li id="1">aaa</li>
  <li id="2">bbb</li>
  <li id="3">ccc</li>
</ul>
//不使用事件委托
var myLink = document.getElementById('myLink');
var li = myLink.getElementsByTagName('li');

for(var i = 0; i < li.length; i++) {
li[i].onclick = function(e) {
var e = event || window.event;
var target = e.target || e.srcElement;
alert(e.target.id + ':' + e.target.innerText);
 };
}
```

存在问题：给每一个列表都绑定事件，消耗内存；当有动态添加的元素时，需要重新给元素绑定事件

当使用事件委托时，

```
 ul.addEventListener('click', function(e){
     if(e.target.tagName.toLowerCase() === 'li'){
         fn() // 执行某个函数
     }
 })
```

但实际上，上面的写法还是有一定错误的，倘若 li 里面包裹的有 span 元素，这时候如果点击到 span 元素，就没有办法触发 fn。所以正确的写法应该是这样的：

```
function update(element,eventType,selector,fn){
  element.addEventListener(eventType,(e)=>{
    let el = e.target;

    while(!el===selector){
      if(el===element){
        el=null;
        break;
      }
      el = el.parentNode;
    }
    el && fn.call(el)
  })
  return element;
}
```

思路就是，当点击 span 元素的时候，一层一层的向上找 span 元素的父级元素，看是否有 ul 里的 li。

事件委托的特点 1.可以大量节省内存占用，减少事件注册；2.实现当新增子对象时无需再次对其绑定事件,实现动态内容很方便

### 6:js 数组以及字符串常用的方法{#a6}

### 7:js 数组去重，js 排序算法{#a7}

### 8:其他{#a8}

如何实现一个三栏布局，左右宽度固定，中间宽度自适应。

1：绝对定位，中间主体用左右 margin 撑开

2：自身浮动法，中间的 div 要放在最后，注意清除浮动

3：margin 负值法：左右两栏均左浮动，左右两栏采用负的 margin 值。中间栏被宽度为 100%的浮动元素包起来。左右两栏 div 的顺序不分先后，但是主体部分 div 要放前面。

4：flex 布局，左右 flex-basis，中间 flex-grow。
