---
title: "CSS入门2"
date: 2019-11-19T17:54:53+08:00
draft: true
categories:
  - css
tags:
  - css layout position animation
featured_image:
---

本篇文章开始具体介绍 CSS,主要内容有：  
[1:CSS 布局](#a1)  
[2:CSS 定位](#a2)

<!-- [3:CSS 动画](#a3)   -->

[3:浏览器渲染原理](#a4)  
[4:CSS 动画的两种做法（transition 和 animation）](#a5)

---

## **这里区分一下布局和定位的区别：布局是在屏幕平面上的，而定位则是垂直于屏幕屏幕**

### 1:CSS 布局 {#a1}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;布局分为固定宽度布局和不固定宽度布局。顾名思义，固定宽度就是宽度是指定的，确定的，无法变化，宽度一般有 960px，1000px，1024px 等。此外还有两者都结合的响应式布局，即 pc 上固定宽度，但在移动端是不固定宽度的。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
**一》在布局的时候，考虑的思路如下：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1：需要兼容 IE9 吗？是的话，就采用 float 布局。不是的话，进入第二步；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2：只兼容最新浏览器吗？是的话，就采用 grid 布局。不是的话，就用 flex 布局。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**注意：当使用 float 或者 flex 布局的时候，有时候会采用负的 margin 值。使用 float 布局的时候，一定不要忘了给父元素清除浮动.clearfix{content:'';display:block;clear:both;}**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
**二》使用 float 布局**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
给子元素设置 width 和 float:left/right。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
给父元素添加.clearfix。<code>.clearfix::after{content:'';display:block;clear:both;}</code>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
一般最后一个不设置宽度或者留一些空间，也可以设置一个最大宽度。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
用 float 布局就不需要做响应式。（因为手机上没有 IE，而这个布局是专为 IE 设计的）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
IE6/7 存在双倍 margin bug。解决方法，把 margin 值减半或者将元素 display：inline-block。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
实践：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用 float 布局可以做两栏，三栏，平均布局等布局。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**这里注意当在做平均布局的时候，需要使用到负 margin。**像下面的例子中，如果不加 wrapper 这个 div，是无法做到平均布局的。需要添加 wrapper，而且还需要给其设置一个负的 margin，这样才能达到平均布局的效果。另外 clearfix 不要忘记添加。

      <div class="content">
      <div class="wrapper clearfix">
      <div class="item1">item1</div>
      <div class="item2">item2</div>
      <div class="item3">item3</div>
      <div class="item4">item4</div>
      <div class="item5">item5</div>
      <div class="item6">item6</div>
      <div class="item7">item7</div>
      <div class="item8">item8</div>
      <div class="item9">item9</div>
      <div class="item10">item10</div>
      <div class="item11">item11</div>
      <div class="item12">item12</div>
      <div class="item13">item13</div>
      </div>
      </div>

<code>.clearfix::after{content:'';display:block;clear:both;}</code>

<code>div.content{outline:1px solid black; width:800px;}</code>

<code>div.wrapper>div{outline:1px solid red; width:191px;height:191px;float:left;margin-right:12px;margin-bottom:20px;}</code>

<code>div.wrapper{outline:1px solid blue; margin-right:-12px;}</code>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(小 tips：  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1》有的时候，在 box-sizing 为 border-box 的情况下，给元素设置 border 会影响到例子的效果，可以将 border 写为 outline。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2》一般 CSS RESET 的时候，将 box-sizing 设置为 border-box。把 img 的宽度设为 100%，或者最大宽度设为 100%。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3》在 div 元素中放入 img 标签，div 下方会有多余的空隙，这时只需要给 img 设置属性 vertical-align:top/middle 即可去掉多余的空隙。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4》块级元素，宽度固定，想让其居中，最好分别写上 margin-left:auto 和 margin-right:auto。用 margin 属性会覆盖掉之前使用 margin-top 或者 margin-bottom 设置的值。而对于不固定高度的元素，想让其上下垂直居中，用 flex 布局可以完美解决，float 布局不方便。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5》float 布局对于 IE 足够使用。手机页面不要使用 float 布局。float 布局需要计算宽度，不灵活。)</b>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
**三》使用 flex 布局**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
父元素 container 添加属性 display:flex/inline-flex。即使得该元素成为一个 flex 容器。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;浮动方向 flex-direction:row/column/row-reverse/column-reverse

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;是否折行 flex-wrap:nowrap/wrap/wrap-reverse

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在主轴方向上，多余空间如何分配，即主轴对齐方式（默认主轴是横轴，除非改变了 flex-direction。）justify-content:flex-start/center/flex-end/space-between/space-around/space-evently

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;次轴对齐 align-items:stretch/flex-start/center/flex-end/baseline

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;多行内容如何分布 align-content:flex-start/center/flex-end/stretch/space-between/space-around

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;里面 items 的样式：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;order 默认值是 0，数值越小越靠前。flex-grow 多余的空间如何分配（控制自己如何长胖）。flex-shrink 在空间不够的情况下，如何缩小（控制自己如何变瘦）；一般写 flex-shrink：0，防止变瘦；默认值是 0。flex-basis 控制基准宽度。flex 是 flex-grow、flex-shrink、flex-basis 三者的缩写。align-self 定制 align-items，即设置自己的对齐方式。

<b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(小 tips：  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1》在 flex 布局中，使用 justify-content：space-between，可以使得多余的空间在元素中间分配，右边的元素在紧靠右边的地方排列，左边的元素在紧靠左边的地方排列。其实给右边的子元素一个 margin-left：auto，或者左边的一个元素 margin-right：auto，也可达到这种效果。

    <div class="header">
    <div class="left">left</div>
    <div class="right">right</div>
    </div>

<code>.header{border:1px solid black;display:flex;justify-content:space-between;}</code>

或者 style 样式这样写<code>.header{display:flex;}</code>
<code>.left{margin-right:auto;}</code>

或者 style 样式这样写<code>.header{display:flex;}</code>
<code>.right{margin-left:auto;}</code>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2》一般不要将宽度，高度固定写死，除非特殊说明。可以用 min-width,min-height,max-width,max-height 设置宽高。另外也可以使用百分比。flex 布局和 grid 布局都可以参考 css tricks！！！</b>

---

### 2:CSS 定位 {#a2}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;定位 position 有五个取值

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1:relative,相对于自身的定位，位移。原位置会保留，会遮盖住它后面正常的元素。一般使用 relative，结合 top 等做居中，但其实用 flex 也可实现。另外一个用法就是给 position：absolute 的元素做父元素。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2:absolute,相对于父元素的绝对定位。父元素是离它最近的那个定位的元素，只要这个父元素定位了即可，并不是 position 值为 relative 的才是。使用场景：脱离原来的位置，另起一层，比如说对话框右上方的关闭按钮；或者是鼠标悬浮时出现的提示。某些浏览器上不写 top，right，bottom，left 等会位置错乱。善于利用 top，right，bottom，left 为 100%。利用 top，right，bottom，left 为 50%，再加上负 margin 或者 transform:translate(-50%)可以让其居中。（white-space：nowrap；控制文字内容不换行。）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3:fixed,相对于可视窗口的固定定位。使用场景：烦人的广告；页面下方回到顶部的按钮。这里要注意：移动端尽量不要使用 fixed。fixed 和 transform 不要在一起使用，会使得定位有差别。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4:sticky,粘滞定位，兼容性很差，这里不做介绍。另外 position 的默认值是 static。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5:经验，如果写了 absolute，一般要给父元素补一个 relative；如果写了 absolute 或者 fixed，一般要不 top，left，bottom，right 这四个中的一个或多个。sticky 兼容性很差。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6:z-index 的问题，默认定位的元素 z-index 值为 auto。这里涉及到一个层叠上下文的问题。一个 div 的分层从下到上依次是：background，border， 块级子元素，浮动元素，内联元素。如果元素定位了，那么元素高度就会处于内联元素的上面。如果设置其 z-index 为负值，那么就会处于 background 的后面。这整个的内容是处于一个层叠上下文中。即 z-index 必须作用于一个小世界或者最外面的小世界，或者某一个小世界。默认的层叠上下文是 HTML 元素。有些元素会因为拥有某些属性而变成层叠上下文，造成里面的 z-index 会重新计算。负的 z-index 有可能逃不出一个元素的背景，有可能该元素变成了层叠上下文。每个层叠上下文都是一个新的小世界，这个小世界里的 z-index 与外界无关，只有处于同一个小世界的 z-index 才能比较。当一个元素定位之后，并且 z-index 的值不为 auto，或者设置了 flex，opacity，transform 等属性，都会成为层叠上下文。具体可以查看[MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context)。

---

### 3:浏览器渲染原理 {#a4}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;浏览器的渲染原理大致主要分为六步：  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1》根据 html 构建 HTML 树(DOM)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2》根据 css 构建 CSS 树(CSSDOM)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3》将上面两棵树合并成渲染树(RENDER TREE)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4》进行 layout 布局  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5》进行 paint 绘制  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6》合成 composite

---

### 4:CSS 动画的两种做法（transition 和 animation） {#a5}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1》使用 transition。具体可以参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;transition 过渡属性，可以自动脑补中间帧。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
transition CSS 属性是 transition-property，transition-duration，transition-timing-function 和 transition-delay 的一个简写属性。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;过渡可以为一个元素在不同状态之间切换的时候定义不同的过渡效果。比如在不同的伪元素之间切换，像是 :hover，:active 或者通过 JavaScript 实现的状态变化。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;transition 属性可以被指定为一个或多个 CSS 属性的过渡效果，多个属性之间用逗号进行分隔。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;举例：
html 代码里有一个类为 demo 的 div，style 属性设置其宽高都为 100px，背景颜色是红色。当鼠标悬浮在 demo 上时，demo 的宽变为 200px，背景色变为黑色。这时使用 transition 过渡属性，设置当宽度，背景颜色改变时，过渡时间持续 1s。

    <div class="demo"><div>

<code>.demo{
width:100px;
height:100px;
background:red;
transition:width 1s,background 1s;
}</code>

<code>.demo:hover{
width:200px;
background:black;
}</code>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2》使用 animation。具体可以参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CSS animation 属性是 animation-name，animation-duration, animation-timing-function，animation-delay，animation-iteration-count，animation-direction，animation-fill-mode 和 animation-play-state 属性的一个简写属性形式。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;animation 属性用来指定一组或多组动画，每组之间用逗号相隔。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;举例：html 代码里有一个类为 demo 的 div，style 属性设置其宽高都为 100px，背景颜色是红色。设置一个动画，让 demo 的宽度从 100px 变化到 200px,与此同时，背景颜色从红色变为黑色。整个过渡时间持续 2s,且一直以 ease 的速度变化循环。**注意要使用@keyframes 定义动画！！！**

    <div class="demo"></div>

<code>@keyframes demoanimation{
0%{};
100%{width:200px;background:black;}
}</code>

<code>.demo{
width:100px;
height:100px;
background:red;
animation:demoanimation 2s ease infinite alternate ;
}</code>

<code>.demo:hover{
width:200px;
background:black;
}</code>
