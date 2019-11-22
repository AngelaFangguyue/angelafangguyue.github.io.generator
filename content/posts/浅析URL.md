---
title: "浅析URL"
date: 2019-11-21T17:08:56+08:00
draft: false
categories:
  - url
tags:
  - url
featured_image:
---

本篇文章大概介绍一下 URL,主要内容有：  
[1:URL ](#a1)  
[2:DNS 以及 nslookup 命令](#a2)  
[3:IP 以及 ping 命令](#a3)  
[4:域名 ](#a4)

---

**李爵士发明了 www，万维网。WWW=URL+HTTP+HTML。**

在搜索资料的时候，发现 CSDN 上这篇博客写的很好，可以看一下[CSDN](https://blog.csdn.net/larry_zeng1/article/details/79520534)。

### 1:URL {#a1}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;统一资源定位符（英语：Uniform Resource Locator，缩写：URL；或称统一资源定位器、定位地址、URL 地址，俗称网页地址或简称网址）是因特网上标准的资源的地址（Address），如同在网络上的门牌。统一资源定位符的标准格式如下：
[协议类型]://[服务器地址]:[端口号]/[资源层级 UNIX 文件路径][文件名]?[查詢]#[片段 ID]

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;统一资源定位符的完整格式如下：
[协议类型]://[访问资源需要的凭证信息]@[服务器地址]:[端口号]/[资源层级 UNIX 文件路径][文件名]?[查詢]#[片段 ID]

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其中[访问凭证信息]、[端口号]、[查询]、[片段 ID]都属于选填项。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-size:20px;font-weight:bold;">用自己的话总结就是</span>**通过 URL，你告诉了浏览器它所需要发现并连接的网络服务器地址，以及获取服务器上的页面路径。**（不过在浏览器发送 HTTP 请求之前，它首先要与目标网络服务器建立 TCP 连接。然后，浏览器再通过 TCP 连接发送 HTTP 请求至服务器，并等待服务器返回 HTTP 响应。当浏览器收到响应的时候，就会在页面上显示响应的内容。）**URL 包含协议，域名或者 IP，端口，路径，查询参数，锚点，其中查询参数和锚点也可以没有。协议是用来规定请求以及响应的格式是什么，常用的有 http 和 https 这两种协议。域名或者 IP 就是要请求的网址。端口是请求的服务类型，不同的服务对应不同的端口，而常用的就是 80 或者 443 端口。不同的路径请求不同的页面。加上查询参数，可以显示不同的查询内容。锚点是具体在一个页面上，定位到一个页面的具体位置。**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;例如：1》https://developer.mozilla.org/zh-CN/docs/Web/HTML#高级主题（https://developer.mozilla.org/zh-CN/docs/Web/HTML#%E9%AB%98%E7%BA%A7%E4%B8%BB%E9%A2%98）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2》https://www.baidu.com/baidu?wd=hugo&tn=monline_4_dg

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1 和 2 中：https 就是协议，developer.mozilla.org 和 www.baidu.com 是域名，
zh-CN/docs/Web/HTML 和/baidu 是路径，?wd=hugo&tn=monline_4_dg 是查询参数，#高级主题是锚点。

---

### 2:DNS 以及 nslookup 命令 {#a2}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;域名系统（英语：Domain Name System，缩写：DNS）是互联网的一项服务。它作为将域名和 IP 地址相互映射的一个分布式数据库，能够使人更方便地访问互联网。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一个域名可以对应多个 IP,这样可以实现负载均衡。而一个 IP 也可以绑定多个域名，这叫做共享主机。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;命令 nslookup 域名，可以查找到这个域名对应的 IP。
如 nslookkup baidu.com。

---

### 3:IP 以及 ping 命令 {#a3}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IP 可以指 IP 地址，一串数字；也可以指 IP 协议，它是互联网最底层的协议之一。一般我们常说的 IP 指的就是 IP 地址。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>网际协议（英语：Internet Protocol，缩写：IP；也称互联网协议）是用于分组交换数据网络的一种协议。IP 是在 TCP/IP 协议族中网络层的主要协议，任务仅仅是根据源主机和目的主机的地址来传送数据。为此目的，IP 定义了寻址方法和数据报的封装结构。</b>第一个架构的主要版本为 IPv4，目前仍然是广泛使用的互联网协议，尽管世界各地正在积极部署 IPv6。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>互联网协议地址（英语：Internet Protocol Address，又译为网际协议地址），缩写为 IP 地址（英语：IP Address），是分配给网络上使用网际协议（英语：Internet Protocol, IP）的设备的数字标签。</b>常见的 IP 地址分为 IPv4 与 IPv6 两大类，但是也有其他不常用的小分类。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
ping 是一种电脑网络工具，用来测试数据包能否透过 IP 协议到达特定主机。我们会在 windows 的命令行下输入 ping baidu.com,测试电脑是否能连接访问百度。如下图![](/images/task19_url/ping.PNG)

---

### 4:域名 {#a4}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**网域名称（英语：Domain Name，简称：Domain），简称域名、网域，是由一串用点分隔的字符组成的互联网上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位。域名可以说是一个 IP 地址的代称，目的是为了便于记忆后者。**例如，wikipedia.org 是一个域名，和 IP 地址 208.80.152.2 相对应。人们可以直接访问 wikipedia.org 来代替 IP 地址，然后域名系统（DNS）就会将它转化成便于机器识别的 IP 地址。这样，人们只需要记忆 wikipedia.org 这一串带有特殊含义的字符，而不需要记忆没有含义的数字。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
**域名的核心**是域名系统（英语：Domain Name System，缩写：DNS），域名系统中的任何名称都是域名。在域名系统的层次结构中，各种域名都隶属于域名系统根域的下级。域名的第一级是顶级域，它包括通用顶级域，例如.com、.net 和.org；以及国家和地区顶级域，例如.us、.cn 和.tk。顶级域名下一层是二级域名，一级一级地往下。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**域名的目的和特性：**IP 地址是因特网主机的作为路由寻址用的数字型标识，不容易记忆，因而产生了域名这一种字符型标识，它比 IP 地址更容易记忆。这也是域名的一个重要功能——为数字化的互联网资源提供易于记忆的名称。另外，域名具有唯一性，在资源更改 IP 地址时，只需要进行新 IP 地址与恒定域名的转换，即可实现将资源移动到网络地址拓扑中的不同物理位置。基于以上两个特性，域名还用于建立个体的唯一标识。任何组织和个人在提供因特网资源时，都可以选择与其名称对应的域名，让其他人轻松访问这些资源

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
**域名分类：**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
1：顶级域名(Top-level domains，缩写：TLD）是域名中最高的一级，每个域名都以顶级域名结尾。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;域名刚被设计出来时，顶级域名主要分成两类：国家及地区双字代码顶级域（国家和地区顶级域）和通用顶级域。前者基于 ISO-3166 规定的国家/地区双字缩写代码；后者代表了一组名称和多个组织，包括.gov（政府，现被用于美国政府的网站），.edu（教育机构，现被用于美国各类学校的网站），.com（商业，现在成为全球注册量最大、最通用的域名），.mil（军事，现被用于美国国防部及其附属机构的网站），.org（非营利组织），.net（网络，当时被定位为网络基础服务提供商）和.int（国际组织）等。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2：子域名,将顶级域名进一步细分。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;域名层次结构中，顶级域名下面是二级域名，它位于顶级域名的左侧。例如，在 zh.wikipedia.org 中，wikipedia 是二级域名。w3.org 中，w3 也是二级域名，与前例中的 wikipedia 属于一个层面。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;二级域名下面是三级域名，它位于二级域名的左侧。例如，在 zh.wikipedia.org 中，zh 是三级域名；zh-classical.wikipedia.org（文言文维基大典的域名）中，zh-classical 也是三级域名，与前例中的 zh 属于一个层面。从右侧到左侧，隔一个点依次下降一层。
通常情况下，人们基于公司、产品或服务的名称来创建二级域名或更低级别的域名，以方便其他人识别和记忆。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
github 有自己的域名 github.io，这是个二级域名。我们在使用 github 的时候，它会给我们一个[用户名].github.io 的域名，这是个三级域名，我们可以使用。这两个是不同的域名，有相同的一级域名也就是顶级域名.io。在这里要注意一下，有的网址前面是不要加 www,加了之后，反而会找不到。
