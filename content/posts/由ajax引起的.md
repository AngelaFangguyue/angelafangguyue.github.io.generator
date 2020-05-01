---
title: "由Ajax引起的"
date: 2020-01-29T22:02:05+08:00
draft: false
categories:
  - ajax promise
tags:
  - ajax promise
featured_image:
---

# 目录：

[ajax](#a1)

[跨域](#a2)

[promise](#a3)

[axios](#a4)

[localStorage 等](#a5)

[其他](#a6)

### ajax：Async Javascript And Xml{#a1}

一般来说请求和响应是浏览器发给服务器的。AJAX 是浏览器上的功能。浏览器在 window 上加了一个 XMLHttpRequest 函数，用这个构造函数（类）可以构造出一个对象，js 通过它发起请求以及接收响应。

使用 ajax 发送请求，分为四步：

第一步创建 XMLHttpRequest 对象，
第二步调用对象的 open 方法,open 方法里要传入请求的路径以及方法
第三步监听对象的 onreadystatechange 事件,在事件处理函数中进行操作
第四步调用对象的 send 方法（发送请求，这里可以上传数据）

```
var request = new XMLHttpRequest();
request.open(method,url);
request.onreadyStateChange = ()=>{};
request.send();
```

### 跨域（CORS,JSONP）{#a2}

浏览器有同源策略的限制。同源指的是协议，地址，端口号相同。该限制会导致如果一个浏览器去向服务器发起请求，若发起请求的地址和要请求的地址不同源，该请求会被拒绝。那么 ajax 请求就也受到这种限制，那么如何处理？一般采取两种手段，CORS 和 JSONP。

CORS 就是在服务器端进行设置，设置响应头 Access-Control-Allow-Origin 将允许跨域访问的网址添加进去。具体文档都在 MDN 上。

但是 IE 浏览器不支持 CORS，这时就使用 JSONP。JSONP 和 JSON 没有任何关系，JSON 就是一门标记性语言。JSONP 是利用了浏览器对于 js 文件的请求没有限制，使用 script 标签等带有 src 属性的标签去发起请求，请求一个 js 文件。该 js 文件会执行一个回调，里面有我们想要的数据。一般回调函数的名字是随机生成的，以 callback 参数的形式传递过去。JSONP 好处是兼容 IE，可以跨域，但是它只支持 get 请求，不支持 post 请求。而且 jsonp 不像 ajax 那样，可以获得请求返回的状态码，jsonp 只知道成功或者失败，也不知道响应头。

### promise 以及用 promise 去封装 ajax 和 jsonp{#a3}

ajax 的操作是异步的，不能直接拿到结果。前端对于异步操作统一使用 Promise 的解决方案。它的好处是：规范了名称，解决了回调地狱的问题，方便的进行错误处理。

使用时构造一个 promise 对象即可：new Promise((resolve,reject)=>{
异步操作，
成功时 resolve;
失败时 reject;
});

promise 对象有两个特点，一是对象的状态不受外界影响，二是状态一旦改变，就不会再变，任何时候都可以得到这个结果。

但它也有缺点。第一，promise 一旦设置，就不能取消。其次，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。第三，当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。但是 axios 就可以取消，通过 cancelToken 方法，取消的不是 promise，而是对应的 ajax 请求。promise 还是会进行操作，但是对应的 ajax 请求会被中止，不要了。

下面是用 promise 封装的一个简单 ajax 请求：

```
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
```

小结：
第一步 return new Promise((resolve,reject)=>{...});任务成功则调用 resolve(result)，任务失败则调用 reject(error)，resolve 和 reject 则会再去调用成功和失败的函数。
第二步使用.then(success,fail)传入成功和失败函数。目前封装的比较简单，不能 post 上传数据，不能设置请求头。可以用 jQuery.ajax 或者 axios 去解决。

下面是用 promise 封装的一个简单的 jsonp 请求：

```
function jsonp(url){
 return new Promise((resolve,reject)=>{

     let rannum = "ff"+Math.random();
     window[rannum] = (res)=>{
         resolve(res);
     }
     let scr = document.createElement("script");
     scr.src = `${url}?callback=${rannum}`;
     scr.onload = ()=>{
         scr.remove();
     };
     scr.onerror = (ee)=>{
         reject(ee);
     };
     document.body.appenChild(scr);

 });
}
jsonp(url).then(succ => {
    console.log("这是用jsonp获取到的", succ);
  },
  fail => {
    console.log(fail);
  });
```

上面的封装要注意，在服务端，处理这个请求的时候，接收 callback 传递的函数名，对应的 js 文件中执行这个函数回调，返回我们所要的数据。

### jQuery.ajax()和 axios{#a4}

由于上面使用 promise 封装的 ajax 和 jsonp 都很简陋，其实已经有封装好的 jQuery.ajax()和 axios 来供我们使用。

jQuery.ajax()：

```
$.ajax({
    type: 'POST',
    url: url,
    data: data,
    dataType: dataType,
    success: function() {},
    error: function() {}
})
```

它是对原生 XHR 的封装，还支持 JSONP。jQuery 发送的所有 Ajax 请求，内部都会通过调用 $.ajax() 函数来实现。通常没有必要直接调用这个函数，可以使用几个已经封装的简便方法，如$.get()和$.load()。如果你需要用到那些不常见的选项，那么$.ajax()使用起来更灵活。

Example: 发送 id 作为数据发送到服务器，保存一些数据到服务器上，一旦完成并通知它的用户。如果请求失败，则提醒用户。

```
var menuId = $("ul.nav").first().attr("id");
var request = $.ajax({
  url: "script.php",
  type: "POST",
  data: {id : menuId},
  dataType: "html"
});

request.done(function(msg) {
  $("#log").html( msg );
});

request.fail(function(jqXHR, textStatus) {
  alert( "Request failed: " + textStatus );
});
```

Axios:一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。它有以下几大特性：是对原生 XHR 的封装；可以在 node.js 中使用；提供了并发请求的接口；支持 Promise API；可以拦截请求和响应；取消请求；

Axios 的一些高级用法，比如说它会 JSON 自动处理，如果发现响应的 Content-Type 是 json，就会自动调用 JSON.parse;请求拦截器，可以在所有请求里加一些操作；响应拦截器，可以在所有响应里加一些东西，甚至改内容；可以生成不同的实例，不同的实例可以设置不同的配置，用于复杂场景。

简单使用：

```
axios({
    method: 'GET',
    url: url,
})
.then(res => {console.log(res)})
.catch(err => {console.log(err)})
```

写法有很多种，可以去查看[官方文档](http://www.axios-js.com/zh-cn/docs/index.html)

并发请求

```
function getUserAccount() {
return axios.get('/user/12345');
}

function getUserPermissions() {
return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
.then(axios.spread(function (acct, perms) {
// Both requests are now complete
}));
```

拦截器：在请求或响应被 then 或 catch 处理前拦截它们。

```
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

如果你想在稍后移除拦截器，可以这样：

```
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

可以为自定义 axios 实例添加拦截器

```
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

错误处理

```
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // The request was made and the server responded with a status code
      // that falls out of the range of 2xx
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // The request was made but no response was received
      // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
      // http.ClientRequest in node.js
      console.log(error.request);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

可以使用 validateStatus 配置选项定义一个自定义 HTTP 状态码的错误范围。

```
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // Reject only if the status code is greater than or equal to 500
  }
})
```

使用 cancel token 取消请求

```
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
     // 处理错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');

还可以通过传递一个 executor 函数到 CancelToken 的构造函数来创建 cancel token：

const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// cancel the request
cancel();
```

注意: 可以使用同一个 cancel token 取消多个请求

现在有一个问题是，既然请求都发出了，为什么我们要有取消请求呢？怎么会有这个需求？我们经常开发的时候会遇到一个重复点击的问题，短时间内多次点击同一个按钮发送请求会加重服务器的负担，消耗浏览器的性能，所以绝大多数的时候我们需要做一个取消重复点击的操作。联系生活实际，当网速不好的情况下，如果我们在 app 中点击一个按钮，毫无反应，我们通常会等不及，连续多次点击。这个时候，就会重复向后台发送请求。这种情形会给后台服务器带来很多不必要的请求回应，浪费资源。所以在这种情况下，我们就需要对后面再次发出的请求进行取消。大致思路就是：

1：我们设置一个数组，数组中存放的是一个个的对象，对象里面是为每一个 ajax 请求做的一个 ajax 请求标记和对应取消请求的函数。

2：每次发起请求前，设置一个拦截器，判断该请求是否已经发出过，如果有的话，就将这次的请求取消掉。同时将该请求对应的数据信息记录在数组中。

3：每次在响应前也要设置拦截器，将已经执行完成的请求取消掉，同时在数组中把相对应的 ajax 请求的有关数据删除掉。

代码：

```
 import axios from 'axios';

    axios.defaults.timeout = 5000;
    axios.defaults.baseURL ='';

    let pending = []; //声明一个数组用于存储每个ajax请求的取消函数和ajax标识
    let cancelToken = axios.CancelToken;
    let removePending = (ever) => {
        for(let p in pending){
            if(pending[p].u === ever.url + '&' + ever.method) { //当当前请求在数组中存在时执行函数体
                pending[p].f(); //执行取消操作
                pending.splice(p, 1); //把这条记录从数组中移除
            }
        }
    }

    //http request 拦截器
    axios.interceptors.request.use(
    config => {
      config.data = JSON.stringify(config.data);
      config.headers = {
        'Content-Type':'application/x-www-form-urlencoded'
      }
      // ------------------------------------------------------------------------------------
      removePending(config); //在一个ajax发送前执行一下取消操作
      config.cancelToken = new cancelToken((c)=>{
         // 这里的ajax标识我是用请求地址&请求方式拼接的字符串，当然你可以选择其他的一些方式
         pending.push({ u: config.url + '&' + config.method, f: c });
      });
      // -----------------------------------------------------------------------------------------
      return config;
    },
    error => {
      return Promise.reject(err);
    }
  );
  //http response 拦截器
  axios.interceptors.response.use(
    response => {
      // ------------------------------------------------------------------------------------------
      removePending(res.config);  //在一个ajax响应后再执行一下取消操作，把已经完成的请求从pending中移除
      // -------------------------------------------------------------------------------------------
      if(response.data.errCode ==2){
        router.push({
          path:"/login",
          querry:{redirect:router.currentRoute.fullPath}//从哪个页面跳转
        })
      }
      return response;
    },
    error => {
      return Promise.reject(error)
    }
  )
```

[参考链接](https://www.jianshu.com/p/22b49e6ad819)
这种做法其实取消的是上一次的请求，这样请求已经发出去了，还是会增加服务器的负担。取消接口请求有可能是在请求发出去的过程中中断，也有可能是在 response 回来的路上中断。目前这一只能保证前端获取数据正常。

所以防止重复点击可以让按钮置灰或 loading。

另外可以在服务器执行的时候给 session 加个标记，结束时取消标记，前端根据这个去查标记是否存在，不存在才发请求。

另外的解决思路：

设置一个 cancelFlag 作为标志符，默认为 true，在请求拦截器时，判断如果 cancelFlag 为 true，就可以发送请求，且将 cancelFlag 设为 false。当 cancelFlag 为 false,就取消请求。在响应拦截器中再将 cancelFlag 设为 true。说明只有当一个请求发送且收到响应后，才可以发送另一个请求。<span style="color:red;">这里存在的问题：cancelFlag 是全局变量，这样多页面多个接口请求时，互相会有影响。</span>这里的解决办法就是在 axios.js 中构建构造函数，这样可以让 cancelFlag 私有化，但是这样的方式会导致占有大量内存。
版本 1 的代码：

```
import Vue from 'vue'
import axios from 'axios'
import {Indicator} from 'mint-ui'
Vue.component(Indicator)
let CancelToken = axios.CancelToken //取消请求
let cancelFlag = true
//设置公共部分，请求头和超时时间
axios.defaults.headers = {
    'X-Requested-With': 'XMLHttpRequest'
}
axios.defaults.timeout = 20000
//在请求拦截器时
axios.interceptors.request.use(config => {
    if (cancelFlag) {
        cancelFlag = false
        Indicator.open()
    } else {
        cancelToken: new CancelToken (c => {
            cancel = c
        })
        cancel()
    }
    return config
}, error => {
    return Promise.reject(error)
})
axios.interceptors.response.use(config => {
    cancelFlag = true
    Indicator.close()
    return config
}, error => {
    //
})
```

版本二：异步请求时，带上一个参数 requestName。
这里一开始的疑惑是，当请求 a 带上参数 requestName 后，发送多次请求，判断 axios[requestName]和 axios[requestName].cancel 存在时，会做取消处理。那发送成功后，再点击时，axios[requestName]和 axios[requestName].cancel 还是会存在啊。这样还是会执行 axios[requestName].cancel()。
这里是因为当上一次请求发送成功后，其 axios[requestName].cancel 这个方法已经失效，即使执行了这个方法也不起作用。axios[requestName].cancel 的值永远是上一次的请求的取消回调。当上一次请求成功后，该回调会失效。

```
axios.interceptors.request.use(config => {
    let requestName = config.data.requestName
    if (requestName) {
        if (axios[requestName] && axios[requestName].cancel) {
            axios[requestName].cancel()
        }
        config.cancelToken = new CancelToken (c => {
            axios[requestName] = {}
            axios[requestName].cancel = c
        })
    }
    return config
}, error => {
    return Promise.reject(error)
})
```

[参考链接：](https://juejin.im/post/5b714a44f265da27ea319fcb)

### localStorage 等{#a5}

localStorage：存储在本地，一般大小限制在 5 到 10m。使用 window.localStorage.getItem(key)去获取，使用 window.localStorage.setItem(key)去设置。

Cookie：服务器下发给浏览器的一段字符串，浏览器必须保存这个 Cookie（除非用户删除），之后发起相同二级域名请求（任何请求时），浏览器必须附上 Cookie。一般最大是 4k。

session：基于 cookie 实现的，是把 session id 存在 cookie 中。

cookie 和 localstorage 的区别，一是大小，二是 cookie 会被发送到服务器，而 localStorage 不会。

session 和 cookie 的区别，前者是存在于服务器文件中，cookie 是存在浏览器文件中。

localStorage 一般不会自动过期（除非用户手动清除），而 sessionstorage 在会话结束时过期（如关闭浏览器）。

### 1》闭包

什么是闭包，闭包的用途是什么，闭包的缺点是什么？  
闭包：能够读取其他函数内部变量的函数，在 js 中，只有定义在函数内部的子函数才能够读取局部变量，因此把闭包理解成一个定义在函数内部的函数。可以把它看作是连接函数内部和函数外部的桥梁。【使用了外部的变量】  
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
**请理解下面的两个例子，另外注意 this！！！**

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

上面的两个例子，第一个例子，alert(obj1.getName()())是在全局执行上下文的环境中执行的，因此里面的 this 指向的是 window 对象。而第二个例子中，将对象作为 this 传递给了里面的匿名回调函数，因此在后面执行，访问的时候，访问就是对象的 name 属性。

### 2》call apply bind 的用法

bind、call、apply 的作用都是用来改变 this 指向的。看一下下面的这个例子：（可以联系上面的两个例子）

    let name = 'lucy'
    let obj = {
    name:'martin'
    say:function(){
        console.log(this.name)
    }
    };
    obj.say();//martin,this指向obj对象
    setTimeout(obj.say,0) //lucy this指向window对象

我们可以得到正常情况下 say 方法中的 this 指向的调用他的 obj 对象，而定时器 setTimeout 中的 say 方法中的 this 指向的 window 对象，这是由于 say 方法在 setTimeout 中是作为回调函数来执行的，因此回到主栈执行时是在全局执行上下文的环境中执行。这时候我们就需要去指定 this 指向的对象。

1:call() 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。function.call(thisArg, arg1, arg2, ...)
1.1>thisArg 可选的。在 function 函数运行时使用的 this 值

1.2>arg1, arg2, ... 指定的参数列表

1.3>返回值是使用调用者提供的 this 值和参数调用该函数的返回值。若该方法没有返回值，则返回 undefined。

1.4 例子

```
var animal = "cat";
var obj1 = {animal:'cat1',food:'fish1'};
var obj2 = {animal:'cat2',food:'fish2'};
function greet(){ console.log(this.animal);};
greet.call();//输出 cat,等同于greet()
greet.call(obj1);//输出 cat1
greet.call(obj2);//输出 cat2
```

2:apply() 方法调用一个具有给定 this 值的函数，以及作为一个数组（或类似数组对象）提供的参数。func.apply(thisArg, [argsArray])：

2.1>thisarg 是必选的。指的是在 func 函数运行时使用的 this 值。

2.2>argsArray 是可选的。指的是一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为 null 或者 undefined，表示不需要传入任何参数。

2.3>返回值就是调用有指定 this 值和参数的函数的结果。

2.4 例子

```
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
```

3:bind()会创建一个新的函数。在 bind()被调用时，这个新函数的 this 被指定为 bind()的第一个参数，而其余参数将作为新函数的参数，供调用时使用。funcfunction.bind(thisArg[,arg1[,arg2[,...]]]); bind()最简单的一个用法就是创建一个函数，无论怎样调用，这个函数都有同样的 this 值。

```
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
```

【call 的用法:第一个参数是 this，后面的参数是 arguments 或其他参数；apply 的用法:第二个参数必须是数组，内含所有其他参数；bind 的用法要有两点: 1》fn.bind(x,y,z) 不会执行 fn，而是会返回一个新的函数；2》新的函数执行时，会调用 fn，调用形式为 fn.call(x, y, z)，其中 x 是 this，y 和 z 是其他参数】

### 3》如何实现数组去重

1>可以借鉴计数排序的思路,该方法的缺点是只支持数字或者字符串数组，如果数组里面有对象，比如 array=[{name:'www'},2],就会出错。
也可以直接将符合条件的结果 push 进结果中。

```
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
```

2>使用 set,缺点是 API 太新，旧浏览器不支持

```
function unique(arr){
    return [...new Set(arr)]
}
```

3>使用 Map,缺点是 API 太新，旧浏览器不支持

```
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
```

### 4》DOM 事件相关

事件委托：也被称作事件代理，就是利用事件冒泡的原理，把事件加到父元素或者祖先元素上，触发执行效果。即只用指定一个事件处理程序，就可以管理某一类型的所有事件。【不监听元素 A 自身，而是监听它的祖先元素 B，然后判断 e.target 是不是该元素 A（或该元素的子元素）】

事件委托的原理：事件委托是利用事件的冒泡原理实现的。什么是事件冒泡？就是事件从最深的节点开始，然后逐步向上传播事件。举个例子：页面上有这么一个节点树，div>ul>li>a;比如给最里面的 a 加一个 click 点击事件，那么这个事件就会一层一层的往外执行，执行顺序 a>li>ul>div，有这样一个机制，那么我们给最外面的 div 加点击事件，那么里面的 ul，li，a 做点击事件的时候，都会冒泡到最外层的 div 上，所以都会触发，这就是事件委托，委托它们父级代为执行事件。

事件委托的优点：1》提高 js 性能。事件委托比起事件分发（即一个 DOM 一个事件处 理程序），可以显著的提高事件的处理速度，减少内存的占用
2》动态的添加 DOM 元素，不需要因为元素的改 动而修改事件绑定

事件委托也有一定的局限性：比如 focus blur 之类的事件本身没有事件冒泡机制，故无法委托；mousemove mouseout 这样的事件，虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，因此也是不适合于事件委托的。

适合用事件委托的事件有：click mousedown mouseup keydown keyup keypress
所有鼠标事件中，只有 mouseenter mouseleave 不冒泡，其他鼠标事件都是冒泡的。

阻止默认动作：【e.preventDefault()或者 return false】

```
function myfn(e){
window.event? window.event.returnValue=false :e.preventDefault();
}
```

阻止事件冒泡：【e.stopPropagation()】

```
function myfn(e){
window.event? window.event.cancelBubble = true : e.stopPropagation();
}
```

### 5》js 的继承

基于原型的继承：【

```
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
```

】
基于 class 的继承：【

```
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
```

】

参考软老师的博客》Javascript 继承机制的设计思想

```
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
```

由于所有的实例对象共享同一个 prototype 对象，那么外界看来，prototype 对象就好像是实例对象的原型，而实例对象则好像“继承”了 prototype 对象一样。

```
class DOG{
       species="犬科";
       constructor(name){this.name = name;}

}
var dog3 = new DOG("doudou");
alert(dog3.species);//犬科
alert(dog3.name);//doudou
```

### 6》数组排序

### 7》对 Promise 的理解

0.Promise 是目前前端用于解决异步问题时的统一解决方案。Promise 可以理解为是一个容器，里面保存的是某个事件的结果（通常是一个异步操作的结果，而且是未来才会结束的）。Promise 是一个对象，一个异步操作的最终完成 (或失败), 及其结果值。从它可以获取异步操作的消息，它提供统一的 API，各种异步操作都可以用同样的方法进行处理。Promise 方案解决了不规范，容易出现回调地狱，很难进行错误处理等这些在解决异步编程问题时出现的问题。

1.创建 Promise 对象：

```
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
```

2.使用 Promise.prototype.then() 它的作用是为 Promise 实例添加状态改变时的回调函数。then 方法的第一个参数是 resolved 状态的回调函数，第二个参数（可选）是 rejected 状态的回调函数。使用 then 方法，返回的还是一个 Promise 对象，但这是一个新的 Promise 对象。所以，可以采用链式写法。一般来说，不要在 then 方法里面定义 Reject 状态的回调函数（即 then 的第二个参数），总是使用 catch 方法。Promise.prototype.catch 方法用于指定发生错误时的回调函数。就像下面写的那样，这样的好处是可以捕获前面 then 方法执行中的错误，也更接近同步的写法（try/catch）。另外要注意的是跟传统的 try/catch 代码块不同的是，如果没有使用 catch 方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。

```
Promise.prototype.then(function1(){}).catch(function2(){});//
```

3.Promise.all()用于将多个 Promise 实例包装成一个新的 Promise 实例。const p = Promise.all([p1, p2, p3]);这个代码中，Promise.all()方法接受一个数组作为参数，p1 p2 p3 都是 Promise 实例。如果不是，会将其转为实例再处理。另外，Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
p 的状态由 p1、p2、p3 决定，分成两种情况。（1）只有 p1、p2、p3 的状态都变成 fulfilled，p 的状态才会变成 fulfilled，此时 p1、p2、p3 的返回值组成一个数组，传递给 p 的回调函数。（2）只要 p1、p2、p3 之中有一个被 rejected，p 的状态就变成 rejected，此时第一个被 reject 的实例的返回值，会传递给 p 的回调函数。

4.Promise.race()也是将多个 Promise 包装成一个新的 Promise 实例。但与 all 方法不同的是，只要传递的参数实例中有一个实例率先改变状态，那么新的 Promise 实例的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给新的 Promise 实例的回调函数。Promise.race()方法的参数与 Promise.all()方法一样，如果不是 Promise 实例，就会先调用下面讲到的 Promise.resolve()方法，将参数转为 Promise 实例，再进一步处理。

【Promise 的用途：Promise 用于避免回调地域，让代码看起来更同步  
如何创建一个 new Promise：

```
function fn(){
    return new Promise((resolve,reject)=>{
        成功时调用 resolve(data);
        失败时调用 reject(reason);
    });
}
```

如何使用 Promise.prototype.then:fn().then(success,fail).then(success2,fail2).catch(fail3)  
如何使用 Promise.all:Promise.all([promise1,promise2])并行，等待所有 promise 成功。如果都成功了，则 all 对应的 promise 也成功，如果有一个失败了，则 all 对应的 promise 也失败。  
如何使用 Promise.race:Promise.all([promise1,promise2])，返回一个 promise，一旦数组中的某个 promise 解决或拒绝，返回的 promise 就会解决或拒绝。
】

### 8》跨域

同源：如果两个 url 的协议，域名，端口号完全一致，那么这两个 url 就是同源的。同源策略是浏览器为了用户隐私，数据安全，故意设计的一个功能限制，不同源的页面之间，不准互相访问数据。

跨域：跨域就是突破浏览器的同源策略限制，不同源的页面之间，可以访问数据。

JSONP 跨域：浏览器不支持 CORS 跨域的另一种跨域方式。它利用的是浏览器不限制对 js 的引用，我们可以随意引用 js。这样的话，我们去请求一个 js 文件，这个 js 文件会执行一个回调，里面有我们要的数据。由于是 script 标签，不能像 ajax 那样，监听到返回码，即不知道返回的状态码是什么，也不知道响应头是什么，只知道成功或者失败。由于是 script 标签，只能发 get 请求，不支持 post。

CORS 跨域：CORS 是一个 W3C 标准，全称是"跨域资源共享"（Cross-origin resource sharing）。它允许浏览器向跨源服务器，发出 XMLHttpRequest 请求，从而克服了 AJAX 只能同源使用的限制。它的使用就是去设置响应头，将允许访问的网址添加进去。

### 9》1 使用 promise 的简单封装 ajax（Axios jQuery.ajax） 2 跨域 异步回调

```
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
```

小结：
第一步 return new Promise((resolve,reject)=>{...});任务成功则调用 resolve(result)，任务失败则调用 reject(error)，resolve 和 reject 则会再去调用成功和失败的函数。
第二步使用.then(success,fail)传入成功和失败函数。

目前封装的比较简单，不能 post 上传数据，不能设置请求头。可以用 jQuery.ajax 或者 axios 去解决。

```
function jsonp(url){
 return new Promise((resolve,reject)=>{

     let rannum = "ff"+Math.random();
     window[rannum] = (res)=>{
         resolve(res);
     }
     let scr = document.createElement("script");
     scr.src = `${url}?callback=${rannum}`;
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
```

上面的封装要注意，在服务端，处理这个请求的时候，接收 callback 传递的函数名，对应的 js 文件中执行这个函数回调，返回我们所要的数据。
