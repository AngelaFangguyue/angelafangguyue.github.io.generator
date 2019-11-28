---
title: "Js基本语法"
date: 2019-11-28T15:30:49+08:00
draft: false
categories:
  - js
tags:
  - js
featured_image:
---

本篇文章是关于 js 的概览，主要内容：

[1:表达式与语句](#a1)  
[2:注意事项](#a2)  
[3:标识符](#a3)  
[4:注释](#a4)  
[5:区块 block](#a5)  
[6:条件语句](#a6)  
[7:循环语句](#a7)  
[8:break&&continue](#a8)  
[9:label](#a9)

---

<b> js 之父布兰登如此评价 js：它的原创之处并不优秀，优秀之处并非原创。哈哈哈哈，好打脸，那为什么它还被学习，因为它有价值，能产生价值，带来价值。

现在距离 0202 年还不到 35 天，我们现在学习的就是 ES6 了。一句话，取其精华，弃其糟粕。</b>

---

### 1:表达式和语句{#a1}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;先要区分一下表达式和语句。语句是为了完成某种任务而进行的操作，下面就是一行赋值语句：

    var a = 1 + 3;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这条语句先用 var 命令，声明了一个变量 a，然后将表达式 1+3 的运算结果赋值给变量 a。1+3 叫做表达式，指一个为了得到返回值的计算式。语句和表达式区别在于：前者为了进行某种操作，一般情况下不需要返回值，后者则是为了得到返回值，一定会返回一个值。详细的可以参考[这篇文章](https://wangdoc.com/javascript/basic/grammar.html#%E8%AF%AD%E5%8F%A5)。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;表达式：1+2 表达式的值是 3；add(1+2)表达式的值为函数的返回值 3；console.log 表达式的值是函数本身；console.log('3')表达式的值是 undefined（这里解释一下，console.log('3')表达式的值是函数 log 的返回值，而函数 log 返回值是 undefined。但该函数功能是会执行输出打印 3。）。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;二者区别：一般来说，表达式都是有值的，语句可有可没有。语句通常会改变环境的，比如说声明，赋值。这两句话并不是绝对的。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**这里还要提一下值和返回值的概念。注意只有函数才有返回值。**

---

### 2:注意事项{#a1}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;js 中对大小写是敏感的；对空格和回车一般没关系，只有一个特殊情况：那就是 return 后不能跟回车。return 后若跟回车，会默认 return 一个 undefined。

---

### 3:标识符{#a3}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;标识符标识符规则：可以以 unicode 字母，中文，\_或者\$开头，但是不能以数字打头。后面的字符，除了前面提到的，还可以有数字。变量名也是标识符。

---

### 4:注释{#a4}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注释并不是越多越好。好的注释：踩坑注释；为什么代码会写得这么奇怪，遇到什么 bug

---

### 5:区块 block{#a5}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;区块 block：把代码包在一起{let a = 1 let b = 2}，常与 if/for/while 合用

---

### 6:条件语句{#a6}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**一：if 条件语句**

    if（表达式）{
    语句 1
    }else{
    语句 2
    }

若中括号内只有一条语句，中括号可以省略不写，但不推荐这样做。另若 else 中什么也不做，else{}也可以省略。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**这里要注意非常态即变态的情况：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1》表达式变态，例如表达式是一个赋值语句，如

    if(a = 1){
    console.log('a 的值是 1')；
    }else{
    console.log('a 的值不是 1')；
    }

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2》if 语句可以嵌套 if-else，

    if(表达式 1){
          if(表达式 2){语句 1}
          else{语句 2}
               }
    else{语句 3}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果两个 else 中什么都不做，那么就可以简写为

    if(表达式 1){
      if(表达式 2){语句 1}
               }

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;且若语句只有一句的话，中括号省略掉，就变为

    if(表达式 1)
     if(表达式 2)
    语句

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（这种情况，要注意缩进，有的时候面试题会挖坑给你跳！！！）。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3》else 语句中也可以嵌套 if-else，

    if(表达式 1){语句 1}
    else{
            if(表达式 2){语句 2}
            else{语句 3}
        }，

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;我们所知道的 if-else if-else 就是这种情况。if(){}else if(){}else{}。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;程序员戒律：使用最没有歧义的写法,那最推荐的写法》

    if（表达式）{语句}
    else if（表达式）{语句}
    else{语句}

次推荐使用的写法》

    function fn（）{
     if（表达式）{return 表达式}
     if（表达式）{return 表达式}
     return 表达式
    }

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**二：switch 语句》**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注意 break，如果忘记了写 break，执行到符合情况的语句后，会继续执行下面的语句，不管其 case 是否满足条件，直至遇到 break 才会返回。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**三：问号冒号表达式》**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;表达式 1？表达式 2：表达式 3
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;若表达式 1 成立，取表达式 2 的值，否则取表达式 3 的值

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**四：逻辑判断符&& ||》**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A&&B 若 A 为真，表达式的值就为 B;若 A 为假，表达式的值就为 A。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A||B 若 A 为真，表达式的值就为 A;若 A 为假，表达式的值就为 B。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A&&B&&C&&D 表达式的值取第一个假值或者 D。注意并不会取 true 或者 false

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A||B||C||D 表达式的值取第一个真值或者 D。注意并不会取 true 或者 false

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;console&&console.log&&console.log('hi');

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;总结：条件语句》if...else.../ switch/ A?B:C/ A&&B/ fn&&fn()/ A||B/ A=A||B

---

### 7:循环语句{#a7}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
js 语法之循环 while&for 语法糖

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;while(表达式){语句}》判断表达式的真假；当表达式为真，执行语句，执行完再判断表达式的真假；当表达式为假，执行后面的语句

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for 语法糖》for(语句 1；表达式 2；语句 3){循环体}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一》执行语句 1

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;二》判断表达式 2

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;三》若为真，执行循环体，然后执行语句 3，再循环执行第二步和第三步

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;四》若为假，直接退出循环，执行 for 循环后面的语句。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注意 for 语法糖中的非常态情况。

---

### 8:break&&continue{#a8}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break 是退出当前循环，若为多层循环嵌套，则退出最近的那层循环。continue 是退出本次循环，继续下次循环

---

### 9:label{#a9}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;label 语法 {a:1} 注意：现在 chrome 浏览器对 label 进行了优化，需要写成这样{a:1;}后面要加分号。否则会将其视为对象。但在 Firefox 中，还和以前一样，就是声明了一个 a 标签，值为 1。
