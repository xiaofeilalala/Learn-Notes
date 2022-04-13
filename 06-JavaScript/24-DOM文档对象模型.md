# DOM 文档对象模型

## 1. Web API 的概念

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220411130252.png)

`api`：应用程序接口。是基于编程语言构建的结构，使开发人员更容易地创建复杂的功能

`Web API`：是 `Web` 的应用程序编程接口，浏览器 `API` 可以扩展 `Web` 浏览器的功能，服务器 `API` 可以扩展 `Web` 服务器的功能

`js` 组成：`ECMAScript`、`DOM`、`BOM`

* `ECMAScript`：定义了 `JavaScript` 的语法规范
* `BOM`：浏览器对象模型，提供用于处理文档之外的所有内容的其他对象
* `DOM`：文档对象模型，提供用于处理文档的对象



## 2. DOM 基础知识

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220411162141.png)



### 2-1 window 对象

* 所有浏览器都支持 `window` 对象，它代表浏览器的窗口，不同窗口对应着不同的`window` 对象
* 在 `javascript` 中，`window` 为全局对象，全局函数和全局变量为 `window` 对象的属性，`document` 对象也是 `window` 对象属性

```js
var user = "jsx";
console.log(window.user); // jsx

function username() {
    console.log('hello dom')
}
window.username()

// 获取浏览器窗口高度
console.log(window.innerHeight)
```



### 2-2 文档对象模型

文档对象模型 `DOM` 是 `HTML` 和 `XML` 文档的编程接口

* 提供了对文档的结构化的表述，
* 定义了一种方式可以使从程序中对该结构进行访问，从而改变文档的结构，样式和内容
* `DOM` 将文档解析为一个由节点和包含属性和方法的对象组成的结构集合

可以通过 `document` 对象去访问改变文档结构、样式和内容

```js
console.log(window.document);
window.onload = function() {
    // 给body标签添加样式
    document.body.style.background = '#2ecc71'
}
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220411160643.png)



### 2-3 DOM 自动修正

浏览器遇到格式不正确的 `HTML`，它会在形成 `DOM` 时自动更正它

* `html` 根标签即使它不存在于文档中 — 它也会出现在 `DOM` 中
* 在生成 `DOM` 时，浏览器会自动处理文档中的错误，关闭标签等
* `</body>` 标签后的所有元素，`DOM` 都会修正添加到 `<body>` 标签内
* 表格必须具有 `<tbody>` 标签，忽略时浏览器在创建 `DOM` 时，自动地创建了 `<tbody>`

```html
<ul><li>修复未闭合标签</ul>
// 补充闭合标签
<ul> <li>修复未闭合标签</body></ul>

<table>
    <tr><td>自动补充tbody标签</tr>
</table>
// 补充tbody标签
<table>
    <tbody>
        <tr><td>自动补充tbody标签</td></tr>
    </tbody>
</table>
```



### 2-4 控制台交互

- `Stylel` 样式 — 我们可以看到按规则应用于当前元素的 `CSS` 规则
- `Computed` 计算属性 — 按属性查看应用于元素的 `CSS`：对于每个属性，我们可以都可以看到赋予它的规则包括 `CSS` 继承等
- `Event Listeners` 事件监听 — 查看附加到 `DOM` 元素的事件侦听器

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220411171729.png)

* 在控制台中获取节点，可以通过点击左上角的小图标按钮可以让我们使用鼠标从网页中选择一个节点
* 也可以在元素 `Elements` 选项面板中选择元素节点
* 也可以通过 `document` 对象来获取元素节点
* 按下 `Esc` — 它将在元素 `Elements` 选项面板下方打开控制台 `Console`
* 最后选中的元素可以通过 `$0` 来进行操作，先前选择的是 `$1`等

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220411173211.png)



## 3. DOM 节点类与树结构

### 3-1 DOM 节点类

* `EventTarget`：该类的对象从未被创建，由可以接收事件、并且可以创建侦听器的对象实现，提供 `addEventListener`、`removeEventListener` 等事件支持方法
  * `Element` 元素节点、`document` 文档节点和 `window` 对象部署了该类
  * `XMLHttpRequest`，`AudioNode`，`AudioContext` 也部署了该类
* `Node`：是所有 `DOM` 节点的基础（包含所有节点类型）
  * `Node` 类的对象从未被创建，提供 `firstChild`、`parentNode` 等节点操作方法
* `Element`：是 `DOM` 元素的基本类（包含所有元素节点）
  * 提供`getElementsByTagName`、`querySelector` 等方法
* `HTMLElement`：是所有 `HTML` 元素的基本类（包含所有 `HTML` 元素节点）
  * 提供`childNodes`、`nodeType`、`nodeName`、`className`、`nodeName`等方法
* `SVGElement`：是所有 `SVG` 元素的基本类（包含所有 `SVG` 元素节点）
* `XMLElement`：是所有 `XML` 元素的基本类（包含所有 `XML` 元素节点）

```js
console.dir(window)
// window对象包含了所有节点类
console.log(window.Node);
console.log(window.Element)
console.log(window.HTMLElement)
console.log(HTMLElement.__proto__ === Element); // true
console.log(Element.__proto__ === Node) // true
console.log(Node.__proto__ === EventTarget) // true
console.log(EventTarget.__proto__.__proto__.__proto__) // true
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220411213528.png)



### 3-2 DOM 树

`DOM` 树：包含了 `HTML` 所有元素，并向网页文档本身提供了全局操作功能

根据文档对象模型，每个 `HTML` 元素都是一个节点对象，嵌套的元素是闭合元素的子元素，元素内的文本也是一个文本节点对象

* `document` 文档节点：`document` 是 `DOM` 操作的起始节点
* `element` 元素节点：`HTML` 元素被称为元素节点，`<html>` 为根节点，`<head>` 和 `<body>` 为相对于 `<html>` 内的子节点
* `attribute` 属性节点：`HTML` 元素中元素的属性 `attributes` 则为属性节点
* `text` 文本节点：`HTML` 元素内的文本被称为文本节点
  * 一个文本节点只包含一个字符串
  * 换行符和空格在 `HTML` 元素中不会被忽略都是有效字符，它们会形成文本节点
* `comment` 注释节点：在 `HTML` 中注释 `<!-- -->` 为注释节点

```html
<!DOCTYPE html>
<html lang="en"><!-- 根节点 -->
<head>
    <meta charset="UTF-8">
    <title>DOM树</title>
</head>
<body>
    <!-- 注释节点 -->
    文本内容 <!-- 文本节点 -->
    <div class="box">属性节点</div><!-- 属性节点 -->
</body>
</html>
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220411165829.png)



### 3-2 常用节点

需要保证浏览器已经渲染了内容才可以读取的节点对象，不然无法读取到节点对象

* `document`：`DOM` 操作的起始节点
* `document.documentElement`：文档节点即 `html` 元素节点
* `document.body`：`body` 元素节点
* `document.head`：`head` 元素节点
* `document.links`：超链接集合
* `document.anchors`：所有锚点集合
* `document.forms`：`form`表单集合
* `document.images`：图片集合

```html
<!-- 链接 -->
<a href="javascript:;">超链接</a>
<!-- 锚点 -->
<a href="#box">锚点</a>
<a name="box" class="box">锚点</a>
<!-- img -->
<img src="https://lf-cdn-tos.bytescm.com/obj/static/xitu_extension/static/gold.981a5510.svg" alt="">
<!-- form表单 -->
<form action="">
    <input type="text" value="form表单">
</form>

<script>
    // document 操作文档起点
    // document.documentElement html根元素节点
    console.log(document.documentElement)

    // document 还能获取title标签 URL 域名 来源地址
    console.log(document.title);
    console.log(document.URL); 
    // http://127.0.0.1:5500/05-JavaScript/23-DOM/07-%E5%B8%B8%E7%94%A8%E8%8A%82%E7%82%B9.html#box
    console.log(document.domain); // 127.0.0.1
    console.log(document.referrer);
     // http://127.0.0.1:5500/05-JavaScript/23-DOM/07-%E5%B8%B8%E7%94%A8%E8%8A%82%E7%82%B9.html

    // 节点对象需要浏览器渲染了html才能读取
    console.log(document.body); // body元素节点
    console.log(document.head); // head元素节点
    console.log(document.links); // 所有超链接的集合
    console.log(document.anchors); // 获取所有锚点
    console.log(document.images); // 获取所有图片
    console.log(document.forms); // 获取所有图片
</script>
```



## 4. 节点属性

### 4-1 nodeType 属性

`nodeType` 属性提供以数值形式获取 `DOM` 节点类型

- 对于元素节点 `elem.nodeType == 1`，
- 对于元素属性节点 `elem.nodeType == 2`
- 对于文本节点 `elem.nodeType == 3`，
- 对于注释节点 `elem.nodeType == 8`
- 对于 `document` 对象 `elem.nodeType == 9`

| nodeType | 说明            |
| -------- | --------------- |
| 1        | 元素节点        |
| 2        | 属性节点        |
| 3        | 文本节点        |
| 8        | 注释节点        |
| 9        | `document` 对象 |

```html
<div class="container">hello DOM</div>
<!-- hello dom -->

<script>
    let body = document.body
    let container = document.querySelector('.container')
    console.log(body.nodeType); // 1 body节点属性

    let attr = container.attributes.class;
    console.log(attr.nodeType); // 2 属性节点
    
    console.log(body.firstChild.nodeType); // 3 文本属性

    console.log(body.childNodes[3].nodeType); // 8 节点注释节点

    let doc = document;
    console.log(document.nodeType); // 9 document对象
</script>
```



### 4-2 nodeName & tagName 属性

`nodeName`：获取指定任何节点类型的名称，获取值为大写形式（为任意 `Node` 节点定义的）

| nodeType | nodeName            |
| -------- | ------------------- |
| 1        | 元素名称 大写 `DIV` |
| 2        | 属性名称            |
| 3        | #text               |
| 8        | #comment            |
| 9        | #documnet           |

```html
<body>
    <!-- 注释 -->
    <h3 class="box" id="box">hello dom</h3>
    <script>
        let h3 = document.querySelector('.box');
        let body = document.body;
        // nodeNmae 获取任意节点的名称 值为大写
        console.log(document.nodeName); // #document
        console.log(body.nodeName); // BODY
        console.log(h3.firstChild.nodeName); // #text
        console.log(body.childNodes[1].nodeName); // #comment
        console.log(h3.attributes.class.nodeName); // class
    </script>
</body>
```



`tagName`：获取 `Element` 所有元素节点的名称，获取的值为大写的标签名（为任意`tagName` 元素节点定义）

* `tagName` 存在于 `Element` 类的原型中
* `document` 文档节点、文本节点、属性节点、注释节点值都为 `undefined`

```html
<!-- 注释 -->
<h3 class="box" id="box">hello dom</h3>

<script>
    let h3 = document.querySelector('.box');
    let body = document.body;

    // tagName 获取元素节点的名称 值为大写
    console.log(document.tagName); // undefined
    console.log(body.tagName); // BODY
    console.log(h3.firstChild.tagName); // undefined
    console.log(body.childNodes[1].tagName); // undefined
    console.log(h3.attributes.class.tagName); // undefined
</script>
```



### 4-3 nodeValue & data 属性

`nodeValue` 属性对于文本节点、注释节点来说，`nodeValue` 返回该节点的文本内容与注释。对于 `attribute` 节点来说,，返回该属性的属性值

| nodeType | nodeValue    |
| -------- | ------------ |
| 1        | `null`       |
| 2        | 属性的属性值 |
| 3        | 文本内容     |
| 8        | 注释内容     |
| 9        | `null`       |

```html
<!-- hello dom -->
<div class="box">hello world</div>

<script>
    let body = document.body;
    let box = document.querySelector('.box')
    console.log(body.nodeValue); // null
    console.log(box.firstChild.nodeValue); // hello world
    console.log(body.childNodes[1].nodeValue); //  hello dom 
    console.log(box.attributes.class.nodeValue); // box
    console.log(document.nodeValue); // null
</script>
```



`data` 属性可以获取文本节点与注释节点的内容，其它节点返回 `undefined`

```html
<!-- hello dom -->
<div class="box">hello world</div>

<script>
    // data获取文本节点和注释节点
    console.log(body.data); // null
    console.log(box.firstChild.data); // hello world
    console.log(body.childNodes[1].data); //  hello dom 
    console.log(box.attributes.class.data); // box
    console.log(document.data); // null
</script>
```



### 4-4 textContent 纯文本

`textContent` 属性会将元素的 `<tags>` 去掉，返回纯文本内容

使用 `textContent`，我们将其作为文本插入，标签符号不进行标签解析

```html
<div class="box">hello dom</div>

<script>
    let box = document.querySelector('.box');
    console.log(box.textContent); // hello dom
    console.log(box); // div节点对象

    // 通过textContent插入内容时，符号会按照字面意思写入
    box.textContent = '<p>hello jsx</p>';
    // <p>hello jsx</p>
    // 此时符号为字符串
</script>
```



### 4-5 hidden 是否可见

`hidden`  属性（attribute）和 `DOM` 属性（property）指定元素是否可见

```html
<div style="display: none"></div>
document.querySelector('div').hidden = true;

<div class="container"></div>

<script>
    let container = document.querySelector('.container')
    console.log(container.hidden); // false
    container.hidden = true
    console.log(container.hidden); // true
</script>
```



## 5. 元素与节点集合

### 5-1 HTMLCollection 元素集合

`HTMLCollection` 是元素的集合，还提供了用来从该集合中选择元素的方法和属性

`getElementsBy...` 方法返回的是 `HTMLCollection`，唯独 `getElementsByName` 方法返回的是 `NodeList`

* `length`属性：返回 `HTMLCollection` 集合当中元素的个数
* `item()`：返回 `HTMLCoolent` 对象中指定索引的元素
* `namedItem()`：根据 `Id` 或根据字符串所表示的 `name` 属性来获取元素

```html
<ul>
    <li name="jsx">1</li>
    <li name="jsx">2</li>
    <li id="js" name="jsx1">3</li>
    <li name="jsx">4</li>
    <li name="jsx">5</li>
</ul>

<script>
    // 元素集合
    // getElementsByName返回的是nodeList及节点集合
    let list = document.getElementsByName('jsx')
    console.log(list);
    // NodeList(5) [li, li, li, li, li]

    let htmllist = document.getElementsByTagName('li');
    console.log(htmllist);
    // HTMLCollection(5) [li, li, li, li, li, jsx: li]
    
    // length属性
    console.log(htmllist.length); // 5
    // 元素集合不是数组而是类数组
    console.log(Array.isArray(htmllist)); // false

    // item() 返回指定索引的元素
    console.log(htmllist.item(0));
    htmllist.item(0).style.color = "#ff0000";

    // namedItem() 根据id返回指定元素,根据字符串所表示的name属性来匹配
    console.log(htmllist.namedItem('js')) 
    // <li id="js" name="jsx1">3</li>
    console.log(htmllist.namedItem('jsx1'))
    // <li id="js" name="jsx1">3</li>
</script>
```



### 5-2 NodeList 节点集合

`NodeList` 是节点的集合，通常是由 `Node.childNodes` 和`document.querySelectorAll` 返回的

* `length` 属性：返回 `NodeList` 集合中包含的节点个数
* `item()`：返回 `NodeList` 对象中指定索引的节点

```html
<ul>
    <li name="jsx">hello</li>
    <li name="jsx">2</li>
    <li name="jsx">3</li>
    <li name="jsx">4</li>
    <li name="jsx">5</li>
</ul>

<script>
    // 节点集合
    // Node.childNodes
    let list = document.querySelector('body').childNodes
    console.log(list);
    // NodeList(8) [text, ul, text, comment, text, script, text, script]

    // querySelectorAll
    let list1 = document.querySelectorAll('li');
    console.log(list1); // NodeList(5) [li, li, li, li, li]

    // length属性
    console.log(list1.length); // 5

    // item() 选择节点
    console.log(list1.item(0)); // <li name="jsx">hello</li>
</script>
```



### 5-3 动态静态集合

`HTMLCollent` 和 `NodeList` 不是一个数组，是一个类似数组的对象

* 通过 `getElementsByName` 获取的 `Nodelist` 集合与 `getElementsBy...` 获取的`HTMLCollection` 集合都是动态的 `live` 集合，这样的集合始终反映的是文档的当前状态，并且在文档发生更改时会自动更新
* 使用 `querySelectorAll` 返回的是静态的 `static` 集合，文档发生更改时不会自动更新

```html
<ul>
    <li name="jsx">1</li>
    <li name="jsx">2</li>
    <li name="jsx">3</li>
    <li name="jsx">4</li>
    <li name="jsx">5</li>
</ul>
<div class="box">
    <button class="btn live">live 动态</button>
    <button class="btn static">static 静态</button>
    <button class="btn liveChangeStatic">将动态集合保存为静态集合</button>
</div>
    
<script>
    // 动态集合 操作数组时会实时更新
    let live = document.getElementsByTagName('li');
    let static = document.querySelectorAll('li');
    // 将动态集合保存静态集合
    let liveChangeStatic = Array.prototype.slice.call(live);
    console.log('live-->', live)
    console.log('static-->', static)
    let button1 = document.querySelector('.live');
    let button2 = document.querySelector('.static');
    let button3 = document.querySelector('.liveChangeStatic');
    button1.addEventListener('click', () => {
        document.querySelector('li').insertAdjacentHTML('beforebegin', '<li name="jsx">新元素</li>')
        console.log('live-->', live.length)
        console.log('live-->', live)
    })
    button2.addEventListener('click', () => {
        document.querySelector('li').insertAdjacentHTML('beforebegin', '<li name="jsx">新元素</li>')
        console.log('static-->', static.length)
        console.log('static-->', static)
    })
    button3.addEventListener('click', () => {
        document.querySelector('li').insertAdjacentHTML('beforebegin', '<li name="jsx">新元素</li>')
        console.log('liveChangeStatic-->', liveChangeStatic.length)
        console.log('liveChangeStatic-->', liveChangeStatic)
    })
</script>
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220412141932.gif)



## 6. 节点关系与元素导航

节点是父子级嵌套与前后兄弟关系，使用 `DOM` 提供的 `API` 可以获取这种关系的元素

节点是根据 `HTML` 内容产生的，所以也存在父子、兄弟、祖先、后代等节点关系

> * 节点：是所有节点类型
> * 元素节点：是 `HTML`、`XML`、`SVG` 元素节点
> * `HTML` 元素节点：是所有 `HTML` 标签元素的节点



### 6-1 获取父节点

`parentNode` 返回指定的节点在 `DOM` 树中的父节点

如果当前节点刚刚被建立，还没有被插入到 `DOM` 树中，则该节点的 `parentNode` 属性也返回 `null`

`document` 是顶级节点， `html `标签的父节点是 `document`

```html
<div class="parent">
    <div class="child">
        <div class="Sibling"></div>
        <div class="Sibling"></div>
    </div>
</div>

<script>
    // parentNode 返回指定节点在dom树中的父节点
    let child = document.querySelector('.child');
    console.log(child.parentNode); // <div class="parent"></div>
    // document 顶级节点
    console.log(document.documentElement.parentNode === document); // true
</script>
```

实现当前节点的所有父节点集合

```js
// 获取所有父节点
function parentNodes(node) {
	let nodes = [];
	while ((node = node.parentNode)) nodes.push(node)
	return nodes;
}
console.log(parentNodes(child));
// [div.parent, body, html, document]

// 递归 + 扩展运算符
function parentNodes(node) {
	const nodes = new Array(node.parentNode)
	if (node.parentNode) nodes.push(...parentNodes(node.parentNode))
	return nodes
}
```



### 6-2 获取子节点

>  **Tips：**文本和注释也是节点

* `firstChild`：返回树中节点的第一个子节点，如果节点是无子节点，则返回 `null`
* `lastChild`：返回当前节点的最后一个子节点，如果节点是无子节点，则返回 `null`
* `childNotes`：返回包含指定节点的子节点的集合，该集合为即时更新的集合（动态集合）

```html
<div class="parent">
    <div class="child">
        <div class="Sibling1"></div>
        <div class="Sibling2"></div>
    </div>
    <div class="child2"></div>
</div>
<script>
    // 获取子节点
    // firstChild dom树中第一个节点
    // lastChild dom树中最后一个子节点
    // childNodes 返回包含指定节点的所有子节点
    let parent = document.querySelector('.parent');
    console.log(parent.firstChild); // text 文本节点
    console.log(parent.lastChild); // text 文本节点
    // 返回所有子节点
    console.log(parent.childNodes); 
    // NodeList(5) [text, div.child, text, div.child2, text]
</script>
```

`hasChildNodes()` 方法检测指定节点是否有子节点

```js
// elem.hasChildNodes() 检测指定节点是否有子节点
console.log(document.documentElement.hasChildNodes()); // true
```



### 6-3 获取兄弟节点

> **Tips：**文本和注释也是节点

* `nextSibling`：返回当前节点的后一个兄弟节点，如果指定的节点为最后一个节点，则返回 `null`
* `prevousSibling`：返回当前节点的前一个兄弟节点,没有则返回 `null`

```html
<div class="parent">
    <div class="child">
        <div class="Sibling1"></div>
        <div class="Sibling2"></div>
    </div>
    <div class="child2"></div>
</div>

<script>
    // 获取兄弟节点
    // nextSibling 获取当前节点的下一个兄弟节点
    let siblings = document.querySelector('.Sibling1');
    console.log(siblings.nextSibling.nextSibling); 
    // <div class="Sibling2"></div>

    let child2 = document.querySelector('.child2');
    console.log(child2.previousSibling.previousSibling); 
    // <div class="child">
</script>
```



### 6-4 元素节点导航

对于很多任务来说，我们并不想要文本节点或注释节点，而是需要获取代表元素的和形成页面结构的元素节点

`DOM API` 也提供了只获取元素节点的属性

* `parentElement` 属性返回的是元素类型的父节点，而 `parentNode` 返回的是任何类型的父节点

* 根节点 `document.documentElement` 的父节点是 `document`，但 `document` 不是一个元素节点，所以 `parentNode` 返回了 `document`， `parentElement` 返回的是 `null`

| 节点属性                 | 说明           |
| ------------------------ | -------------- |
| `parentElement`          | 获取父元素     |
| `children`               | 获取所有子元素 |
| `childElementCount`      | 子元素的数量   |
| `firstElementChild`      | 第一个子元素   |
| `lastElementChild`       | 最后一个子元素 |
| `previousElementSibling` | 上一个兄弟元素 |
| `nextElementSibling`     | 下一个兄弟元素 |

```html
<div class="parent">
    <div class="child1">
        <div class="Sibling1"></div>
        <div class="Sibling2"></div>
    </div>
    <div class="child2"></div>
</div>

<script>
    // 元素节点导航
    // 只获取元素节点的属性

    // 获取父元素
    let child1 = document.querySelector('.child1');
    console.log(child1.parentElement)
    // <div class="parent"></div>

    // 获取所有子元素
    let parent = document.querySelector('.parent');
    console.log(parent.children)
    // HTMLCollection(2) [div.child1, div.child2]

    // 获取子元素的数量
    console.log(parent.childElementCount); // 2

    // 第一个子元素
    console.log(parent.firstElementChild)
    // <div class="child1"></div>

    // 最后一个子元素
    console.log(parent.lastElementChild)
    // <div class="child2"></div>


    // 获取上一个兄弟元素
    let Sibling2 = document.querySelector('.Sibling2')
    console.log(Sibling2.previousElementSibling)
    // <div class="Sibling1"></div>

    // 获取下一个兄弟元素
    console.log(child1.nextElementSibling)
    // <div class="child2"></div>
</script>
```



## 7. 节点遍历



### 7-1 for...of

`Nodelist` 与 `HTMLCollection` 是类数组的可迭代对象所以可以使用 `for...of`进行遍历

```html
<div class="container">
    <p>hello html</p>
    <p>hello css</p>
    <p>hello js</p>
    <p>hello vue</p>
</div>
    
<script>
    let elem = document.querySelector('.container').children;
    console.log(elem)
    for (const item of elem) {
        console.log(item)
    }
</script>
```



### 7-2 forEach

虽然 `NodeList` 不是一个数组，但是可以使用 `forEach()` 来迭代，但 `HTMLCollection` 则不可以

```js
// HTMLCollector 不能使用forEach遍历节点
let elem = document.querySelectorAll('p');
console.log(elem)
elem.forEach((item) => {
	console.log(item)
})
```



### 7-3 call/apply

节点集合对象原型中不存在 `map` 方法，但可以借用 `Array` 的原型 `map` 方法实现遍历

```js
let elem = document.querySelector('.container').children;
console.log(elem);
[].map.call(elem, (node, index) => {
 	console.log(node, index)
})
```



### 7-4 Array.from

`Array.from` 用于将类数组转为真数组，并提供第二个迭代函数，所以可以借用`Array.from` 实现遍历

```js
// 通过Array.from将类数组转为数组
// 第二个参数是一个回调函数，每一个元素都会指向这个回调
let elem = document.querySelector('.container')
	.children;
console.log(elem);
Array.from(elem, function(item) {
	console.log(item)
})
```



### 7-5 展开语法

使用`...` 展开语法转换节点为数组

```js
// 通过...将类数组转为数组然后使用map方法
let elem = document.querySelector('.container')
	.children;
console.log(elem);
[...elem].map((item) => {
	console.log(item)
})
```

