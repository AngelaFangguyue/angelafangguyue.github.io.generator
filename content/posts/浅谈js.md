---
title: "浅谈js"
date: 2019-11-25T11:35:40+08:00
draft: false
categories:
  - js
tags:
  - js
featured_image:
---

本篇文章是关于 js 的概览，主要内容：

[1:常见浏览器及其内核介绍](#a1)  
[2:JS 发展历史以及标准](#a2)

---

### 1:常见浏览器及其内核介绍{#a1}

**1：浏览器内核简要说明：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一个完整的浏览器包含浏览器内核和浏览器的外壳（shell）。浏览器内核又可以分为两部分：渲染引擎（Layout Engine 或 Rendering Engine）和 JS 引擎。由于 JS 引擎越来越独立，内核就倾向于只指渲染引擎。

**2：常见的浏览器及其内核介绍：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;浏览器的内核的种类很多，常见的浏览器内核可以分为四种：Trident、Gecko、Blink、Webkit。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（1）Trident (IE 内核):国内很多的双核浏览器的其中一核便是 Trident，美其名曰 "兼容模式"。代表： IE、傲游、世界之窗浏览器、Avant、腾讯 TT、猎豹安全浏览器、360 极速浏览器、百度浏览器等。Window10 发布后，IE 将其内置浏览器命名为 Edge，Edge 最显著的特点就是新内核 EdgeHTML，但其实现 Edge 浏览器使用的是 Chrome 浏览器的内核。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（2）Gecko(firefox):Mozilla FireFox(火狐浏览器) 采用该内核，Gecko 的特点是代码完全公开，因此，其可开发程度很高，全世界的程序员都可以为其编写代码，增加功能。 可惜这几年已经没落了， 比如打开速度慢、升级频繁。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（3）webkit(Safari):Safari 是苹果公司开发的浏览器，所用浏览器内核的名称是大名鼎鼎的 WebKit。代表浏览器：傲游浏览器 3、 Apple Safari (Win/Mac/iPhone/iPad)、Symbian 手机浏览器、Android 默认浏览器。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（4）Chromium/Bink(chrome):在 Chromium 项目中研发 Blink 渲染引擎（即浏览器核心），内置于 Chrome 浏览器之中。Blink 其实是 WebKit 的分支。大部分国产浏览器最新版都采用 Blink 内核。Chrome 浏览器内核：统称为 Chromium 内核或 Chrome 内核，以前是 Webkit 内核，现在是 Blink 内核；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（5）Presto (Opera):Presto 是挪威产浏览器 opera 的 "前任" 内核，最新的 opera 浏览器早已将之抛弃从而投入到了谷歌怀抱了。Opera 浏览器内核：最初是自己的 Presto 内核，后来是 Webkit，现在是 Blink 内核。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**[简书](https://www.jianshu.com/p/f4bf35898719)上面这篇文章介绍得还挺详细准确的。**

---

### 2:URL {#a2}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**JS 之父布兰登如此评价 js：原创之处并不优秀，优秀之处并非原创。**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JavaScript 最初开发于 1996 年，被使用于 Netscape Navigator 网页浏览器。同年微软在 Internet Explorer 发布了一个实现。由于商标问题，这项实现被命名为 JScript。1997 年，JavaScript 已经被网景公司提交给 ECMA 制定为标准，称之为 ECMAScript，标准编号 ECMA-262。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1:一般来说，完整的 JavaScript 包括以下几个部分：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1)ECMAScript，描述了该语言的语法和基本对象

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2)文档对象模型（DOM），描述处理网页内容的方法和接口

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(3)浏览器对象模型（BOM），描述与浏览器进行交互的方法和接口

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2:JavaScript 的基本特点如下：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1)是一种解释性脚本语言（代码不进行预编译）。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2)主要用来向 HTML 页面添加交互行为。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(3)可以直接嵌入 HTML 页面，但写成单独的 js 文件有利于结构和行为的分离。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3:JavaScript 常用来完成以下任务：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1)嵌入动态文本于 HTML 页面

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2)对浏览器事件作出响应

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(3)读写 HTML 元素

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(4)在数据被提交到服务器之前验证数据

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(5)检测访客的浏览器信息
控制 cookies，包括创建和修改等

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**4:不同于服务器端脚本语言，例如 PHP 与 ASP，JavaScript 主要被作为客户端脚本语言在用户的浏览器上运行，不需要服务器的支持。**所以在早期程序员比较青睐于 JavaScript 以减少对服务器的负担，而与此同时也带来另一个问题：安全性。而随着服务器变得强大，现在的程序员更喜欢运行于服务端的脚本以保证安全，但 JavaScript 仍然以其跨平台、容易上手等优势大行其道。同时，有些特殊功能（如 AJAX）必须依赖 JavaScript 在客户端进行支持。随着引擎如 V8 和框架如 Node.js 的发展，及其事件驱动及异步 IO 等特性，JavaScript 逐渐被用来编写服务器端程序。且在近几年中，Node.js 的出世，让 JavaScript 也具有了一定的服务器功能，且在某些方面比 PHP 的效果更为显著。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;关于 javascript 的其他具体内容可以参考[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;关于 javascript 的标准规范可以参考[ECMAScript](https://zh.wikipedia.org/wiki/ECMAScript#%E7%89%88%E6%9C%AC)
