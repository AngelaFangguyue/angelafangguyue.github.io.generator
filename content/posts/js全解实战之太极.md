---
title: "Js全解实战之太极"
date: 2019-12-08T20:47:50+08:00
draft: false
categories:
  - js
tags:
  - js
featured_image:
---

### 本篇文章是 js 全解实战之太极,讲的是做一个旋转的太极，并且在页面上动态的展示所写的代码。主要内容有：  
[1:重点一：将所写的内容动态显示在页面上](#a1)

[2:重点二：将所写的CSS样式应用在页面上](#a2)

[3:重点三：画会动的太极](#a3)

[4:重点四：适配移动端](#a4)

<b style="font-size:20px;">点击查看[预览效果](http://angelafang.top/task29parcel_cv/dist/first.html)。</b>

#### <span style="color:red;">重点一：将所写的内容动态显示在页面上</span>{#a1}

&nbsp;&nbsp;&nbsp;&nbsp;<b>1》动态展示：</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;setTimeout 可以间隔一个固定的时间去执行这个操作，但是该操作只执行一次，setInterval 可以连续的操作，每次都间隔一个固定的时间。但是考虑到 setInterval 不方便取消操作，一般采用的是递归的 setTimeout。利用递归的 setTimeout 函数模拟 setInterval，可以随时停止，这是它的好处。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;做法：将所做的操作写作一个函数 fun1，里面用到了 setTimeout 函数，然后在 setTimeout 这个函数体内写所需要的操作，并且继续调用 fun1 这个函数本身。当然，在调用 fun1 函数的时候，需要加判断，当满足终止条件后，就不再调用 fun1 函数，return 即可。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;例子：(html 页面里有一个 id 为 wenzi 的 span 标签，标签里会动态的展示数字，从 0 依次加 1，每 1s 变一次，直至变到 10 之后停止。)下面是 js 代码，主要看函数 fun1！

    let wenzi = document.queryselector("#wenzi");
    let n = 0;
    let fun1 = ()=>{
        seTimeout(()=>{wenzi.innerHTML = n;
        n+=1;
        if(n<=10){
           fun1();
        }
    },1000);};

&nbsp;&nbsp;&nbsp;&nbsp;<b>2》动态展示一段文字：</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上面的是动态展示一个，接下来要让其动态展示一段文字。动态展示一段文字，使用到 string 字符串的 substring 函数。<span style="color:red;">（在声明字符串的时候，可以用反引号）</span>如图所示：
![](/images/task29_js/js1.PNG)

#### <span style="color:red;">重点二：将所写的CSS样式应用在页面上</span>{#a2}

&nbsp;&nbsp;&nbsp;&nbsp;<b>3》如何将 css 也应用到这个页面上：</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;解决思路，将样式代码写入到 style 标签里。这样就需要在 index.html 文件里加一个 id 为 sty1 的 style 标签，在 main.js 文件里获取这个元素，并向该元素中写入 css 样式内容。
如图所示：
![](/images/task29_js/html1.PNG)
![](/images/task29_js/js2.PNG)
![](/images/task29_js/js3.PNG)
效果：
![](/images/task29_js/xg1.PNG)

&nbsp;&nbsp;&nbsp;&nbsp;<b>4》文字细节问题：</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;写的文字需要换行，但是在 html 页面里显示的时候，回车和多个空格都会只被当做一个空格输出在页面中。所以需要使用 html 中的表示换行的br标签和表示空白的&nbsp来表示回车和换行。替换用到 string 的 replace 函数。但是替换之后，需要注意，这时候换行的br标签和表示空白的&nbsp的字符个数就已经变为 4 个和 5 个，不能再使用 string.substring 去获得所要显示的内容。解决办法：再定义一个字符串 string2，来存储每次需要显示的文字。

&nbsp;&nbsp;&nbsp;&nbsp;<b>5》具体实现：</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那接着我们就可以把在页面上显示的字也写入到 style 标签里，让里面的 css 样式生效。此时有两点需要注意的：1 是 css 样式里不允许有 html 代码，所以我们不能用 string2 去填写，继续使用 string.substring 即可；2 是由于要显示的字不仅有 css 样式，还有其他文字，这样样式也不会生效，需要将非 css 样式文字注释起来。下图是将整个页面背景色变为灰色。如图所示：
![](/images/task29_js/js4.PNG)
![](/images/task29_js/js5.PNG)
![](/images/task29_js/js6.PNG)
效果：
![](/images/task29_js/xg2.PNG)

#### <span style="color:red;">重点三：画会动的太极</span>{#a3}

&nbsp;&nbsp;&nbsp;&nbsp;<b>6》接下来写一个会动的太极</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;首先画一个圆 div#taiji，可以在html文件中先将这个圆的位置固定，然后在js文件中画圆。代码如图：
![](/images/task29_js/js21.PNG)
![](/images/task29_js/js22.PNG)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接着使用背景渐变将圆分为左白又黑。这里推荐[CSS Gradient generator](https://cssgradient.io/)可以在这里面在线调试 css 效果。
代码如图：
![](/images/task29_js/js23.PNG)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;然后利用伪元素去画阴阳鱼。伪元素是利用 css 写出一个类似 span 标签的元素。div#::before 和 div::after，要设置它们的 content:'',display:block;还要对这两个元素定位。这些也可以先写在html文件里，其余的样式继续写在js文件中。<span style="color:red;">居中定位有一个常用的套路，left 和 transform 相结合。</span>代码如图：
![](/images/task29_js/js24.PNG)
![](/images/task29_js/js25.PNG)
![](/images/task29_js/js26.PNG)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接着继续使用背景颜色渐变在阴阳鱼的中间显出一个圆。
![](/images/task29_js/js19.PNG)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后给其加上动画。代码如图：
![](/images/task29_js/js20.PNG)
效果：
![](/images/task29_js/xg3.PNG)

&nbsp;&nbsp;&nbsp;&nbsp;<b>7》改善。</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1：当页面上显示的字过多之后，页面不会自动向下显示，需要用户手动滑动,需要进行设置。设置 window.scrollTo(0,9999);2：显示的文字不会折行,设置显示文字的div元素的 word-break 属性为 break-all。<span style="color:red;">这里注意当写的内容过多的时候，就不要使用span元素来显示了，该换为div元素。</span>代码如图：
![](/images/task29_js/scroll.PNG)
![](/images/task29_js/wb.PNG)

#### <span style="color:red;">重点四：适配移动端</span>{#a4}
&nbsp;&nbsp;&nbsp;&nbsp;<b>8》适配移动端</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一要加 meta 标签，二是要用到媒体查询。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;遇到的问题，首先移动端的话，屏幕的宽度有限制，这样就需要在移动端设置 div#wenzi 和 div#taiji 为上下结构。这样 div#taiji 的位置等都需要进行调整。另外因为设置其平分这个屏幕，各占可视区域的 50%，即 50vh。后面又需要给其设置具体宽高。所以 div#taiji 外面还需要一个 div#wrapper 来包裹。而且太极的大小也要调整，由之前的400px变为200px。手机上显示的文字不能全屏显示，要设置overflow:hidden;同时也要让html页面自行拖动，即像设置window.scrollTo(0,9999)一样，设置html.scrollTo(0,9999)。代码如图：
![](/images/task29_js/html2.PNG)
![](/images/task29_js/html3.PNG)

这个过程中遇到很多小细节问题需要去调整修改完善，在这里就不一一说了。学习就是这样，不断的去实践，在实践中发现问题，解决问题，深化对知识的理解掌握。

不忘初心，努力，坚持，等待，希望，加油。



