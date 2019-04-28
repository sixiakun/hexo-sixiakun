---
title: web前端演进史简述
---
一直想维护自己的博客，但是都未下手，拖延的毛病总是改不了，
古语有云，以史为镜可以知兴替，第一篇就以前端的历史讲起吧，由简入繁

## 本文主要从三个方面说起

1.为什么是javascript在web中使用
2.web前端关键节点
3.前端开发趋势

### 一、为什么是javascript在web中使用

在浏览器刚刚兴起的时候，网速很慢，上网的费用也很贵，但是当时有这样一种场景。如果用户忘记填写“用户名”，就点了“发送”按钮，到服务器再发现这一点就有点太晚了，最好能在用户发出数据之前，就告诉用户“请填写用户名”。这就需要在网页中嵌入小程序，让浏览器检查每一栏是否都填写了。

那一年，正逢 Sun 公司的 Java 语言问世，市场推广活动非常成功。Netscape 公司决定与 Sun 公司合作，浏览器支持嵌入 Java 小程序（后来称为 Java applet）。但是，浏览器脚本语言是否就选用 Java，则存在争论。后来，还是决定不使用 Java，因为网页小程序不需要 Java 这么“重”的语法。但是，同时也决定脚本语言的语法要接近 Java，并且可以支持 Java 程序。这些设想直接排除了使用现存语言，比如 Perl、Python 和 TCL。

1995年5月，Brendan Eich 用了10天时间，设计完成了javascript的第一版

基本语法：借鉴c和java
数据结构：借鉴java，将值分为原始值和对象
函数：借鉴scheme，将函数作为一等公民
原型继承：借鉴self
正则：借鉴perl
字符串和数组处理：借鉴python

为了保持简单，这种脚本语言缺少一些关键的功能，比如块级作用域、模块、子类型（subtyping）等等，但是可以利用现有功能找出解决办法。这种功能的不足，直接导致了后来 JavaScript 的一个显著特点：对于其他语言，你需要学习语言的各种功能，而对于 JavaScript，你常常需要学习各种解决问题的模式。而且由于来源多样，从一开始就注定，JavaScript 的编程风格是函数式编程和面向对象编程的一种混合体。

“the part that is good is not original, and the part that is original is not good” –Samual johnson

### 二、web前端关键节点

1996年11月，网景正式向ECMA（欧洲计算机制造商协会）提交语言标准。1997年6月，ECMA以JavaScript语言为基础制定了ECMAScript标准规[ECMA-262](https://www.ecma-international.org/publications/standards/Ecma-262-arch.htm)。JavaScript成为了ECMAScript最著名的实现之一

标准的出现，是一门语言赖以成长下去的基础，可参考现在的各大厂商小程序。当然，这对某个时期的开发者而言，也未必幸福，如兼容ie。。
和 ECMAScript 有关的不止是262，ECMA 402 则是制定一些基于 ECMAScript 5 或者之后版本的一些国际化 API 标准。ECMA 404 是 JSON 规范。ECMA 414 则规定了哪些规范是和 ECMAScript 有关的。还有一些其他的版本

1998年前后，Outlook Web Access小组写成了允许客户端脚本发送HTTP请求（XMLHTTP）的第一个组件。该组件原属于微软Exchange Server，并且迅速地成为了Internet Explorer 4.0[2]的一部分
1999年，IE 5部署了 XMLHttpRequest 接口，允许 JavaScript 发出 HTTP 请求
2004年4月1日，Google Gmail的发布

gmail让众前端开发者(当时估计就没有几个专职的前端开发人员)认识到，原来前端可以做如此吊的应用，ajax原来可以这样玩。
2004年，Google 公司发布了 Gmail，促成了互联网应用程序（Web Application）这个概念的诞生。由于 Gmail 是在4月1日发布的，很多人起初以为这只是一个玩笑。
也正是2004年之后，国际上开始出现前端开发工程师

2006年8月26日，jquery第一个稳定版本发布

jQuery 为操作网页 DOM 结构提供了非常强大易用的接口，成为了使用最广泛的函数库，并且让 JavaScript 语言的应用难度大大降低，推动了这种语言的流行。

2008年9月2日。chrome发布了第一个版本

chrome的发布伴随着v8的到来，如果不是它，node也就可能不会出现，对前端而言，可能是不可想象的

2007年，Webkit 引擎在 iPhone 手机中得到部署。它最初基于 KDE 项目，2003年苹果公司首先采用，2005年开源。这标志着 JavaScript 语言开始能在手机中使用了，意味着有可能写出在桌面电脑和手机中都能使用的程序。
2009年，PhoneGap 项目诞生，它将 HTML5 和 JavaScript 引入移动设备的应用程序开发，主要针对 iOS 和 Android 平台，使得 JavaScript 可以用于跨平台的应用程序开发。
2009年11月8日，Dahl在欧洲JSConf大会上展示了Node.js项目

按理来说，node是js服务端进军的可能，而下一个则是手机的发展，并没什么联系，其实则不然
一方面是node的出现，让前端脱离后端开发成为可能，而另一方面，则是智能手机的兴起，导致了业内对前后端分离开发的追求
也正是2010年前后，国内开始有了真正意义上的web前端工程师

2009年，AngularJS诞生
2010年10月13号，backbone发布了第一个版本
2013年5月React在JSConf US开源
2014年2月vue发布

### 三、前端开发趋势

建议大家去看阿里技术出版的[《不止代码》](https://102.alibaba.com/downloadFile.do?file=1530517140411/Codelife.pdf)一书，当然这本书也是一种趋势的思考，并不能代表全部，真正的路在脚下，送大家一句话
“一个人几乎可以在任何他怀有无限热忱的事情上成功” -查尔斯·史考伯