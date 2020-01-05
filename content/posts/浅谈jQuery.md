---
title: "浅谈jQuery"
date: 2020-01-05T11:39:13+08:00
draft: false
categories:
  - js jQuery 设计模式
tags:
  - js jQuery
featured_image:
---

---

<b>Everyday is an opportunity to learn and grow。

现在是公元 0202 年，1 月 5 日，腊月十一。距离农历新年还不到 二十 天了，加油鸭</b>

---

本篇文章主要总结梳理最近一段时间对 jQuery 的学习理解，主要内容：

[1:jQuery 的设计思想](#a1)  
[2:具体分析](#a2)

&nbsp;&nbsp;&nbsp;&nbsp;[2.1:获取元素](#a21)

&nbsp;&nbsp;&nbsp;&nbsp;[2.2:元素操作](#a22)

&nbsp;&nbsp;&nbsp;&nbsp;[2.3:链式操作](#a23)

---

<span style="font-size:20px;">前言：</span>  
&nbsp;&nbsp;&nbsp;&nbsp;
jQuery 是目前使用最为广泛的 js 函数库，经久不衰。这在前端业界内是一个奇观，目前还没有哪一个库能像它这样，有长达十几年的寿命，地位仍屹立不倒，不得不称之为经典。这要归功于它的设计思想。感谢那些为此付出了心血的前辈们。对于网页开发者来说，学好 jQuery 是必要的。通过学习 jQuery，我们可以了解业界最通用的技术，为将来学习更高级的库打基础。

---

自己的碎碎念：

jQuery 就是获取元素，进行操作。通过 jQuery 构造函数构造出一个 jQuery 对象，该对象可以对选择到的元素进行操作（使用了闭包来隐藏细节，只要 jQuery 对象存在，对应要获取的元素就也一直存在。），操作之后返回的还是 jQuery 对象，因此可以进行链式操作。jQuery 别出心裁的没有使用 new 来构造对象;而且用别名$.fn代替$.prototype,jQuery()也简写为$();闭包隐藏细节;getter/setter;$()可以接受不同的参数，重载;以及根据不同的浏览器采用不同的代码，适配。

---

### 1:jQuery 的设计思想{#a1}

<strong>jQuery 核心思想是：<em>获取元素，对其操作</em>。</strong>

&nbsp;&nbsp;&nbsp;&nbsp;在使用 jQuery 的时候，首先根据选择需要，去获取相对应的元素，接着返回一个 jQuery 对象，而不是返回获取到的元素。重点在于返回的这个 jQuery 对象，提供了许多 api，去操作获取的元素！！！  
&nbsp;&nbsp;&nbsp;&nbsp;(这用到了使用闭包来隐藏细节，只要这个 jQuery 对象一直存在，查找到的元素也会一直存在。还有链式操作，因为每次操作，返回的都是 jQuery 对象。)  
&nbsp;&nbsp;&nbsp;&nbsp;这样我们就可以对目标元素进行一系列的操作，所有操作都可以连接在一起，以链条的形式写出来，这就是<strong>链式操作</strong>。这是因为<strong>jQuery 的每一步操作，返回的都是一个 jQuery 对象。</strong>另外 jQuery 还提供了 end()方法来返回到上一个结果集。

---

### 2:具体分析{#a2}

下面跟着具体的例子来了解 jQuery 的设计思想。我们在使用 jQuery 的时候，

#### 2.1:获取元素{#a21}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong>第一步：往往就是将一个选择表达式，放进构造函数\_ jQuery()（简写为$()），然后得到被选中的元素。</strong>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如$("#test"),就是要去获取 id 为 test 的元素。要注意$("#test")它本身是一个对象，里面有方法，可以对选中的 id 为 test 的元素进行操作。即若 let test = \$("#test")，test 是一个 jQuery 对象，利用这个对象以及对象里的方法，我们可以去对获取到的 id 为 test 的元素进行操作。当然获取到的元素可能有多个。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;选择表达式可以是 CSS 选择器：

    $(document) //选择整个文档对象
    $('#myId') //选择 ID 为 myId 的网页元素
    $('div.myClass') // 选择class为myClass的div元素
    $('input[name=first]') // 选择 name 属性等于 first 的 input 元素

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;也可以是 jQuery 特有的表达式：

    $('a:first') //选择网页中第一个a元素
    $('tr:odd') //选择表格的奇数行
    $('#myForm :input') // 选择表单中的input元素
    $('div:visible') //选择可见的 div 元素
    $('div:gt(2)') // 选择所有的div元素，除了前三个
    $('div:animated') // 选择当前处于动画状态的 div 元素

#### 2.2:元素操作{#a22}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong>第二步：对获取到的元素进行操作。jQuery 中提供了很多对元素的操作。</strong>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1》有对获取到的元素再进行过滤筛选等。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.1》提供各种强大的过滤器，对结果集进行筛选，缩小选择结果

    $('div').has('p'); // 选择包含p元素的div元素
    $('div').not('.myClass'); //选择 class 不等于 myClass 的 div 元素;
    $('div').filter('.myClass'); //选择class等于myClass的div元素
    $('div').first(); //选择第 1 个 div 元素
    $('div').eq(5); //选择第6个div元素
    $('div').find('h3') //找到div元素，选择其中的h3元素。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.2》从结果集出发，移动到附近的相关元素，jQuery 也提供了在 DOM 树上的移动方法

    $('div').next('p'); //选择 div 元素后面的第一个 p 元素
    $('div').parent(); //选择 div 元素的父元素
    $('div').closest('form'); //选择离 div 最近的那个 form 父元素
    $('div').children(); //选择 div 的所有子元素
    $('div').siblings(); //选择 div 的同级元素

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2》有给元素添加一些属性等，

    $("#test").addClass("cc")//这个就是给 id 为 test 的元素添加一个 cc 的样式类名。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3》获取元素的值，或者给元素赋值等，

    $('h1').html();//html()没有参数，表示取出h1的值。
    $('h1').html('Hello');//html()有参数 Hello，表示对 h1 进行赋值。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;常见的取值和赋值函数如下：

    .html() 取出或设置 html 内容；
    .text() 取出或设置 text 内容；
    .attr() 取出或设置某个属性的值；
    .width() 取出或设置某个元素的宽度；
    .height() 取出或设置某个元素的高；
    .val() 取出某个表单元素的值

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【这种思想：就是使用同一个函数，来完成取值（getter）和赋值（setter），即"取值器"与"赋值器"合一。到底是取值还是赋值，由函数的参数决定。】

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4》移动元素的位置。

    $('div').insertAfter($('p'));//把 div 元素移动 p 元素后面,
    $('p').after($('div'));也可以起到同样的作用。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;另外使用这种模式的操作方法，一共有四对：

    .insertAfter()和.after()：在现存元素的外部，从后面插入元素；
    .insertBefore()和.before()：在现存元素的外部，从前面插入元素；
    .appendTo()和.append()：在现存元素的内部，从后面插入元素；
    .prependTo()和.prepend()：在现存元素的内部，从前面插入元素。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5》复制，删除，新增元素：

    复制元素使用.clone()。
    删除元素使用.remove()和.detach()。//两者的区别在于，前者不保留被删除元素的事件，后者保留，有利于重新插入文档时使用。
    清空元素内容（但是不删除该元素）使用.empty()。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;创建新元素的方法非常简单，只要把新元素直接传入 jQuery 的构造函数就行了：

    $('<p>Hello</p>');
    $('<li class="new">new list item</li>');
    $('ul').append('<li>list item</li>');

#### 2.3:链式操作{#a23}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong>第三步：链式操作，对选中的网页元素进行一系列操作，所有操作可以连接在一起，以链条的形式写出来。</strong>

    $('div').find('h3').eq(2).html('Hello');

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;分解开来，就是下面这样：

    $('div') //找到div元素
    .find('h3') //选择其中的h3元素
    .eq(2) //选择第3个h3元素
    .html('Hello'); //将它的内容改为Hello

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这是 jQuery 最令人称道、最方便的特点。它的原理在于每一步的 jQuery 操作，返回的都是一个 jQuery 对象，所以不同操作可以连在一起。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;jQuery 还提供了.end()方法，使得结果集可以后退一步：

    $('div')
    .find('h3')
    .eq(2)
    .html('Hello')
    .end() //退回到选中所有的h3元素的那一步
    .eq(0) //选中第一个h3元素
    .html('World'); //将它的内容改为World

以上只是对 jQuery 的一个初步介绍，jQuery 还有很多东西，比如 工具方法 、事件操作 、动画等。如果要想掌握好 jQuery 还是需要从项目中来。当然前端发展到今天 jQuery 大家可能都不用了，但是我们还是可以学习一下它的一些思想，也利于我们更好的去掌握更加新的框架，万变不离其宗，加油呀！

参照 jQuery 的思想，自己封装了一些方法 [dom-api](https://github.com/AngelaFangguyue/dom_api_jQuery) 。

参考学习资料：

[jQuery 设计思想](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)

[jQuery API 中文文档](https://www.jquery123.com/)
