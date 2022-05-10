# HTML5 简介

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220504174726.png)



## 1. 什么是 HTML5？

`HTML5` 并不仅仅只是做为 `HTML` 标记语言的一个最新版本，更重要的是它制定了 `Web` 应用开发的一系列标准，成为第一个将 `Web` 做为应用开发平台的 `HTML` 语言

`HTML5` 定义了一系列新元素，如新语义标签、智能表单、多媒体标签等，可以帮助开发者创建富互联网应用，还提供了一些 `Javascript API`，如地理定位、重力感应、硬件访问等，可以在浏览器内实现类原生应用。我们甚至可以结合 `Canvas` 开发网页版游戏

`HTML5` 代表浏览器端技术的一个发展阶段，在这个阶段浏览器的呈现技术得到了飞跃发展和广泛支持，它包括：`HTML5`、`CSS3`、`Javascript API` 在内的一套技术组合

`HTML5`是新一代开发 `Web` 富客户端应用程序整体解决方案，具有很强的交互性和体验的客户端程序

* `HTML5` 是最新的 `HTML` 标准
* `HTML5` 是专门为承载丰富的 `web` 内容而设计的，并且无需额外插件
* `HTML5` 拥有新的语义、图形以及多媒体元素
* `HTML5` 提供的新元素和新的 `API` 简化了 `web` 应用程序的搭建
* `HTML5` 是跨平台的，被设计为在不同类型的硬件（PC、平板、手机、电视机等等）之上运行



## 2. HTML5新增

### 2-1 浏览器支持

最新版本的 `Safari`、`Chrome`、`Firefox` 以及 `Opera` 支持某些 `HTML5` 特性，`Internet Explorer 9` 将支持某些 `HTML5` 特性

`IE9` 以下版本浏览器兼容 `HTML5` 的方法，使用本站的静态资源的 `html5shiv` 包

引用 `shiv` 代码的链接必须位于 `<head>` 元素中，因为 `Internet Explorer` 需要在读取之前认识所有新元素

| 浏览器          | 正式版本 | 分数 |
| --------------- | -------- | ---- |
| Google Chrome   | 68       | 528  |
| Opera           | 55       | 528  |
| Microsoft Edge  | 18       | 496  |
| Mozilla Firefox | 62       | 497  |
| Apple Safari    | 11.2     | 477  |

```html
<!--[if lt IE 9]>
    <script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script>
<![endif]-->
```

```css
/*html5*/
article,aside,dialog,footer,header,section,nav,figure,menu{ display:block }
```



### 2-2 文档声明

`HTML5` 的新的文档类型（DOCTYPE）声明非常简单，`HTML5` 中默认的字符编码是 `UTF-8`

`<!doctype>` 声明必须位于 `HTML5` 文档中的第一行

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>文档标题</title>
</head>
 
<body>
文档内容......
</body>
 
</html>
```



### 2-3 新特性

`video`、`audio` 和 `canvas` 的加入可以减少浏览器对于插件的需求，而自己实现丰富的网络应用服务，用户可以更容易地在网页中添加和处理多媒体和图片内容

语义化的标签和新属性的添加更丰富了文档的数据内容，有利于搜索引擎的索引整理、小屏幕设备和视障人士使用

`HTMl5` 的存储，更加的安全与快速，这些数据不会被保存在服务器上，这些数据只用于用户请求网站数据上，并且它可以存储大量的数据，而不影响网站的性能

* 新语义化标签的引入，比如 `article`、`footer`、`header`、`nav`、`section` 等
* 新的表单控件，比如数字、日期、时间、日历和滑块 `date`、`time`、`email`、`url`、`search` 等
* 多媒体元素引入音频 `audio` 和视频 `video`
* 绘制图像 `Canvas`、`SVG`
* 对本地离线存储的更好的支持，完全支持 `CSS3`
* `Web`应用，本地 `Web SQL` 数据库、`Web Workers`线程、`localStorage`与`sessionStorage `存储、`websocket`通信
* 地理位置定位、拖放`API`



### 2-4 HTML5模版

> **Tips：**对于中文网页需要使用 `<meta charset="utf-8">` 声明编码，否则会出现乱码

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>文档标题</title>
  </head>

  <body>
    文档内容......
  </body>
</html>
```

* `<!DOCTYPE html>` 声明为 `HTML5` 文档，告诉浏览器正确的 `HTML` 版本，有助于正确显示网页内容（不区分大小写）
* `<html>` `HTML` 页面的根元素
* `<head>` 是页面的头部元素，这些内容包括你想在搜索结果中出现的关键字和页面描述，CSS 样式，字符集声明等等

- `<meta charset="utf-8">` 定义网页编码格式为 `utf-8`
- `<title>` 元素描述了文档的标题，出现在浏览器标签上
- `<body>` 元素包含了可见的页面内容



### 2-5 删除元素

以下 HTML 4.01 元素已从 HTML5 中删除

- `<acronym>` 声明一个字符序列
- `<applet>` 标志着包含了 `Java` 的 `applet`
- `<basefont>` 用来设置文档的默认字体大小
- `<big>` 使字体加大一号
- `<center>` 块级元素，元素的整个内容在它的上级元素中水平居中
- `<dir>` 作为一个文件和/或文件夹的目录的容器
- `<font>` 定义了该内容的字体大小、顏色与表现
- `<frame>` 定义了一个特定区域，另一个 HTML 文档可以在里面展示
- `<frameset>` 是一个用于包含 `frame` 的 HTML 元素
- `<noframes>` 用于支持不支持  `frame` 元素的浏览器
- `<strike>` 在文本上放置删除线
- `<tt>` 产生一个内联元素，使用浏览器内置的 monotype 字体展示
