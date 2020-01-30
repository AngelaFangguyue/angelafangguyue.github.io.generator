---
title: "Ajax封装以及Promise"
date: 2020-01-29T22:02:05+08:00
draft: false
categories:
 - ajax promise
tags:
 - ajax promise
featured_image:
---
### ajax
ajax：Async Javascript And Xml
一般来说请求和响应是浏览器发给服务器的。AJAX是浏览器上的功能。浏览器在window上加了一个XMLHttpRequest函数，用这个构造函数（类）可以构造出一个对象，js通过它发起请求以及接收响应。
使用ajax发送请求，分为四步：
第一步创建XMLHttpRequest对象，
第二步调用对象的open方法,open方法里要传入请求的路径以及方法
第三步监听对象的onreadystatechange事件,在事件处理函数中进行操作
第四步调用对象的send方法（发送请求）
var request = new XMLHttpRequest();
request.open(method,url);
request.onreadyStateChange = ()=>{};
request.send();
### 1》闭包
什么是闭包，闭包的用途是什么，闭包的缺点是什么？   
闭包：能够读取其他函数内部变量的函数，在js中，只有定义在函数内部的子函数才能够读取局部变量，因此把闭包理解成一个定义在函数内部的函数。可以把它看作是连接函数内部和函数外部的桥梁。【使用了外部的变量】   
用途：就是读取函数内部变量，使得变量的值一直保存在内存中。 【隐藏局部变量，暴露操作函数
举例：

    const createAdd = ()=>{
    let n = 0
    return ()=>{
        n += 1
        console.log(n)
        }
    }

    const add = createAdd()
    add() // 1
    add() // 2
】  
缺点：由于变量一直保存在内存中，会导致内存消耗大，使得网页性能低下。所以在退出函数之前，将不使用的局部变量全部删除。而且由于它能够在父函数外部改变内部变量的值，使得数据不安全。【容易内存泄漏】
代码举例：

    function addAge(){
    let age = 21;
    return function(){
        age++;
        console.log(age);
    }
    }
    const closure = addAge;
    closure();
    closure()();
![](/images/task48/clo1.png)
**请理解下面的两个例子，另外注意this！！！**

    var name = "windowname";
    var obj1 = {
    name: "obj1name",
    getName: function(){
        return function(){
            return this.name;
        }
    }
    }
    alert(obj1.getName()());//windowname
///////////////////////////////////////////////////////////////

    var name = "windowname";
    var obj2 = {
    name: "obj2name",
    getName: function(){
        var that = this;
        return function(){
            return that.name;
        }
    }
    }
    alert(obj2.getName()());//obj2name
上面的两个例子，第一个例子，alert(obj1.getName()())是在全局执行上下文的环境中执行的，因此里面的this指向的是window对象。而第二个例子中，将对象作为this传递给了里面的匿名回调函数，因此在后面执行，访问的时候，访问就是对象的name属性。
### 2》call apply bind的用法
bind、call、apply的作用都是用来改变this指向的。看一下下面的这个例子：（可以联系上面的两个例子）

    let name = 'lucy'
    let obj = {
    name:'martin'
    say:function(){
        console.log(this.name)
    }
    };
    obj.say();//martin,this指向obj对象
    setTimeout(obj.say,0) //lucy this指向window对象

我们可以得到正常情况下say方法中的this指向的调用他的obj对象，而定时器setTimeout中的say方法中的this指向的window对象，这是由于say方法在setTimeout中是作为回调函数来执行的，因此回到主栈执行时是在全局执行上下文的环境中执行。这时候我们就需要去指定this指向的对象。

1:call() 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。function.call(thisArg, arg1, arg2, ...)
1.1>thisArg可选的。在 function 函数运行时使用的 this 值

1.2>arg1, arg2, ...   指定的参数列表

1.3>返回值是使用调用者提供的 this 值和参数调用该函数的返回值。若该方法没有返回值，则返回 undefined。

1.4例子

~~~
var animal = "cat";
var obj1 = {animal:'cat1',food:'fish1'};
var obj2 = {animal:'cat2',food:'fish2'};
function greet(){ console.log(this.animal);};
greet.call();//输出 cat,等同于greet()
greet.call(obj1);//输出 cat1
greet.call(obj2);//输出 cat2
~~~
2:apply() 方法调用一个具有给定this值的函数，以及作为一个数组（或类似数组对象）提供的参数。func.apply(thisArg, [argsArray])：

2.1>thisarg是必选的。指的是在func函数运行时使用的this值。

2.2>argsArray是可选的。指的是一个数组或者类数组对象，其中的数组元素将作为单独的参数传给func函数。如果该参数的值为null或者undefined，表示不需要传入任何参数。

2.3>返回值就是调用有指定this值和参数的函数的结果。

2.4例子

```
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
```

3:bind()会创建一个新的函数。在bind()被调用时，这个新函数的this被指定为bind()的第一个参数，而其余参数将作为新函数的参数，供调用时使用。funcfunction.bind(thisArg[,arg1[,arg2[,...]]]); bind()最简单的一个用法就是创建一个函数，无论怎样调用，这个函数都有同样的this值。

~~~
this.x = 9;    // 在浏览器中，this 指向全局的 "window" 对象
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();   
// 返回 9 - 因为函数是在全局作用域中调用的

// 创建一个新函数，把 'this' 绑定到 module 对象
// 新手可能会将全局变量 x 与 module 的属性 x 混淆
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
~~~   

【call 的用法:第一个参数是 this，后面的参数是 arguments 或其他参数；apply 的用法:第二个参数必须是数组，内含所有其他参数；bind 的用法要有两点: 1》fn.bind(x,y,z) 不会执行 fn，而是会返回一个新的函数；2》新的函数执行时，会调用 fn，调用形式为 fn.call(x, y, z)，其中 x 是 this，y 和z 是其他参数】

### 3》如何实现数组去重
1>可以借鉴计数排序的思路,该方法的缺点是只支持数字或者字符串数组，如果数组里面有对象，比如array=[{name:'www'},2],就会出错。
也可以直接将符合条件的结果push进结果中。
~~~
function unique(arr){
    let res = [];
    let hashdata = {};
    for(let i=0;i<arr.length;i++){
            hashdata[arr[i]] = true;   
    }
    for(let k in hashdata){
           res.push(k);
       }
    return res;
}

function unique(arr){
    let res = [];
    for(let i=0;i<arr.length;i++){
        if(res.indexOf(arr[i])===-1){
            res.push(arr[i]);
        }
        else{
            continue;
        }
    }
    return res;
}
~~~
2>使用set,缺点是API太新，旧浏览器不支持
~~~
function unique(arr){
    return [...new Set(arr)]
}
~~~
3>使用Map,缺点是API太新，旧浏览器不支持
~~~
function unique(arr){
    let map = new Map();
    let result = [];
    for(let i=0;i<arr.length;i++){
        if(map.has(arr[i])){
            continue;
        }else{
            map.set(arr[i],true);
            res.push(arr[i]);
        }
    }
    return res;
}
~~~

### 4》DOM事件相关
事件委托：也被称作事件代理，就是利用事件冒泡的原理，把事件加到父元素或者祖先元素上，触发执行效果。即只用指定一个事件处理程序，就可以管理某一类型的所有事件。【不监听元素A自身，而是监听它的祖先元素B，然后判断e.target是不是该元素A（或该元素的子元素）】

事件委托的原理：事件委托是利用事件的冒泡原理实现的。什么是事件冒泡？就是事件从最深的节点开始，然后逐步向上传播事件。举个例子：页面上有这么一个节点树，div>ul>li>a;比如给最里面的a加一个click点击事件，那么这个事件就会一层一层的往外执行，执行顺序a>li>ul>div，有这样一个机制，那么我们给最外面的div加点击事件，那么里面的ul，li，a做点击事件的时候，都会冒泡到最外层的div上，所以都会触发，这就是事件委托，委托它们父级代为执行事件。

事件委托的优点：1》提高js性能。事件委托比起事件分发（即一个DOM一个事件处 理程序），可以显著的提高事件的处理速度，减少内存的占用
2》动态的添加DOM元素，不需要因为元素的改 动而修改事件绑定

事件委托也有一定的局限性：比如focus blur 之类的事件本身没有事件冒泡机制，故无法委托；mousemove mouseout这样的事件，虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，因此也是不适合于事件委托的。

适合用事件委托的事件有：click mousedown mouseup keydown keyup keypress
所有鼠标事件中，只有mouseenter mouseleave不冒泡，其他鼠标事件都是冒泡的。

阻止默认动作：【e.preventDefault()或者return false】

~~~
function myfn(e){
window.event? window.event.returnValue=false :e.preventDefault();
}
~~~

阻止事件冒泡：【e.stopPropagation()】
~~~
function myfn(e){
window.event? window.event.cancelBubble = true : e.stopPropagation();
}
~~~

### 5》js的继承
基于原型的继承：【
~~~
function Parent(name1){
    this.name1 = name1;
}
Parent.prototype.pmethod = ()=>{
    console.log(this.name1);
}
function Child(name2,name1){
    Parent.call(this,name1);
    this.name2 = name2;
}
Child.prototype.__proto__ = Parent.prototype;
Child.prototype.cmethod = ()=>{
    console.log(this.name2);
}
~~~
】
基于class的继承：【
~~~
class parent{
    constructor(name1){
        this.name1 = name1
    }
    pmethod(){
        console.log(this.name1);
    }
}
class child extends parent{
    
    constructor(name2,name1){
        super(name1);
        this.name2 = name2;
    }
    cmethod(){
        console.log(this.name2);
    }
}
~~~
】

参考软老师的博客》Javascript继承机制的设计思想
~~~
function DOG(name){
    this.name = name;
}
DOG.prototype = {species:"犬科"}
var dog1 = new DOG("miaomiao");
var dog2 = new DOG("benben");
alert(dog1.species);//犬科
alert(dog2.species);//犬科
DOG.prototype.species = '猫科';
alert(dogA.species); // 猫科
alert(dogB.species); // 猫科
~~~
由于所有的实例对象共享同一个prototype对象，那么外界看来，prototype对象就好像是实例对象的原型，而实例对象则好像“继承”了prototype对象一样。
~~~
class DOG{
       species="犬科";
       constructor(name){this.name = name;}
 
}
var dog3 = new DOG("doudou");
alert(dog3.species);//犬科
alert(dog3.name);//doudou
~~~

### 6》数组排序

### 7》对Promise的理解
0.Promise是目前前端用于解决异步问题时的统一解决方案。Promise可以理解为是一个容器，里面保存的是某个事件的结果（通常是一个异步操作的结果，而且是未来才会结束的）。Promise是一个对象，一个异步操作的最终完成 (或失败), 及其结果值。从它可以获取异步操作的消息，它提供统一的API，各种异步操作都可以用同样的方法进行处理。Promise方案解决了不规范，容易出现回调地狱，很难进行错误处理等这些在解决异步编程问题时出现的问题。

1.创建Promise对象：
~~~
return new Promise((resolve,reject)=>{})
或者
const promise1 = new Promise(function(resolve,reject){
    //...some code
    if(/*异步操作成功*/){
        resolve(value);
    }else{
    reject(error);
    }
});
~~~

2.使用Promise.prototype.then() 它的作用是为 Promise 实例添加状态改变时的回调函数。then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。使用then方法，返回的还是一个Promise对象，但这是一个新的Promise对象。所以，可以采用链式写法。一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。Promise.prototype.catch方法用于指定发生错误时的回调函数。就像下面写的那样，这样的好处是可以捕获前面then方法执行中的错误，也更接近同步的写法（try/catch）。另外要注意的是跟传统的try/catch代码块不同的是，如果没有使用catch方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。
~~~
Promise.prototype.then(function1(){}).catch(function2(){});//
~~~

3.Promise.all()用于将多个Promise实例包装成一个新的Promise实例。const p = Promise.all([p1, p2, p3]);这个代码中，Promise.all()方法接受一个数组作为参数，p1 p2 p3都是Promise实例。如果不是，会将其转为实例再处理。另外，Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
p的状态由p1、p2、p3决定，分成两种情况。（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

 4.Promise.race()也是将多个Promise包装成一个新的Promise实例。但与all方法不同的是，只要传递的参数实例中有一个实例率先改变状态，那么新的Promise实例的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给新的Promise实例的回调函数。Promise.race()方法的参数与Promise.all()方法一样，如果不是 Promise 实例，就会先调用下面讲到的Promise.resolve()方法，将参数转为 Promise 实例，再进一步处理。

【Promise 的用途：Promise 用于避免回调地域，让代码看起来更同步   
如何创建一个 new Promise：
~~~
function fn(){
    return new Promise((resolve,reject)=>{
        成功时调用 resolve(data);
        失败时调用 reject(reason);
    });
}
~~~   
如何使用 Promise.prototype.then:fn().then(success,fail).then(success2,fail2).catch(fail3)   
如何使用 Promise.all:Promise.all([promise1,promise2])并行，等待所有promise成功。如果都成功了，则all对应的promise也成功，如果有一个失败了，则all对应的promise也失败。   
如何使用 Promise.race:Promise.all([promise1,promise2])，返回一个promise，一旦数组中的某个promise解决或拒绝，返回的promise就会解决或拒绝。
】

### 8》跨域
同源：如果两个url的协议，域名，端口号完全一致，那么这两个url就是同源的。同源策略是浏览器为了用户隐私，数据安全，故意设计的一个功能限制，不同源的页面之间，不准互相访问数据。
跨域：跨域就是突破浏览器的同源策略限制，不同源的页面之间，可以访问数据。
JSONP跨域：浏览器不支持CORS跨域的另一种跨域方式。它利用的是浏览器不限制对js的引用，我们可以随意引用js。这样的话，我们去请求一个js文件，这个js文件会执行一个回调，里面有我们要的数据。由于是script标签，不能像ajax那样，监听到返回码，也不知道响应头是什么，只知道成功或者失败。由于是script标签，只能发get请求，
CORS跨域：CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。它的使用就是去设置响应头，将允许访问的网址添加进去。

### 9》1使用promise的简单封装ajax（Axios jQuery.ajax） 2跨域 异步回调 
~~~
function ajax(method,url){
    return new Promise((resolve,reject)=>{
        var request = new XMLHttpRequest();
        request.open(method,url);
        request.onreadyStateChange = ()=>{
            if(request.readyState===4){
                //成功就调用resolve 失败就调用reject
                if(request.status<400){
                    resolve.call(null,request.response);
                }else if(request.status>=400){
                    reject.call(null,request);
                }
            }
        };
        request.send();
    });
}
ajax("get","/xxx").then((response)=>{},(request)=>{});
~~~
小结：
第一步return new Promise((resolve,reject)=>{...});任务成功则调用resolve(result)，任务失败则调用reject(error)，resolve和reject则会再去调用成功和失败的函数。
第二步使用.then(success,fail)传入成功和失败函数。

目前封装的比较简单，不能post上传数据，不呢个设置请求头。可以用jQuery.ajax或者axios去解决。
~~~
function jsonp(url){
 return new Promise((resolve,reject)=>{

     let rannum = "ff"+Math.random();
     window[rannum] = (res)=>{
         resolve(res);
     }
     let scr = document.createElement("script");
     scr.src = `${url}?callback=${funname}`;
     scr.onload = ()=>{
         scr.remove();
     };
     scr.onerror = (ee)=>{
         reject(ee);
     };
     document.body.appenChild(scr);

 });
}
jsonpp(url).then(succ => {
    console.log("这是用jsonp获取到的", succ);
  },
  fail => {
    console.log(fail);
  });
~~~
上面的封装要注意，在服务端，处理这个请求的时候，接收callback传递的函数名，对应的js文件中执行这个函数回调，返回我们所要的数据。