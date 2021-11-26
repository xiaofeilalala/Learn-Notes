# HTML基本结构



## 1. HTML结构代码展示

```html
<!DOCTYPE HTML> <!-- HTML5标准网页声明 -->
<HTML> <!-- HTML为根标签，代表整个网页 -->

<head> <!-- head为头部标签，一般用来描述文档的各种属性和信息， 包括标题等-->
  <meta charset="UTF-8"> <!-- 设置字符集为utf-8 -->
  <title>my HTML</title> <!-- 设置浏览器的标题 -->
</head>

<!-- 网页所有的内容都写在body标签内 -->
<body> 
  我的第一个HTML网页
</body>

</HTML>
```

> **Tips**：`<!-- -->`为 HTML 文件的注释， 注释的内容写在 `<!-- -->` 内，但不会在页面中显示。



## 2. HTML 文件结构详解

* `<!DOCTYPE HTML>` 标签：

  为文档类型声明，表示该文件为 HTML5 文件。` <!DOCTYPE>` 声明必须是 HTML 文档的第一行，位于 `<HTML>`标签之前。

* `<HTML></HTML>`标签对：

  `<HTML>` 标签位于 HTML 文档的最前面，用来标识 HTML 文档的开始； `</HTML>` 标签位于 HTML 文档的最后面，用来标识 HTML 文档的结束；这两个标签对成对存在，中间的部分是文档的**头部**和**主题**。

* `<head></head>` 标签对：

  标签包含有关 HTML 文档的信息，可以包含一些辅助性标签。如 `<title></title>` ，`<link /><meta />` ， `<style></style>` ， `<script></script>` 等，**但是浏览器除了会在标题栏显示 `<title>` 元素的内容外，不会向用户显示 `head` 元素内的其他任何内容**。

* `<body></body>` 标签对：

  它是 HTML 文档的主体部分，在这个标签中可以包含 `<p><h1><br>` 等众多标签，`<body>` 标签出现在 `</head>` 标签之后，且必须在闭标签 `</HTML>` 之前闭合。



## 3. HTML <!DOCTYPE> 声明

`<!DOCTYPE>` 声明必须是 HTML 文档的第一行，位于` <html> </html>`标签之前。

`<!DOCTYPE>` 声明没有结束标签，对大小写不敏感。

`<!DOCTYPE>` 声明不是 HTML 标签；它是指示 web 浏览器关于页面使用哪个 HTML 版本进行编写的指令。

> Tips：请始终向 HTML 文档添加 `<!DOCTYPE>` 声明，这样浏览器才能获知文档类型。



### 3-1 不同版本的 !DOCTYPE 声明

在 HTML 4.01 中，`<!DOCTYPE>` 声明引用 DTD，因为 HTML 4.01 基于 SGML。DTD 规定了标记语言的规则，这样浏览器才能正确地呈现内容。

HTML5 不基于 SGML，所以不需要引用 DTD。

在 HTML 4.01 中有三种 `<!DOCTYPE>` 声明。在 HTML5 中只有一种：

| HTML版本  |       声明模式        |                             描述                             |
| :-------: | :-------------------: | :----------------------------------------------------------: |
|   HTML5   |           /           |              HTML5 不基于 SGML，不需要引用 DTD               |
|     /     |    Strict 严格模式    | 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。 |
| HTML 4.01 | Transitional 普通模式 | 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。 |
|     /     |   Frameset 框架模式   | 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。允许框架集内容。 |
|     /     |    Strict 严格模式    | 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。 |
| XHTML 1.0 | Transitional 普通模式 | 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。 |
|     /     |   Frameset 框架模式   | 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。允许框架集内容。必须以格式正确的 XML 来编写标记。 |
| XHTML 1.1 |           /           | 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。允许添加模型（例如提供对东亚语系的 ruby 支持） |



**HTML 5**

```html
<!DOCTYPE html>
```



**HTML 4.01 Strict**

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```



**HTML 4.01 Transitional**

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```



**HTML 4.01 Frameset**

该 DTD 等同于 HTML 4.01 Transitional，但允许框架集内容。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```



**XHTML 1.0 Strict**

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```



**XHTML 1.0 Transitional**

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```



**XHTML 1.0 Frameset**

该 DTD 等同于 XHTML 1.0 Transitional，但允许框架集内容。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```



**XHTML 1.1**

该 DTD 等同于 XHTML 1.0 Strict，但允许添加模型（例如提供对东亚语系的 ruby 支持）。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```



## 4. lang 属性

属性 lang 是英语 Language 的缩写，规定元素内容的语言。

```html
<html lang="en">
```

最常见的语言类型有两种：

- en：定义页面语言为英语。
- zh-CN：定义页面语言为中文。



### 4-1 lang 属性的作用

- 根据根据 lang 属性来设定不同语言的css样式，或者字体。
- 告诉搜索引擎做精确的识别。
- 让语法检查程序做语言识别。
- 帮助翻译工具做识别。
- 帮助网页阅读程序做识别。



搜索引擎首先自己无法判断自己抓取的页面中的内容是什么语言，因为在它看来都是二进制文件，那么这时就需要我们告诉它这个页面中的内容是什么语言，进而它才能知道下一步该干嘛，也就是说，当你把 `lang` 设置为 `"en" `时，无论你网页中是什么语言的内容，在它看来都是英语，如果本地浏览器的默认语言不是英语，就会提示上面的选项，问您是否需要翻译。



## 5. head 标签

> Tips：应该把 `<head>` 标签放在文档的开始处，紧跟在 `<html>` 后面，并处于 `<body>` 标签或 `<frameset>` 标签之前。
>
> 请记住始终为文档规定标题！

`<head>` 标签用于定义文档的头部，它是所有头部元素的容器。`<head> `中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。

文档的头部描述了文档的各种属性和信息，包括文档的标题、在 Web 中的位置以及和其他文档的关系等。绝大多数文档头部包含的数据都不会真正作为内容显示给读者。

`<meta>`、`<title>`、`<base>`、`<style>`、`<link>`、`<script>` 这些标签可用在 head 部分。

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210601195059.png)



## 6. meta 标签

`<meta>` 标签提供了 HTML 文档的元数据。

`<meta>` 标签位于文档的头部，不包含任何内容，不会显示在客户端，但是会被浏览器解析。

`<meta>` 标签的属性定义了与文档相关联的名称/值对。

`<meta>` 标签共有四个属性，它们分别是scheme属性、 http-equiv 属性和 name 属性，在 HTML5 中，有一个新的 charset 属性，它使字符集的定义更加容易，不同的属性又有不同的参数值，这些不同的参数值就实现了不同的网页功能。



### 6-1 什么是元数据？

元数据（Metadata）是数据的数据信息。元数据可以被使用浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他 Web 服务调用。

`meta` 标签通常用于指定网页的描述，关键词，文件的最后修改时间，作者及其他元数据。



### 6-2 字符集 charset

字符集用 `meta` 标签中的 `charset` 定义，`charset `就是character set（即“字符集”），即**网页的编码方式**。

**字符集**（Character set）是多个字符的集合。计算机要准确的处理各种字符集文字，需要进行字符编码，以便计算机能够识别和存储各种文字。

`meta` 标签中一定要定义 `charset` 属性，否则可能导致乱码。比如你保存的时候，`meta`写的和声明的不匹配，那么浏览器就是乱码。



HTML 4.01：

```html
<meta http-equiv="content-type" content="text/html; charset=UTF-8" />
```



HTML5：

```html
<meta charset="UTF-8" />
```



UTF-8 是目前最常用的字符集编码方式，常用的字符集编码方式还有 GBK 和 GB2312 等。

| charset 信息参数 |    编码方式    |
| ---------------- | :------------: |
| GB2312（或GBK）  |    简体中文    |
| BIG5             |    繁体中文    |
| iso-2022-jp      |      日文      |
| ks_c_5601        |      韩文      |
| ISO-8859-1       |      英文      |
| UTF-8            | 世界通用的语言 |

```html
<meta chraset="UTF-8" />
```



### 6-3 scheme 属性

> Tips：HTML5 不支持 `<meta>` scheme 属性。

`scheme` 属性规定用于翻译 `content` 属性的值的方案（格式或 URI）。

```html
<meta scheme="ISBN" name="identifier" content="0-14-XXXXXX-1" >
```



### 6-4 name 属性

`name` 属性主要用于描述网页，与之对应的属性值为 `content`，`content` 中的内容主要是便于搜索引擎机器人查找信息和分类信息用的。



meta 标签的 name 属性语法格式是：

```html
<meta name="参数" content="具体的参数值">
```



keywords：关键字，告诉搜索引擎该网页的关键字。

```html
<!-- keywords -->
<meta name="keywords" content="关键字，可以有多个关键字" />
```



description：网站内容的描述，用于告诉搜索引擎你网站的主要内容，有助于 SEO 搜索引擎优化。

```html
<!-- description -->
<meta name="description" content="对网站内容的描述" />
```



viewport：窗口视图。

* width：设置 ***layout viewport*** 的宽度，为一个正整数，或字符串`"width-device"`。

* initial-scale：设置页面的初始缩放值，为一个数字，可以带小数。

* minimum-scale：允许用户的最小缩放值，为一个数字，可以带小数。

* maximum-scale：允许用户的最大缩放值，为一个数字，可以带小数。

* height：设置 ***layout viewport*** 的高度，这个属性对我们并不重要，很少使用

* user-scalable：是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes 代表允许。

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```



robots：定义搜索引擎爬虫的索引方式 robots 用来告诉爬虫哪些页面需要索引，哪些页面不需要索引。

* all：文件将被检索，且页面上的链接可以被查询，默认为all。
* none：文件将不被检索，且页面上的链接不可以被查询。
* index：文件将被检索。
* follow：页面上的链接可以被查询。
* noindex：文件将不被检索，但页面上的链接可以被查询。
* nofollow：文件将被检索，但页面上的链接不可以被查询。

```html
<!-- robots -->
<meta name="robos" content="all" />
```



author：标注网页的作者。

```html
<meta name="author" content="网页的作者">
```



generator：网页制作软件。

```html
<meta name="generator" content="制作软件">
```



copyright：说明网站版权信息。

```html
<meta name="copyright" content="版权">
```



### 6-5 http-equiv 属性

http-equiv 相当于 http 的文件头作用，它可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容。



Content-Type：设定网页的字符集，html4.01 用法不推荐。

```html
<!-- html4.01 版本 -->
<meta http-equiv="content-type" content="text/html; charset=UTF-8" />
```



Expires：期限，可以用于设定网页的到期时间。一旦网页过期，必须到服务器上重新传输。

```html
<meta http-equiv="expires" content="Fri,12 Jan 2022 18:18:18 GMT">
```



Pragma：cache 模式，是用于设定禁止浏览器从本地机的缓存中调阅页面内容，设定后一旦离开网页就无法从 cache 中再调出。

* cache模式：允许脱机浏览。
* no-cache模式：无法脱机浏览。

```html
<!-- 当前无法脱机浏览 -->
<meta http-equiv="peragma" content="no-cache" />
```



Refresh：刷新，自动刷新并指向新页面。

```html
<meta http-equiv="refresh" content="时间（秒为单位）;URL">
<!-- 停留2秒钟后自动刷新到URL网址 -->
```



Cache-Control：请求和响应遵循的缓存机制。



Set-Cookie：cookie设定。

如果网页过期，那么存盘的cookie将被删除

```html
<!-- 必须使用GMT的时间格式 -->
<meta http-equiv="set-cookie" content="cookievalue=xxx; expires=Friday,12-Jan-2021 22:36:18 GMT; path=/">
```



Content-Language：显示语言的设定。

```html
<meta http-equiv="content-language" content="zh-cn" />
```



Window-Target：显示窗口的设定。

Window-Target 有四个属性值：

* _top：表示页面以当前整个窗口显示，可以防止自己的页面被其他网页嵌套
* _blank： 表示页面以新打开的窗口显示
* _parent：表示页面以父容器或窗口显示
* _self：表示页面以当前容器或窗口显示

```html
<meta http-equiv="window-target" content="_top">
```



Imagetoolbar：指定是否显示图片工具栏，当为 false 代表不显示，当为true代表显示。

```html
<meta http-equiv="imagetoolbar" content="false" />
```



## 7. meta 标签中包含的其他标签



### 7-1 title 标签

`<title>` 用于设置网页标题，title 标签也是有助于 SEO 搜索引擎优化的。

```html
<title>网页标题</title>
```



### 7-2 base 标签

`<base>`  标签为页面上的所有链接规定默认路径或默认目标。指定之后，所有的 a 链接都是以这个路径为基准。

```html
<base href="" />
```



### 7-3 style 标签

`<style>` 标签用于为 HTML 文档定义样式信息。

type 属性是必需的，定义 style 元素的内容。唯一可能的值是 `"text/css"`。

```html
<style type="text/css"></style>
```



### 7-4 link 标签

`<link>`标签定义文档与外部资源的关系。最常见的用途是链接样式表。

```html
<link rel="stylesheet" type="text/css" href="">
```



### 7-5 script 标签

`<script>` 标签用于定义客户端脚本，比如 JavaScript。


script 标签既可以包含脚本语句，也可以通过 src 属性指向外部脚本文件。

必需的 type 属性规定脚本的 MIME 类型。

```html
<!-- 直接写入脚本 -->
<script type="text/javascript"></script>

<!-- 通过src属性外部引入脚本 -->
<script type="text/javascript" src=""></script>
```



## 8. body 标签

body 元素是定义文档的主体。

body 是用在网页中的一种HTML标签，标签是用在网页中的一种 HTML 标签，表示网页的主体部分，也就是用户可以看到的内容，可以包含文本、图片、音频、视频等各种内容！

```html
<body></body>
```
