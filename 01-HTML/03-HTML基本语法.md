# HTML基本语法

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210527211952.png)

HTML 它有属于它自己的一套专属语法。我们如果想要编写一个完整的网页，那么我们就必须遵循 HTML 的语法来编写代码，HTML 都是由各种**标签**构成的，我们只需要记住这些标签的写法和意义，那么我们就可以编写网页的基本结构了。

HTML 文件都由不同的标签构成的：

``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>html基本语法</title>
</head>
<body>
	<h1>标题标签</h1>
    <p>文本标签</p>
</body>
</html>
```



## 1. HTML 文档的构成

HTML 文档都是由标签构成的，大部分标签都是**双标签**，由**开始标签**和**结束标签**组成，小部分标签为**单标签**，没有结束标签。每个标签有特定的作用

HTML 标签 和 HTML 元素 通常都是描述同样的意思。

但是严格来讲, 一个 HTML 元素包含了开始标签与结束标签。

```html
<!-- 双标签 -->
<div>双标签</div>

<!-- 单标签 -->
<img src="" alt=""/>
```



## 2. 标签语法

- HTML 元素以**开始标签**起始
- HTML 元素以**结束标签**终止
- **元素的内容**是开始标签与结束标签之间的内容
- 某些 HTML 元素具有**空内容（empty content）**
- 空元素**在开始标签中进行关闭**（以开始标签的结束而结束）
- 大多数 HTML 元素可拥有**属性**



### 2-1 双标签的写法

双标签是成对出现的， 结束标签在标签名前会多一个`/`

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210528220344.png)

```html
<p>这是一段话</p>
```



### 2-2 单标签的写法

没有内容的 HTML 元素被称为空元素，空元素也称为单标签，单标签没有结束标签。

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210528221719.png)

```html
<img src="https://www.baidu.com/img.png" alt='百度图片'>
```



## 3. 标签的内容和属性



### 3.1 标签的内容

标签的内容写在开始标签和结束标签之间， 代表这段内容由特定的标签修饰。

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210528222507.png)

```html
<p>这是一段话 <!-- 这段为p标签的内容 --></p>
```

> **Tips**：单标签没有内容，因为它没有尾标签，通常我们指的是双标签之间的内容。



### 3.2 标签的属性

属性是 HTML 元素提供的附加信息。属性一般描述于开始标签，属性总是以名称/值对的形式出现。

| 属性  | 作用                                                         |
| ----- | ------------------------------------------------------------ |
| class | 为html元素定义一个或多个类名（classname）(类名从样式文件引入) |
| id    | 定义元素的唯一id                                             |
| style | 规定元素的行内样式（inline style）                           |
| title | 描述了元素的额外信息 (作为工具条使用)                        |



标签的属性，如果是标签为双标签，则属性写在开始标签中（开始标签的`<>`内）， 如果是单标签，则写在标签的`<>`内。

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210528222936.png)

```html
<!-- 双标签的属性写在头标签的<>内 -->
<a href="https://www.baidu.com">百度</a> 

<!-- 单标签的属性写在标签的<>内 -->
<img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210525201037.png" alt="HTML图片"> 
```

* 标签的属性有三部分构成，**属性名**，**等号**，**属性值**
* 等号左边的为属性名，等号右边的为属性值
* 属性值必须由引号引起来，单引号和双引号都可以
* 标签的属性用来给标签添加属性，让标签有特定的作用



## 4. HTML 标签的关系



### 4.1 嵌套关系

一组标签写在另外一组标签之间，充当了另外一组标签的内容。

```html
<div>
  <p>我是一个p标签</p>
</div>
```

标签与标签之间是可以嵌套的，但先后顺序必须保持一致。



### 4.2 并列关系

一组标签和另外一组标签平级，没有任何的嵌套关系。

```html
<div>我是一个div标签</div>
<p>我是一个p标签</p>
```

>**Tips**：HTML 标签只有两种关系，要么是嵌套关系，要么是并列关系。



## 5. HTML 规范

* HTML 文档开头必须要有 DTD 文档类型定义（使用正确的文档类型），HTML5直接使用`<！DOCTYPE html>`。
* HTML页面的后缀名是 .html 或者 .htm。（有一些系统不支持后缀名长度超过3个字符，比如dos系统）
* HTML不区分大小写，建议 HTML 标签名、类名、标签属性、大部分属性值统一用小写。
* 所有的标签都必须闭合。

  - 双标签：`<span></span>`

  - 单标签：`<br>` 建议写成 `<br />`   `<hr>` 建议转成 `<hr />`，还有`<img src=“URL” />`
* 所有标签元素都要正确的嵌套，不能交叉嵌套。
* 属性与标记之间、各属性之间需要以空格隔开。属性值以双引号括起来。
* 所有的属性值必须加引号。`<font  color="red"></font>`
* 所有的属性必须有值。`<input type="radio" checked="checked" />`
* HTML 对换行不敏感，对 tab 不敏感，HTML 中所有的文字之间，如果有空格、换行、tab都将被折叠为一个空格显示。
* 使用图片标签时，通常使用 alt 属性，在图片不能显示时，它能替代图片显示。
* 避免单行代码过长。

