# CSS 简介

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211009234533.png)



## 1. 什么是 CSS ?

CSS（层叠样式表）可以用于给文档添加样式 —— 比如改变标题和链接的颜色及大小。它也可用于创建布局 —— 比如将一个单列文本变成包含主要内容区域和存放相关信息的侧边栏区域的布局。它甚至还可以用来做一些特效，比如动画。

- CSS 指层叠样式表 (**C**ascading **S**tyle **S**heets)
- 样式定义**如何显示** HTML 元素
- 样式通常存储在**样式表**中
- 把样式添加到 HTML 4.0 中，是为了**解决内容与表现分离的问题**
- **外部样式表**可以极大提高工作效率
- 外部样式表通常存储在 **CSS 文件**中
- 多个样式定义可**层叠**为一个



## 2. CSS 语法

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211011174428.png)

* 选择器通常是您需要改变样式的 HTML 元素。
* 每条声明由一个属性和一个值组成，如果一个属性有多个值的话，那么多个值用空格隔开。
* 属性（property）是您希望设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。
* CSS声明总是以分号`;`结束，声明总以大括号`{}`括起来。

```css
h1 {color: red; font-size: 12px;}
选择器 { 属性名: 属性值; 属性名: 属性值;}
```



## 3. CSS 注释

注释是用来解释你的代码，并且可以随意编辑它，浏览器会忽略它

CSS注释以 **/\*** 开始, 以 ***/** 结束



单行注释：

注释内容第一个字符和最后一个字符都是一个空格字符，单独占一行，行与行之间相隔一行

```css
/* 单行注释 */
.example {
    color: red;
}
```



模块注释：

注释内容第一个字符和最后一个字符都是一个空格字符，`/*` 与 模块信息描述占一行，多个横线分隔符`-`与`*/`占一行，行与行之间相隔两行

```css
/* Module A
---------------------------------------- */
.example-a {
    color: red;
}

/* Module B
---------------------------------------- */
.example-b {
    color: blue;
}
```



多行注释：

注释内容第一个字符和最后一个字符都是一个空格字符，`/*` 在选择器左侧，`*/`在 `}` 右侧，行与行之间相隔多行

```css
/* .example {
        display: block;
        font-size: 18px;
        color: red;
} */
```



文件信息注释：

在样式文件编码声明 `@charset` 语句下面注明页面名称、作者、创建日期等信息

```css
@charset "utf-8";
/**
 * @desc index.css
 * @author jsx
 * @date 2021-10-12
 */
```



## 4. CSS 书写规范

无论多少人开发一个项目，规范是最初始也是最基本的，一个好的规范不仅让代码看起来完美也能够解决冲突。下面是我在开发时使用的css规范



### 4-1 CSS 书写顺序

- 布局定位属性：display / position / float / clear / visibility / overflow 
- 自身盒模型属性：width / height / margin / padding / border / background 
- 文本属性：color / font / text-decoration / text-align / vertical-align / white- space / break-word 
- 其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient …

```css
.example {
    display: block;
    position: relative;
    float: left;
    width: 100px;
    height: 100px;
    margin: 0 10px;
    padding: 20px 0;
    font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif;
    color: #333;
    background: rgba(0,0,0,.5);
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    -o-border-radius: 10px;
    -ms-border-radius: 10px;
    border-radius: 10px;
}
```



CSS3 浏览器私有前缀在前，标准前缀在后

```css
.example {
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    -o-border-radius: 10px;
    -ms-border-radius: 10px;
    border-radius: 10px;
}
```



### 4-2 CSS 语法规范

- 样式选择器，属性名，属性值关键字全部使用小写字母书写，属性字符串允许使用大小写
- 为了代码的易读性，在每个声明块的左花括号前添加一个空格
- 每条声明语句的 `:` 后应该插入一个空格
- 所有声明语句都应当以分号结尾。最后一条声明语句后面的分号是可选的，但是，如果省略这个分号，你的代码可能更易出错
- css 属性值需要用到引号时，统一使用单引号
- 使用缩写，CSS有些属性是可以缩写的，比如 padding，margin，font 等等，这样精简代码同时又能提高用户的阅读体验
- 对于属性值或颜色参数，省略小于 1 的小数前面的 0 （例如，`.5` 代替 `0.5`；`-.5px` 代替 `-0.5px`）
- 十六进制值应该全部小写，例如，`#fff`
- 尽量使用简写形式的十六进制值，例如，用 `#fff` 代替 `#ffffff`
- 避免为 0 值指定单位，例如，用 `margin: 0;` 代替 `margin: 0px;`
- 选择器分隔，为单个 css 选择器或新申明开启新行
- 避免不必要的 CSS 选择符嵌套
- 不要使用 ID 选择器，少使用 ***** 通用选择器，不使用没有语义定义的标签选择器，**选择器声明常用 class 类选择器声明**

```css
.example {
    margin: 0 0 2px 2px;
    font-family: 'Arial';
    color: #fff;
    transition all .5s;
    padding: 0;
}


.example,
.example-menu,
.example-list {
    color: #fff;
}
```



### 4-3 @charset规则

* 样式文件必须写上 @charset 规则，并且一定要在样式文件的第一行首个字符位置开始写，编码名用 “UTF-8”

* 否则的话就会有机会让 BOM 设置生效（如果有设置 BOM 的话）而优于 @charset 作为样式表的编码
* @charset "utf-8";  一定要写上，并且用小写字母，不能出现转义符

```css
@charset "UTF-8";
/* @charset规则在文件首行首个字符开始 */

/**
 * @desc File Info
 * @author Author Name
 * @date 2015-10-10
 */
```



### 4-4 命名规范

* 样式选择器命名使用小写字母书写

* 长名称或词组可以使用中横线 `-` 来为选择器命名
* 不建议使用 `_` 下划线来命名 CSS 选择器
  - 输入的时候少按一个 shift 键
  - 浏览器兼容问题 （比如使用 ._tips 的选择器命名，在IE6是无效的）
  - 能良好区分 JavaScript 变量命名（ js 变量命名是用 `_`）



#### 1. 常用的命名规则

**页面结构：**

| classname                | 常用命名          |
| ------------------------ | ----------------- |
| 容器                     | container         |
| 页头                     | header            |
| 内容                     | content/container |
| 页面主体                 | main              |
| 页尾                     | footer            |
| 导航                     | nav               |
| 侧栏                     | sidebar           |
| 栏目                     | column            |
| 页面外围控制整体布局宽度 | wrapper           |
| 左右中                   | left right center |



**功能：**

| classname | 常用命名  |
| --------- | --------- |
| 标志      | logo      |
| 广告      | banner    |
| 登陆      | login     |
| 注册      | regsiter  |
| 搜索      | search    |
| 状态      | status    |
| 按钮      | btn       |
| 提示信息  | msg       |
| 文章列表  | list      |
| 标签页    | tab       |
| 小技巧    | tips      |
| 图标      | icon      |
| 投票      | vote      |
| 友情链接  | link      |
| 版权      | copyright |
| 指南      | guild     |
| 当前的    | current   |
| 滚动      | scroll    |
| 下载      | download  |



**常用css文件名：**

| css file name | 文件夹命名  |
| ------------- | ----------- |
| 主要的        | master.css  |
| 模块          | module.css  |
| 基本共用      | base.css    |
| 布局，版面    | layout.css  |
| 主题          | themes.css  |
| 专栏          | columns.css |
| 文字          | font.css    |
| 表单          | forms.css   |
| 补丁          | mend.css    |
| 打印          | print.css   |



#### 2. 语义化命名

1. **Container** 就是将你页面中的所有元素包在一起的部分，这部分你还可以命名为: wrapper，wrap，page
2. **Header** 是网站页面的头部区域，一般来讲，它包含网站的logo和一些其他元素。这部分你还可以命名为：top，logo，page-header（或 pageHeader）
3. **Navbar** 等同于横向的导航栏，是最典型的网页元素。这部分你还可以命名为：nav，navigation，nav-wrapper
4. **Menu** 区域包含一般的链接和菜单，这部分你还可以命名为：sub-nav，links
5. **Main** 是网站的主要区域，如果是博客的话它将包含你的日志。这部分你还可以命名为：content，main-content（or mainContent ）
6. **Sidebar** 部分可以包含网站的次要内容，比如最近更新内容列表、关于网站的介绍或广告元素等…这部分你还可以命名为：sub-nav，side-panel，secondary-content
7. **Footer** 包含网站的一些附加信息，这部分你还可以命名为：copyright

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211012204139.png)



## 5. CSS 引入

CSS 的引入一般有两种，link 和 @import

一般建议使用 link 标签引入，这样可以避免考虑 `@import` 的语法规则和注意事项，避免产生资源文件下载顺序混乱和 http 请求过多的烦恼。

采用 @import 语法，必须书写在`<style>`标签中，并且必须在第一行书写

```html
<!-- link标签引入 -->
<link href="xxx.css" rel="stylesheet">

<!-- @import语法引入 -->
<style type="text/css">
    @import url("xxx.css");
    @import url(xxx.css);
    @import "xxx.css";
</style>
```

