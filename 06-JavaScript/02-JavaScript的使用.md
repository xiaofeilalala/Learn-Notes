# JavaScript 的使用



## 1. Hello World

- 我们可以使用一个 `<script>` 标签将 JavaScript 代码添加到页面中
- `type` 和 `language` 特性（attribute）不是必需的
- 外部的脚本可以通过 `<script src="path/to/script.js"></script>` 的方式插入

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World</title>
    <script>
        document.write("<h1 style='text-align: center;'>Hello World</h1>");
    </script>
</head>

<body>

</body>

</html>
```

document 表示网页文档对象；document.write() 表示调用 Document 对象的 write() 方法，在当前网页源代码中写入 HTML 字符串

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211218175338.png)

## 2. 三种使用引入方式

javascript 有三种使用引入方式：内嵌式、外部引用式、行内式



### 2-1 内嵌式

> **Tips：**type 属性和 language 属性，在现代 JavaScript 中这样的特性声明现在已经不再需要
>
> `type` **特性：**`<script type=…>`
>
> 在老的 HTML4 标准中，要求 script 标签有 `type` 特性。通常是 `type="text/javascript"`。这样的特性声明现在已经不再需要。而且，现代 HTML 标准已经完全改变了此特性的含义。现在，它可以用于 JavaScript 模块。但这是一个高级话题，我们将在本教程的另一部分中探讨 JavaScript 模块
>
> `language` **特性：**`<script language=…>`
>
> 这个特性是为了显示脚本使用的语言。这个特性现在已经没有任何意义，因为语言默认就是 JavaScript。不再需要使用它了

在 HTML 页面中嵌入 JavaScript 脚本需要使用 `<script>` 标签，开发人员可以在 `<script>` 标签中直接编写 JavaScript 代码

```html
<script>
    document.write("hello world");
</script>
```



### 2-2 外部引入式

JavaScript 程序不仅可以直接放在 HTML 文档中，也可以放在 JavaScript 脚本文件中。JavaScript 脚本文件是文本文件，扩展名为`.js`，使用任何文本编辑器都可以编辑

* 脚本文件可以通过 `src` 特性（attribute）添加到 HTML 文件中
* `src` 属性可以为相对路径、绝对路径、或者完整的网络路径
* **如果设置了 `src` 特性，`script` 标签内容将会被忽略。**一个单独的 `<script>` 标签不能同时有 `src` 特性和内部包裹的代码
* 如需要引入多个 `.js` 文件可以使用多个`<script>` 标签

```html
<!-- 内嵌式 -->
<!-- 通过scriptt标签插入到html文档内 现代javascript中不需要再声明type和language属性-->
<script>
    document.write("<h1 style='text-align: center;'>Hello World</h1>");
</script>

<!-- 外部引用 -->
<!-- 通过script标签的src属性引用外部js文件，src路径可以为相对、绝对、网络路径 -->
<!-- 如果一个script标签设置了src属性，那么script标签内书写的代码会被忽略 -->
<script src="myjs.js"></script>
<script src="myjs.js">
    // 这段js代码被忽略
    document.write("<h1 style='text-align: center;'>Hello World</h1>");
</script>
```



### 2-3 行内式

行内引入 javascript 的方式必须结合事件来使用,内部 js 和外部 js 可以不结合事件

在行内 js 写法中，推荐使用单引号，防止和原本 html 的双引号冲突

```html
<!-- 行内式：通过事件结合使用，js推荐使用单引号，html中的代码推荐使用双引号 -->
<h1 onclick="alert('Hello world')" style="text-align: center;">Hello world</h1>
```



## 3. 代码结构



### 3-1 语句

语句是执行行为（action）的语法结构和命令。

我们已经见过了 `alert('Hello world!')` 这样可以用来显示消息的语句

我们可以在代码中编写任意数量的语句。语句之间可以使用分号进行分割

```js
console.log("hello world");
alert("hello world");
```



### 3-2 分号

> **Tips：**在大多数情况下，换行意味着一个分号。但是“大多数情况”并不意味着“总是”！

当存在换行符（line break）时，在大多数情况下可以省略分号

当两条语句出现合并无法解析时，即使语句被换行符分隔了我们依然建议在它们之间加分号

```js
console.log(3+
2+
1);
// 6

console.log("hello world");
console.log("hello world");
```

```js
// 错误示例
console.log("hello world")
[1, 2].forEach(value => {
    console.log(value)
});

// JavaScript 引擎并没有假设在方括号 [...] 前有一个分号。因此，最后一个示例中的代码被视为了单个语句
console.log("hello world")[1, 2].forEach(value => {console.log(value)});
```



### 3-3 注释

**单行注释以两个正斜杠字符 `//` 开始**

```js
// 单行注释以双斜杠//开头，//之后的所有内容都会看作是注释的内容，对//之前的内容则不会产生影响
console.log("hello world"); // 单行注释
```



**多行注释以一个正斜杠和星号开始 `“/\*”` 并以一个星号和正斜杠结束 `“\*/”`**

> **Tips：**不支持注释嵌套！不要在 /*...*/ 内嵌套另一个 /*...*/

```js
/* console.log("hello world");
console.log("hello world"); */
```



**vscode 热键：**

* 单行注释：`ctrl` + `/`
* 多行注释：`shift` + `alt` + `A`



## 4. 输入输出语句

* `alert()`：模态窗，弹出提示框
* `prompt`：显示一个带有文本消息的模态窗口，还有 input 框和确定/取消按钮
* `confirm`：显示一个带有问题以及确定和取消两个按钮的模态窗口
* `console.log()`：写入到浏览器的控制台
* `document.write()`：将内容写到 HTML 文档中
* `innerHTML`：写入到浏览器的控制台



### 4-1 alert() 函数

可以在浏览器中弹出一个提示框，在提示框中我们可以定义要输出的内容

`alert()` 函数是 window 对象下的一个函数

```js
alert("hello world");
```



### 4-2 prompt 函数

浏览器会显示一个带有文本消息的模态窗口，还有 input 框和确定/取消按钮

`prompt` 将返回用户在 `input` 框内输入的文本，如果用户取消了输入，则返回 `null`

* `prompt` 函数接收两个参数
* `title` 显示给用户的文本
* `default` 可选的第二个参数，指定 input 框的初始值
* 第二个参数是可选的。但是如果我们不提供的话，**IE 浏览器会提供默认值**，值为 undefined

```js
var result = prompt(title, [defult]);
```



### 4-3 confirm 函数

显示一个带有 `question` 以及确定和取消两个按钮的模态窗口

点击确定返回 `true`，点击取消返回 `false`

```js
var result = confirm(question);
```



### 4-4 console.log() 方法

在浏览器的控制台输出信息，通常使用 console.log() 来调试程序

```js
console.log();
```



### 4-5 document.write() 方法

`document.write()` 方法将一个文本字符串写入一个由 `document.open()`打开的文档流

执行 `doucment.write()` 函数会自动调用 `document.open()` 函数创建一个新的文档流，并写入新的内容，再通过浏览器展现，这样就会覆盖原来的内容

**`document.write` 调用只在页面加载时工作。**如果我们稍后调用它，则现有文档内容将被擦除

`document.close()` 函数只能够关闭由`document.open()` 函数创建的文档流

* `document.open()`：打开文档流
* `document.close()`：关闭文档流 

```js
document.open();
document.close();
document.write();
```



### 4-6 innerHTML

innerHTML 属性可以返回或设置元素中的内容

设置 `innerHTML` 的值可以让你轻松地将当前元素的内容替换为新的内容

```js
document.body.innerHTML = "Hello World";
/* 属性删除 body 的全部内容将内容替换为 Hello World */
```

