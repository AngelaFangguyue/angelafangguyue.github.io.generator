---
title: "HTML常用标签"
date: 2019-10-16T11:32:08+08:00
draft: true
---

# HTML 常用标签

## [a 标签](#ida)

## [img 标签](#ida1)

## [table 标签](#ida2)

## [form 标签](#ida3)

## [碎碎念其他标签](#ida4)

### <span id="ida" style="font-size:30px;">a 标签</span>

    <a href="//baidu.com" target="_blank">百度</a>

<strong style="font-size:22px;">作用:</strong>  
&nbsp;&nbsp;跳转到外部页面;跳转到内部锚点;跳转到邮箱或电话。

<strong style="font-size:22px;">常用属性:</strong>  
&nbsp;&nbsp;1>href 表示超链接的地址，常用的形式:

---

&nbsp;&nbsp;&nbsp;&nbsp;<strong><em>形式一之网址:</strong></em>&nbsp;(http://baidu.com) &nbsp;&nbsp;(https://baidu.com) &nbsp;&nbsp;(//baidu.com)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[推荐用最后一种形式]

---

&nbsp;&nbsp;&nbsp;&nbsp;<strong><em>形式二之路径:</strong></em>&nbsp;(/a/b/c.html) &nbsp;&nbsp;(a/b/c.html)&nbsp;&nbsp;(index.html) &nbsp;&nbsp;(./index.html)

[这里要注意：作为一名前端人员，在本地预览自己的网页时，不要使用 file 协议去打开预览网页；而是要用 http-server 或者 parcel 去开启服务，使用 http 协议去打开网页。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;因为当使用 file 协议打开的时候，使用绝对路径如/a/b/c 的形式，这时候根目录对应的是当前的硬盘，就会去 “对应盘符/a/b/c”下找；但若使用 http 协议打开，根目录就是打开服务所在的目录。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如下是在我本地电脑所做的实验。在 E 盘下新建一个目录 20191015-10，里面新建了 a-href.html 文件，还有 a 目录。同时 a 目录下，又新建了一个 b 目录，b 目录下新建了一个 c.html 文件。
![](/images/10-html/1.PNG)
![](/images/10-html/2.PNG)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下图是我直接在资源管理器中，双击打开 a-herf.html 文件，可看到地址栏中的显示的路径 E:/20191015-10/a-href.html。将红线 1 标注处的地址栏中地址复制在控制台，会出现红线二标注的 file///E:20191015-10/a-href.html 字样，注意这里的 file 指的就是使用的 file 协议。
![](/images/10-html/3.PNG)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当我点击下图一矩形中为"超链接:/a/b/c.html"的字样时,显示无法找到，此时可以看图二地址栏中的地址是 E://a/b/c.html。可以看图二的源代码，字样为"超链接:/a/b/c.html"。
![](/images/10-html/4.PNG)
上图一
![](/images/10-html/5.PNG)
上图二
![](/images/10-html/6.PNG)
上图三

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;而如果是用 http 协议，即在 E://20191015-10 下打开 http-server 服务之后，如图一，红色矩形标注的两个网址，选择其中任意一个，按住 ctrl 键，右击在浏览器中打开，并且在地址栏中输入 a-href.html，就可以预览，如下图二。此时点击链接地址为/a/b/c.html，即可正确找到该页面并显示出来，如图三，可以看到地址栏中是 http://192.168.124.4:8080/a/b/c.html 只不过省略了 http。
![](/images/10-html/7.PNG)
上图一
![](/images/10-html/8.PNG)
上图二
![](/images/10-html/9.PNG)
上图三

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于绝对路径：a/b/c.html 的写法，以及 index.html 和./index.html 的写法，不管是用 file 协议还是 http 协议打开，都是可以正确找到 c.html 的。这里就不详细截图介绍了。

]

---

&nbsp;&nbsp;&nbsp;&nbsp;<strong><em>形式三之伪协议:</strong></em>&nbsp;(JavaScript:;)&nbsp;&nbsp;锚点(#id 名)&nbsp;&nbsp;邮箱:(mailto:邮箱地址)&nbsp;&nbsp;电话:(tel:电话)

[一般使用伪协议，就是因为若伪协议中，什么也不写，那么点击之后，什么也不做。但其他的，无论在 href 后面写什么，都会有操作！

    <a href="javascript:;">点击该链接，什么反应都没有</a>
    <a href="">点击该链接，刷新页面</a>
    <a href="#">点击该链接，若此时页面在下方，会回到页面起始处</a>
    <a href="#id名">点击该链接，会跳到id对应的元素处</a><!--锚点-->

]

&nbsp;&nbsp;2>target 表示超链接打开的窗口

&nbsp;&nbsp;&nbsp;&nbsp;<strong><em>内置名字:</strong></em>(\_self)
&nbsp;&nbsp;(\_blank)
&nbsp;&nbsp;(\_top)
&nbsp;&nbsp;(\_parent)

[默认的 target 属性是_self，即在当前窗口打开。注意 top 和 parent 的区别，前者是顶部窗口，后者是父级窗口。]

&nbsp;&nbsp;&nbsp;&nbsp;<strong><em>程序员命名:</strong></em>&nbsp;&nbsp;(iframe 的名字)&nbsp;&nbsp;(window 的名字)

[若在一个文件中，有如下代码，那么点击百度，就会在一个新的名为 AA 的窗口打开百度。此时若去原页面点击 QQ，则会在 AA 的窗口打开腾讯的页面。

    <a href="//baidu.com" target="AA">百度</a>
    <a href="//qq.com" target="AA">QQ</a>

]

&nbsp;&nbsp;3>downlod(目前很少使用，只在一些老版浏览器上使用，下载功能。不做过多介绍。)

&nbsp;&nbsp;4>rel=noopener

### <span id="ida1">img 标签</span>

    <img src="A" alt="B" title="C"/>

A 是要请求的图片的路径，B 是当图片无法正常加载时，用于代替图片的说明文字，C 是对图片的解释，当鼠标在图片上时，会出来 title 中设置的文字。

<strong style="font-size:22px;">作用:</strong>  
&nbsp;&nbsp;发出 get 请求，展示一张图片。

<strong style="font-size:22px;">常用属性:</strong>  
&nbsp;&nbsp;1>src 图片的地址  
&nbsp;&nbsp;2>alt  
&nbsp;&nbsp;3>width  
&nbsp;&nbsp;4>height 如果只设置 img 的 width 或者 height 的任一个，图片会按比例，自动调整大小。但若同时设置，图片可能会变形。温馨提示：作为前端从业人员，图片一定不要变形！！！  
&nbsp;&nbsp;5>title  
&nbsp;&nbsp;6>max-width:100%;给图片加上该属性，图片就是响应式，能适配各种终端。

### <span id="ida2">table 标签</span>

    <table border-collapse="collapse">
        <thead>
            <tr>
                <th></th>
                <th>语文</th>
                <th>英语</th>
                <th>数学</th>
            </tr>
        </thead>
        <tbody>
           <tr>
                <th>小明</th>
                <td>80</td>
                <td>90</td>
                <td>70</td>
            </tr>
           <tr>
                <th>小红</th>
                <td>70</td>
                <td>80</td>
                <td>90</td>
            </tr>
           <tr>
                <th>小新</th>
                <td>90</td>
                <td>70</td>
                <td>80</td>
            </tr>
            <tr>
                <th>小刘</th>
                <td>80</td>
                <td>80</td>
                <td>80</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <th>总分</th>
                <td>320</td>
                <td>320</td>
                <td>320</td>
            </tr>
        </tfoot>
    </table>

table 可以设置多个表头。如上代码》横表头是科目：语文英语数学；竖表头是学生：小红小明小兰等。若 thead，tbody，tfoot 顺序写错，浏览器在解析的时候，会自动将其位置调整好。若没有写 thead，tbody，tfoot，那么浏览器在解析的时候，会自动加上 tbody 标签，将其放在 tbody 标签中。同样 td 标签会自动将其加到 tr 标签中。th 中的文本默认会居中显示，td 中的文本默认是靠左显示。可以给 table 标签设置 border 属性，但是 thead tbody tfoot tr 就不能设置，可以给 th td 设置 border。

<strong style="font-size:22px;">常用属性:</strong>  
&nbsp;&nbsp;事件：onload onerror  
&nbsp;&nbsp;1>table-layout：默认值是 auto。如果 table 没有设置宽度，那么 table-layout 设置为 fixed 和 auto 效果是一样的。如果设置了 table 宽度 width，即 table 宽度固定，table-layout 的值若为 auto，那么浏览器会会自动根据文本内容灵活调整列宽；值若为 fixed 的话，每列的宽度是平分固定的。  
&nbsp;&nbsp;2>border-spacing：默认值是 2px。  
&nbsp;&nbsp;3>border-collapse：默认是 seperate。若将值设为 collapse，则 border-spacing 属性将不起作用。  
具体 border-collapse 和 border-spacing 的用法以及区别如下图：
![](/images/10-html/10.PNG)

### <span id="ida3">form 标签</span>

    <form action="XXX" method="get/post" targer="AAA" name="表单名称" autocomplete="on">
      <p>--input: type="text" --</p>
      <input type="text" name="userName" value="angela"/>
      <p>--input: type="password" --</p>
      <input type="password" name="pasName" />
      <p>--input: type="radio" 以及label标签的使用--</p>
      <input id="nan" type="radio" name="gender" value="male"/>
      <label for="nan">男</label>
      <label><input type="radio" name="gender" value="female"/>女<label>
      <p>--input: type="checkbox" 以及label标签的使用--</p>
      <label>喜欢唱歌吗<input  type="checkbox" name="hobby" value="sing"/><label>
      <label>喜欢看书吗<input  type="checkbox" name="hobby" value="read"/><label>
      <label>喜欢运动吗<input  type="checkbox" name="hobby" value="read"/><label>
      <p>--input: type="submit" --</p>
      <input type="submit" name="subName" value="subValue"/>
      <p>--select和option标签 --</p>
      <select>
        <option value="" checked>--请选择--</option>
        <option value="1">星座一</option>
        <option value="2">星座二</option>
        <option value="3">星座三</option>
        <option value="4">星座四</option>
        <option value="5">星座五</option>
        <option value="6">星座六</option>
        <option value="7">星座天</option>
      </select>
      <p>--textarea标签 --</p>
      <textarea>自我描述及评价：</textarea>
      <p>--input: type="file" --</p>
      上传单个文件<input type="file" />
      上传多个文件<input type="file" multiplied/>
      <p>--input: type="image" --</p>
      上传图片<input type="image" name="image" src="/files/2917/fxlogo.png" width="50">
      <p>--input: type="color" --</p>
      <input type="color" />
      <p>--input: type="hidden" --</p>
      <input type="hidden"/>
      <p>--input: type="reset" --</p>
      <input type="reset" />
      <p>--input: type="button" --</p>
      <input type="button" />
      <p>--button --</p>
      <button type="submit">button:type="submit"</button>
      <button type="button">button:type="button"</button>
    </form>

<strong style="font-size:22px;">作用:</strong>
&nbsp;&nbsp;发出 get 或者 post 请求，刷新页面。

<strong style="font-size:22px;">常用属性:</strong>  
&nbsp;&nbsp;1>action 一个处理此表单信息的程序所在的 URL。此值可以被 button 元素或者 input 元素中的 formaction 属性覆盖。  
&nbsp;&nbsp;2>method 提交的方式  
&nbsp;&nbsp;3>name 表单名称  
&nbsp;&nbsp;4>target，可取的值有\_blank,\_self,\_parent,\_top,framename(frame 的名称)。提交的时候是重新打开窗口还是在自身窗口打开等。
&nbsp;&nbsp;5>autocomplete，当值为 on 的时候，表示表单启用自动完成功能；off 则是关闭。  
&nbsp;&nbsp;事件：onsubmit

form 表单，里面必须要有提交的按钮，才能实现提交表单的功能。
若 button 元素是 form 表单的子元素，且 type 属性未设置，那么除了 IE 浏览器， 其他浏览器都会将该 button 按钮视为 form 表单的提交按钮。
所以一般对于 button 元素，必须设置其 type 属性。button 元素的 type 属性有 button，submit，reset。

一提交按钮设置:  
《1 使用 input 元素，设置一个 type="submit"类型的按钮  
《2 使用 button 元素，设置一个 type="submit"类型的按钮

二 input 标签创建的按钮和 button 元素的区别:  
input type="button"是 input 元素的特殊版本，被用来创建一个没有默认值的可点击按钮。即浏览器生成一个控件没有默认值的可点击按钮。该按钮上可有任何标签。该控件在不同的浏览器上，可能有不同的样式。它已经在 HTML5 被 button 元素取代。  
button 元素里面可以添加其他元素，如可在 button 里添加 span 元素。而元素 input type="button"则不可以，可以通过给 input 元素添加 value 属性，值用文本显示，如 input type="button" value="这是使用 input 创建的一个普通按钮"，value 的值会显示在按钮上。

三 input 的 type 为 text：单行字段；换行会将自动从输入的值中移除。

四 input 的 type 为 password：一个值被遮盖的单行文本字段。使用 maxlength 指定可以输入的值的最大长度 。

五 input 的 type 为 submit：用于提交表单的按钮；reset：用于将表单所内容设置为缺省值的按钮；button：无缺省行为按钮。

六 input 的 type 为 radio：单选按钮。必须使用 value 属性定义此控件被提交时的值。使用 checked 必须指示控件是否缺省被选择。在同一个”单选按钮组“中，所有单选按钮的 name 属性使用同一个值； 一个单选按钮组中是，同一时间只有一个单选按钮可以被选择。

七 input 的 type 为 checkbox：复选框。必须使用 value 属性定义此控件被提交时的值。使用 checked 属性指示控件是否被选择。也可以使用 indeterminate 指示复选框在一种不确定状态（大多数平台上，显示为一条穿过复选框的水平线）。

八 input 的 type 为 image：图片提交按钮。必须使用 src 属性定义图片的来源及使用 alt 定义替代文本。还可以使用 height 和 width 属性以像素为单位定义图片的大小。

九 input 的 type 为 file：此控件可以让用户选择文件。使用 accept 属性可以定义控件可以选择的文件类型。另外 multiplied 属性可以设置上传多个文件。

十 input 的 type 为 color：用于指定颜色的控件。

十一 input 的 type 为 hidden：不显示在页面上的控件，但它的值会被提交到服务器。

十 textarea 标签：一个多行纯文本编辑控件，会根据输入的内容自动调整大小，但如果给其设置了 col 以及 row 之后，大小就会固定。

十一 selection 以及 option 标签：selection》表示一个控件，提供一个选项菜单；菜单内的选项为 option，可以由 optgroup 元素分组。选项可以被用户预先选择。

十二 label 标签：表示用户界面中某个元素的说明。在 form 表单中，将一个 label 和一个 input 元素放在一起会有以下几点好处：

1：标签文本不仅与其相应的文本输入在视觉上相关联; 它也以编程方式与它相关联。 这意味着，当用户点击到表单输入时，屏幕阅读器可以读出标签，使在使用辅助技术的用户更容易理解应输入哪些数据.

2：你可以单击关联的标签来聚焦或者激活 input，以及 input 本身。这种增加的命中区域为激活 input 提供了方便，包括那些使用触摸屏设备的。

想要将一个 label 和一个 input 元素匹配在一起，你需要给 input 一个 id 属性。而 label 需要一个 for 属性，其值和 input 的 id 一样。  
另外，你也可以将 input 直接放在 label 里，这种情况就不需要 for 和 id 属性了，因为这时关联是隐含的：

    <!-- 下面是关联的第一种方法 -->
    <label>Do you like peas?
    </label>
    <input type="checkbox" name="peas">
    <!-- 下面是关联的第二种方法 -->
    <label>Do you like peas?
      <input type="checkbox" name="peas">
    </label>

其他使用事项：

1：标签标记的表单控件称为标签元素的标签控件。一个 input 可以与多个标签相关联。  
2：标签本身并不与表单直接相关。它们只通过与它们相关联的控件间接地与表单相关联。  
3：当点击或者触碰（tap）一个与表单控件相关联的 label 时，关联的控件的 click 事件也会触发。

### <span id="ida4">碎碎念其他标签 </span>

iframe 标签，内嵌窗口。目前很少使用，只有一些老版的浏览器还在使用。现在都是使用 ajax 的方式。像 vadio audio canvas svg 这些标签兼容性要查看文档。

一些注意事项：

&nbsp;&nbsp;1》input 有 onclick，onfocus，onblur 事件。一般不监听 input 的 click 事件，而是 focus 事件。

&nbsp;&nbsp;2》input 验证器，这是在 HTML5 中新增功能。给 input 元素设置 required 属性。

&nbsp;&nbsp;3》form 里的 input 要设置 name 属性。

&nbsp;&nbsp;4》form 要有一个 type="submit"的 button 或者 input 元素，这样才会有 submit 提交事件。
