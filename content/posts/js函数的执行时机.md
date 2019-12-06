---
title: "Js函数的执行时机"
date: 2019-12-06T08:45:30+08:00
draft: false
categories:
  - js js函数 同步 异步 闭包 作用域 立即执行函数
tags:
  - js js函数 同步 异步 闭包 作用域 立即执行函数
featured_image:
---

本篇文章介绍 js 函数的执行时机：

---

**请看下面的代码：**

    let i=0;
    for(i=0;i<6;i++){
    console.log(i);
    }

执行后的结果：
![](/images/task28/for1.PNG)

---

**再看下面这个代码：**

    let i=0;
    for(i=0;i<6;i++){
    setTimeout(()=>console.log(i),10);
    }

执行后的结果：
![](/images/task28/for2.PNG)

---

**上面两个结果不同，原因在哪里？原因就出在 setTimeout 这个函数上面。**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:red">最上面的代码，它的执行时机是按照顺序，依次执行，循环一下，打印一下 i 的值。但是 setTimeout 函数，是要等待现在的代码执行完成后，再去执行 setTimeout 里的内容。针对上面的代码，就是把 for 语句循环完成了以后，再去执行 cosole.log(i),这时 i 的值已经变为 6，所以会输出 6 个 6。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:red">（setTimeout(语句 W,10)这里的 10 表示是等待现在的代码执行完，10 毫秒后就立即执行 setTimeout 里面的语句 W，若为数字 X，表示执行完现在的代码，等待 X 毫秒后，去执行语句 W）</span>。

---

**那么如何才能在第二个代码中，让它的结果也能依次输出 0，1，2，3，4，5 呢？**

<span style="color:red">js 给我们提供了 let 和 for 搭配使用，就可以办到。原理：let 为代码块的作用域，所以每一次 for 循环，console.log(i); 都引用到 for 代码块作用域下的 i，因为这样被引用，所以 for 循环结束后，这些作用域在 setTimeout 未执行前都不会被释放。</span>

    for(let i=0;i<6;i++){
       setTimeout(()=>console.log(i),10); console.log(i);
    }

执行后的结果：
![](/images/task28/for3.PNG)

<i>注意：这里的 i 变量是在 for 循环内部声明定义的！！！</i>

---

**除了使用 for 和 let，我们还可以使用闭包来解决这个问题。原理是声明了立即执行函数，保存了当时的值到内部。这样 console.log(i); 中的 i 就保存在每一次循环生成的立刻执行函数中的作用域里了（参考[文章 1](https://www.jb51.net/article/122489.htm) [文章 2](https://www.cnblogs.com/huchong-bk/p/11757786.html) [文章 3](https://www.cnblogs.com/wangwenhui/p/7657654.html)）**

    for(var i=0;i<6;i++>){
            (function(i){   //立即执行函数
                setTimeout(function(){console.log(i);},0);
                })(i);
    }

执行后的结果：
![](/images/task28/for4.PNG)

---

上面的代码，涉及到了 js 执行时异步（同步），闭包（立即执行函数），作用域等这些知识点。

<b style="color:red">分析上面的代码，setTimeout 是异步执行，10ms 后往任务队列里面添加一个任务，只有主线上的全部执行完，才会执行任务队列里的任务，当主线执行完成后，i 是 6，所以此时再去执行任务队列里的任务时，i 全部是 6 了。对于打印 6 次是：
每一次 for 循环的时候，settimeout 都执行一次，但是里面的函数没有被执行，而是被放到了任务队列里面，等待执行，for 循环了 6 次，就放了 6 次，当主线程执行完成后，才进入任务队列里面执行。
　　（注意：for 循环从开始到结束的过程，需要维持几微秒或几毫秒。)
当我把 var 变成 let 时

     for(let i = 0;i < 6;i++){
     setTimeout(function() { console.log(i)},0);
     }

打印出的是：0,1,2,3,4,5

当解决变量作用域后，因为 for 循环头部的 let 不仅将 i 绑定到 for 循环块中，事实上它将其重新绑定到循环体的每一次迭代中，确保上一次迭代结束的值重新被赋值。setTimeout 里面的 function()属于一个新的域，通过 var 定义的变量是无法传入到这个函数执行域中的，通过使用 let 来声明块变量，这时候变量就能作用于这个块，所以 function 就能使用 i 这个变量了；这个匿名函数的参数作用域 和 for 参数的作用域 不一样，是利用了这一点来完成的。这个匿名函数的作用域有点类似类的属性，是可以被内层方法使用的。</b>
