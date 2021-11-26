# CSS 选择器



## 1. 什么是 CSS 选择器？

CSS 选择器用于查找（或选取）要设置样式的 HTML 元素

CSS 选择器可以分为五大类：

- 简单选择器（根据名称、id、类来选取元素）
- 组合器选择器（根据它们之间的特定关系来选取元素）
- 伪类选择器（根据特定状态选取元素）
- 伪元素选择器（选取元素的一部分并设置其样式）
- 属性选择器（根据属性或属性值来选取元素）

```css
选择器 {属性: 属性值;}
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211011174428.png)

## 2. 简单选择器

* 元素选择器
* 类选择器
* id 选择器
* `*` 通用选择器（通配符）
* 交集与并集选择器



### 2-1 元素选择器

元素选择器根据元素名称来选择 HTML 元素

```css
<h1>元素选择器</h1>

h1 {
    color: #3496db;
}
```



### 2-2  类选择器

class 选择器选择有特定 class 属性的 HTML 元素

class 选择器有别于id选择器，class可以在多个元素中使用

class 选择器在HTML中以class属性表示, 在 CSS 中，类选择器以一个点"."号显示

```css
<h1 class="example">类选择器</h1>

.example {
    color: #3496db;
}
```



**组合元素选择器**

类选择器可以结合元素选择器来使用

```css
<h1 class="example">组合元素选择器</h1>

h1.example {
    color: #3496db;
}
/* 选择器现在会匹配 class 属性包含 example 的所有 h1 元素 */
```



**多类选择器**

> 在 IE7 之前的版本中，不同平台的 Internet Explorer 都不能正确地处理多类选择器。

一个 class 值中可能包含一个词列表，各个词之间用空格分隔

通过把两个类选择器链接在一起，仅可以选择同时包含这些类名的元素（类名的顺序不限）

```css
<h1 class="example hover">多类选择器</h1>

.example.hover {
    color: #3496db;
}
/* 选择器现在会匹配 class 属性同时包含 example和 hover 的所有元素 */
```



### 2-3 id 选择器

在一个HTML文档中，id 选择器会根据该元素的 id 属性中的内容匹配元素,元素 id 属性名必须与选择器中的 id 属性名完全匹配，此条样式声明才会生效。

id 选择器前面有一个 # 号 - 也称为棋盘号或井号

类选择器与 id 选择器，类名与 id 第一个字符不能使用数字

```css
<h1 id="#example">id 选择器</h1>

#example {
    color: #3496db;
}
```



类选择器与 id 选择器的区别：

* 在一个 HTML 文档中，id 选择器会使用一次，而且仅一次
* 不同于类选择器，id 选择器不能结合使用，因为 id 属性不允许有以空格分隔的词列表



### 2-4 通用选择器（通配符）

通用选择器（*）选择页面上的所有的 HTML 元素

通用选择器可以与任何元素匹配

```css
* {
    color: 3496db;
}
/* 设置所有html元素字体颜色 */
```



### 2-5 交集与并集选择器

交集选择器：由两个或者多个选择器构成，其中第一个为标签选择器，第二个为class选择器，两个选择器之间不能有空格

交集选择器没有空格

```css
<h1 class="example">交集选择器</h1>
<p class="example">交集选择器</p>

.example {
    color: #3496db;
}

h1.example {
    color: #000;
}
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211022200237.png)



并集选择器：可以选择多组标签，同时为它们定义相同的样式，通常用于集体声明

定义的时候用`,`逗号隔开,任何形式的选择器都可以成为并集选择器的一部分，最后洗个选择器后面不需要`,`逗号隔开

```css
<h1>并集选择器</h1>
<p>并集选择器</p>
<div class="example">并集选择器</div>

h1，p, .example {
    color: #3496db;
}
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211022200946.png)

## 3. 组合选择器

- 后代选择器 (空格)
- 子选择器 (>)
- 相邻兄弟选择器 (+)
- 通用兄弟选择器 (~)



### 3-1 文档结构树的父子关系

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211022213202.png)

相邻的两个节点（层级是连续的）它们之间是父子关系

两个元素之间跨越两个层级以上，它们是祖辈和后代的关系



### 3-2 后代选择器

后代选择器匹配属于指定元素后代的所有元素

```css
<div>
    <p>后代选择器</p>
    <p>后代选择器</p>
    <p>后代选择器</p>
    <h1>
        <p>后代选择器</p>
    </h1>
</div>

div p {
    color: #3496db;
}
/* 匹配div后代里的所有p元素 */
```



### 3-3 子选择器

子选择器匹配属于指定元素子元素的所有元素

```css
<div>
    <p>子选择器</p>
    <p>子选择器</p>
    <p>子选择器</p>
    <h1>
        <p>子选择器</p>
    </h1>
</div>

div > p {
    color: #3496db;
}
/* 匹配div子元素中所有的p元素 */
```



### 3-4 相邻兄弟选择器

相邻兄弟选择器匹配所有作为指定元素后的相邻同级的元素

兄弟（同级）元素必须具有相同的父元素，相邻的意思是紧随其后（只匹配一个元素）

```css
<div>
    <h1>相邻兄弟选择器</h1>
    <p>相邻兄弟选择器</p>
    <p>相邻兄弟选择器</p>
</div>
<p>相邻兄弟选择器</p>
<p>相邻兄弟选择器</p>

div + p {
    color: #3496db;
}
/* 匹配与div同级紧随其后的一个p元素 */
```



### 3-5 通用兄弟选择器

通用兄弟选择器匹配属于指定元素后的同级元素的所有元素

```css
<p>相邻兄弟选择器</p>
<div>
    <h1>相邻兄弟选择器</h1>
    <p>相邻兄弟选择器</p>
    <p>相邻兄弟选择器</p>
</div>
<p>相邻兄弟选择器</p>
<p>相邻兄弟选择器</p>

div ~ p {
    color: #3496db;
}
/* 匹配与div同级后面的所有p元素 */
```



## 4. 伪类选择器

CSS **伪类** 是添加到选择器的关键字，指定要选择的元素的特殊状态

```css
/* 语法 */
selector:pseudo-class {
  property: value;
}
```



### 4-1 常用状态伪类选择器

* :link：未被访问状态
* :visited：已经被访问过的状态
* :hover：鼠标移到元素上时的状态
* :active：鼠标移动到元素上点击不松手时的状态
* :focus：元素获得焦点时的状态

```css
a:link {
    background-color: #40407a;
}

a:visited {
    background-color: #706fd3;
}

a:hover {
    background-color: #ff5252;
}

a:active {
    background-color: #33d9b2;
}
```



### 4-2 常用的结构伪类选择器

* :first-child：匹配兄弟元素中的第一个元素
* :first-of-type：匹配兄弟元素中第一个某种类型的元素
* :last-child：匹配兄弟元素中最末的那个元素
* :last-of-type：匹配兄弟元素中最后一个某种类型的元素
* :not：匹配作为值传入自身的选择器未匹配的物件
* :nth-child：匹配一列兄弟元素中的元素——兄弟元素按照an+b形式的式子进行匹配
* :nth-of-type：匹配某种类型的一列兄弟元素（比如，`<p>`元素）——兄弟元素按照an+b形式的式子进行匹配
* :nth-last-child：匹配一列兄弟元素，从后往前倒数。兄弟元素按照an+b形式的式子进行匹配
* :nth-last-of-type：匹配某种类型的一列兄弟元素（比如，`<p>`元素），从后往前倒数。兄弟元素按照an+b形式的式子进行匹配
* :root：匹配文档的根元素

```css
:root

/* 匹配兄弟元素中第一个p元素 */
p:first-child {
    color: #ddd;
}

/* 匹配兄弟元素中某种类型的第一元素 */
p:first-of-type {
    color: #ddd;
}

/* 匹配兄弟元素中最后一个p元素 */
p:last-child {
    color: #ddd;
}

/* 匹配兄弟元素中p元素类型的最后一个p元素 */
p:last-of-type {
    color: #ddd;
}

/* 匹配除了p元素的其它所有元素 */
:not(p) {
    color: #ddd;
}

/* 匹配一列兄弟元素中的元素 */
p:nth-child(an+b) {
    color: #ddd;
}

/* 匹配某种类型的一列兄弟元素  */
p:nth-of-type(an+b) {
    color: #ddd;
}

/* 匹配一列兄弟元素中的元素 从后往前*/
p:nth-last-child {
    color: #ddd;
}

/* 匹配某种类型的一列兄弟元素 从后往前 */
p:nth-last-of-type {
    color: #ddd;
}
```



### 4-3 常用的表单伪类选择器

* :checked：匹配处于选中状态的单选或者复选框
* :disable：匹配处于关闭状态的用户界面元素
* :enabled：匹配处于开启状态的用户界面元素
* :read-only：匹配用户不可更改的元素（包括 disabled 属性）
* :valid：内容验证正确的 `<input>` 或其他的 `<form>` 元素

```css
/* 验证 input输入是否正确 */
input:vaild {
    color: #ddd;
}

 /* 匹配用户不可更改的元素 */
input:read-only {
    color: #ddd;
}

/* 匹配处于关闭状态的用户界面元素 */
input:disabled {
    color: #ddd;
}

/* 匹配处于开启状态的用户界面元素 */
input:enabled {
    color: #ddd;
}

/* 匹配处于选中状态的单选或者复选框 */
input:checked {
    color: #ddd;
}
```



## 5. 伪元素选择器

CSS伪元素是用来添加一些选择器的特殊效果

```css
/* 语法 */
selector::pseudo-element {
    property:value;
}
```

* ::first-line：用于向文本的首行设置特殊样式，只能用于块级元素
* ::first-letter：用于向文本的首字母设置特殊样式，只能用于块级元素
* ::before：在元素的内容前面插入新内容，使用 content 属性来指定要插入的内容
* ::after：在元素的内容之后插入新内容，使用 content 属性来指定要插入的内容

```css
/* 用于向文本的首行设置特殊样式 */
p::first-line {
    color: #ddd;
}

/* 用于向文本的首字母设置特殊样式 */
p::first-letter {
    color: #ddd;
}

/* 在元素的内容前面插入新内容 */
p::before {
    content: "xxx";
}

/* 在元素的内容之后插入新内容 */
p::after {
    content: "xxx";
}
```



## 6. 属性选择器

根据属性或属性值来选取元素，为带有特定属性的 HTML 元素设置样式

| 选择器          | 示例                            | 描述                                                         |
| :-------------- | :------------------------------ | :----------------------------------------------------------- |
| `[attr]`        | `a[title]`                      | 匹配带有一个名为*attr*的属性的元素——方括号里的值。           |
| `[attr=value]`  | `a[href="https://example.com"]` | 匹配带有一个名为*attr*的属性的元素，其值正为*value*——引号中的字符串。 |
| `[attr~=value]` | `p[class~="special"]`           | 匹配带有一个名为*attr*的属性的元素 ，其值正为*value*，或者匹配带有一个*attr*属性的元素，其值有一个或者更多，至少有一个和*value*匹配。注意，在一列中的好几个值，是用空格隔开的。 |
| `[attr|=value]` | `div[lang|="zh"]`               | 匹配带有一个名为*attr*的属性的元素，其值可正为*value*，或者开始为*value*，后面紧随着一个连字符。 |

| 选择器          | 示例                | 描述                                                         |
| :-------------- | :------------------ | :----------------------------------------------------------- |
| `[attr^=value]` | `li[class^="box-"]` | 匹配带有一个名为*attr*的属性的元素，其值开头为*value*子字符串。 |
| `[attr$=value]` | `li[class$="-box"]` | 匹配带有一个名为*attr*的属性的元素，其值结尾为*value*子字符串 |
| `[attr*=value]` | `li[class*="box"]`  | 匹配带有一个名为*attr*的属性的元素，其值的字符串中的任何地方，至少出现了一次*value*子字符串。 |



### 6-1 [attribute] 

选择器用于选取带有指定属性的元素

```css
<!-- [attribute] 匹配带有指定属性的元素 -->
<div>
    <a href="#">匹配带有指定属性的元素</a>
    <a href="#" title="链接">匹配带有指定属性的元素</a>
</div>

/* a标签中有title属性的元素设置颜色样式 */
a[title] {
    color: #FD7272;
}
```



### 6-2 [attribute="value"] 

选择器用于选取带有指定属性和值的元素

```css
<!-- [attribute="value"] 匹配带有指定属性和值的元素-->
<div>
    <a href="#" target="_blank">匹配带有指定属性和值的元素</a>
    <a href="#">匹配带有指定属性和值的元素</a>
</div>

/* a标签中有target属性并且值为_blank的元素设置样式 */
a[target="_blank"] {
    color: #9b59b6;
}
```



### 6-3 [attribute~="value"] 

> **Tips：**值必须是完整或单独的单词

选择器选取属性值包含指定词的元素

```css
<!-- [attribute~="value"] 选择器选取属性值包含指定词的元素 -->
<div>
    <p class="hello world">选择器选取属性值包含指定词的元素</p>
    <p class="world">选择器选取属性值包含指定词的元素</p>
</div>

/* p标签中有class属性并且值包含hello的元素设置样式 */
p[class~="hello"] {
    background-color: #e74c3c;
}
```





### 6-4 [attribute|="value"]

> **Tips：**值必须是完整或单独的单词

选择器用于选取指定属性以指定值开头的元素

```css
<!-- [attribute|="value"] 选择器用于选取指定属性以指定值开头的元素 值必须是完整单词-->
<div>
    <p class="top-box">选择器用于选取指定属性以指定值开头的元素</p>
    <p class="topbox">选择器用于选取指定属性以指定值开头的元素</p>
</div>

/* p标签中有class属性并且值必须以top开头的元素设置样式 值必须为完整单词 */
p[class|="top"] {
    background-color: #3498db;
}
```





### 6-5 [attribute^="value"] 

> **Tips：**值不必是完整单词！

选择器用于选取指定属性以指定值开头的元素

```css
<!-- [attribute^="value"]  选择器用于选取指定属性以指定值开头的元素 值不必是完整单词 -->
<div>
    <p class="text">选择器用于选取指定属性以指定值开头的元素</p>
    <p class="textbox">选择器用于选取指定属性以指定值开头的元素</p>
    <p title="文本描述">选择器用于选取指定属性以指定值开头的元素</p>
</div>

/* p标签中有class属性并且值必须以top开头的元素设置样式 值不必为完整单词 */
p[class^="text"] {
    background-color: #2ecc71;
}
```





### 6-6 [attribute$="value"]

> **Tips：**值不必是完整单词！

选择器用于选取指定属性以指定值结尾的元素

```css
<!-- [attribute$="value"] 选择器用于选取指定属性以指定值结尾的元素 值不必是完整单词 -->
<div>
    <p class="content-box">选择器用于选取指定属性以指定值结尾的元素</p>
    <p class="contentbox">选择器用于选取指定属性以指定值结尾的元素</p>
    <p class="content-boxs">选择器用于选取指定属性以指定值结尾的元素</p>
</div>

/* p标签中有class属性并且值必须以box结尾的元素设置样式 值不必为完整单词 */
p[class$="box"] {
    background-color: #1abc9c;
}
```





### 6-7 [attribute*="value"] 

> **Tips：**值不必是完整单词！

选择器选取属性值包含指定词的元素

```css
<!-- [attribute*="value"] 选择器选取属性值包含指定词的元素 值不必是完整单词 -->
<div>
    <p class="nbsp">选择器选取属性值包含指定词的元素</p>
    <p class="spell">选择器选取属性值包含指定词的元素</p>
</div>

/* p标签中有class属性并且值包含指定词的元素 值不必为完整单词 */
p[class*="sp"] {
    background-color: #f1c40f;
}
```

