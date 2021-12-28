# JavaScritp 简介

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211217205206.png)



## 1. 什么是 JavaScript?

*JavaScript* 最初被创建的目的是使网页更生动

这种编程语言写出来的程序被称为 **脚本**。它们可以被直接写在网页的 HTML 中，在页面加载的时候自动执行

脚本被以纯文本的形式提供和执行。它们不需要特殊的准备或编译即可运行

JavaScript 不仅可以在浏览器中执行，也可以在服务端执行，甚至可以在任意搭载了 JavaScript 引擎的设备中执行



## 2. JavaScript 的历史

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211218162932.png)

​        JavaScript 最初被称为 LiveScript，由 Netscape（Netscape Communications Corporation，网景通信公司）公司的布兰登·艾奇（Brendan Eich）在 1995 

年开发。在 Netscape 与 Sun（一家互联网公司，全称为“Sun Microsystems”，现已被甲骨文公司收购）合作之后将其更名为了 JavaScript

​        之所以将 LiveScript 更名为 JavaScript，是因为 JavaScript 是受 Java 的启发而设计的，因此在语法上它们有很多相似之处 ，JavaScript 中的许多命名规范也

都借鉴自 Java，还有一个原因就是为了营销，蹭 Java 的热度

​        同一时期，微软和 Nombas（一家名为 Nombas 的公司）也分别开发了 JScript 和 ScriptEase 两种脚本语言，与 JavaScript 形成了三足鼎立之势。它们之间

没有统一的标准，不能互用。为了解决这一问题，1997 年，在 ECMA（欧洲计算机制造商协会）的协调下，Netscape、Sun、微软、Borland（一家软件公司）

组成了工作组，并以 JavaScript 为基础制定了 ECMA-262 标准（ECMAScript）

​        第二年，ISO/IEC（国际标准化组织及国际电工委员会）也采用了 ECMAScript 作为标准（即 ISO/IEC-16262）



## 3. Javascript 的组成

ECMAScript（简称“ES”）是根据 ECMA-262 标准实现的通用脚本语言，ECMA-262 标准主要规定了这门语言的语法、类型、语句、关键字、保留字、操作符、对

象等几个部分，目前 ECMAScript 的最新版是 ECMAScript6（简称“ES6”）

至于 JavaScript，有时人们会将 JavaScript 与 ECMAScript 看作是相同的，其实不然，JavaScript 中所包含的内容远比 ECMA-262 中规定的多得多

完整的 JavaScript 是由以下三个部分组成：

- 核心（ECMAScript）：提供语言的语法和基本对象
- 文档对象模型（DOM）：提供处理网页内容的方法和接口
- 浏览器对象模型（BOM）：提供与浏览器进行交互的方法和接口



## 4. Javascript的特点

**解释型脚本语言：**

JavaScript 是一种解释型脚本语言，与 C、[C++](http://c.biancheng.net/cplus/) 等语言需要先编译再运行不同，使用 JavaScript 编写的代码不需要编译，可以直接运行

**面向对象：**

JavaScript 是一种面向对象语言，使用 JavaScript 不仅可以创建对象，也能操作使用已有的对象

**弱类型：**

JavaScript 是一种弱类型的编程语言，对使用的数据类型没有严格的要求，例如您可以将一个变量初始化为任意类型，也可以随时改变这个变量的类型

**动态性：**

JavaScript 是一种采用事件驱动的脚本语言，它不需要借助 Web 服务器就可以对用户的输入做出响应，例如我们在访问一个网页时，通过鼠标在网

页中进行点击或滚动窗口时，通过 JavaScript 可以直接对这些事件做出响应

**跨平台：**

JavaScript 不依赖操作系统，在浏览器中就可以运行。因此一个 JavaScript 脚本在编写完成后可以在任意系统上运行，只需要系统上的浏览器支持 JavaScript 即可



## 5. 浏览器如何执行 Javascript 

作为一种脚本语言，JavaScript 代码不能独立运行，通常情况下我们需要借助浏览器来运行 JavaScript 代码，所有 Web 浏览器都支持 JavaScript。

除了可以在浏览器中执行外，也可以在服务端或者搭载了 JavaScript 引擎的设备中执行 JavaScript 代码，浏览器之所以能够运行 JavaScript 代码就是因为浏览器中

都嵌入了 JavaScript 引擎，常见的 JavaScript 引擎有：

|    浏览器    |                            js引擎                            |
| :----------: | :----------------------------------------------------------: |
| 谷歌 chrome  |                              V8                              |
|      IE      |                           Trident                            |
| 火狐 Firefox | SpiderMonkey（1.0-3.0）/ TraceMonkey（3.5-3.6）/ JaegerMonkey（4.0-） |
|    Safari    |                            Nitro                             |
|  欧朋 Opera  | Linear A（4.0-6.1）/ Linear B（7.0-9.2）/ Futhark（9.5-10.2）/ Carakan（10.5-） |
|     Edge     |                            Chakra                            |

JavaScript 引擎工作步骤：

1. 引擎（如果是浏览器，则引擎被嵌入在其中）读取（“解析”）脚本
2. 然后，引擎将脚本转化（“编译”）为机器语言
3. 然后，机器代码快速地执行

引擎会对流程中的每个阶段都进行优化。它甚至可以在编译的脚本运行时监视它，分析流经该脚本的数据，并根据获得的信息进一步优化机器代码