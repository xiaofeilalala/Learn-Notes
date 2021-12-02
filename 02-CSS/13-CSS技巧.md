# CSS技巧



## 1. css初始化

不同浏览器对有些标签的默认值是不同的，为了消除不同的浏览器对 HTML 文本呈现的差异，兼容浏览器需要对 css 初始化

* 使用 github 上超火的 css 初始化库 Normalize.css 使浏览器呈现所有 HTML 元素更加一致，并且符合现代 web 标准。
* Normalize.css 只作用于需要规范化的样式
  * cdn 引入  `<link href="https://cdn.bootcdn.net/ajax/libs/normalize/8.0.1/normalize.css" rel="stylesheet">`
  * 文件下载地址 `https://necolas.github.io/normalize.css/8.0.1/normalize.css`

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211125221615.png)



## 2. 字体图标

常见字体图标库：iconfont 阿里巴巴矢量图标库、font awesome v4/v5、iconmoon、

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211202184301.png)

使用步骤如下：

第一步：打开 iconfont 阿里巴巴矢量图标库，选择自己想要的图标加入项目然后本地下载文件

第二步：解压下载的 .zip 文件，打开 demo_index.html 文件

第三步：引入文件到自己的项目中，有三种引入方式 Unicode 引入、Font class 引入、Symbol 引入



### 2-1 Font class 引入 (常用)

font-class 是 Unicode 使用方式的一种变种，主要是解决 Unicode 书写不直观，语意不明确的问题

与 Unicode 使用方式相比，具有如下特点：

- 相比于 Unicode 语意明确，书写更直观。可以很容易分辨这个 icon 是什么
- 因为使用 class 来定义图标，所以当要替换图标时，只需要修改 class 里面的 Unicode 引用



使用步骤如下：

第一步：引入项目下面生成的 fontclass 代码：

```html
<link rel="stylesheet" href="./iconfont.css">
```

第二步：挑选相应图标并获取类名，应用于页面：

```html
<span class="iconfont icon-meh-filling"></span>
```

> " iconfont" 是你项目下的 font-family。可以通过编辑项目查看，默认是 "iconfont"



### 2-2 Unicode 引入

Unicode 是字体在网页端最原始的应用方式，特点是：

- 支持按字体的方式去动态调整图标大小，颜色等等
- 默认情况下不支持多色，直接添加多色图标会自动去色

> 注意：新版 iconfont 支持两种方式引用多色图标：SVG symbol 引用方式和彩色字体图标模式。（使用彩色字体图标需要在「编辑项目」中开启「彩色」选项后并重新生成。）



Unicode 使用步骤如下：

第一步：拷贝项目下面生成的 `@font-face`

```css
@font-face {
  font-family: 'iconfont';
  src: url('iconfont.woff2?t=1638446867490') format('woff2'),
       url('iconfont.woff?t=1638446867490') format('woff'),
       url('iconfont.ttf?t=1638446867490') format('truetype');
}
```

第二步：定义使用 iconfont 的样式

```css
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

第三步：挑选相应图标并获取字体编码，应用于页面

```html
<span class="iconfont">&#xe68b;</span>
```

> "iconfont" 是你项目下的 font-family。可以通过编辑项目查看，默认是 "iconfont"



### 2-3 Symbol 引入

这是一种全新的使用方式，应该说这才是未来的主流，也是平台目前推荐的用法。相关介绍可以参考这篇[文章]() 这种用法其实是做了一个 SVG 的集合，与另外两种相比具有如下特点：

- 支持多色图标了，不再受单色限制。
- 通过一些技巧，支持像字体那样，通过 `font-size`, `color` 来调整样式。
- 兼容性较差，支持 IE9+，及现代浏览器。
- 浏览器渲染 SVG 的性能一般，还不如 png。

使用步骤如下：



第一步：引入项目下面生成的 symbol 代码：

```html
<script src="./iconfont.js"></script>
```

第二步：加入通用 CSS 代码（引入一次就行）：

```html
<style>
.icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>
```

第三步：挑选相应图标并获取类名，应用于页面：

```html
<svg class="icon" aria-hidden="true">
  <use xlink:href="#icon-meh-filling"></use>
</svg>
```



## 3. 字体引用

字体引入是通过  `@font-face` 属性来指定一个用于显示文本的自定义字体



### 3-1 属性介绍

```css
/* 语法 */
@font-face {
      font-family: <YourDefineFontName>;
      src: <url> [<format>],[<source> [<format>]];
      [font-weight: <weight>];
      [font-style: <style>];
}
/* 带中括号的属性为非必须选项 */
```

* **font-family**：为载入的字体取名字

* **src**：

  * url：加载字体，可以是相对路径，可以是绝对路径，也可以是网络地址

  * format：定义的字体的格式，用来帮助浏览器识别。主要取值为：truetype(.ttf)、opentype(.otf）、truetype-aat、embedded-opentype(.eot)、svg(.svg)、woff(.woff)

* **font-weight**：定义加粗样式

* **font-style**：定义字体样式

```css
/* format对应字体格式 以及 常见兼容性写法 */
@font-face {
  font-family: 'MyName';
  src: url('../fonts/xxx.eot');
  src: url('../fonts/xxx.eot?#iefix') format('embedded-opentype'),
       url('../fonts/xxx.woff') format('woff'),
       url('../fonts/xxx.ttf') format('truetype'),
       url('../fonts/xxx.svg#defineName') format('svg');
  font-weight: normal;
  font-style: normal;
}
```



**#iefix有何作用？**

IE9 之前的版本没有按照标准解析字体声明，当 src 属性包含多个 url 时，它无法正确的解析而返回 404 错误，而其他浏览器会自动采用自己适用的 url。因此把仅 

IE9 之前支持的 EOT 格式放在第一位，然后在 url 后加上 ?，这样 IE9 之前的版本会把问号之后的内容当作 url 的参数。至于 #iefix 的作用，一是起到了注释的作

用，二是可以将 url 参数变为锚点，减少发送给服务器的字符



**为何有两个src？**

绝大多数情况下，第一个 src 是可以去掉的，除非需要支持 IE9 下的兼容模式。在 IE9 中可以使用 IE7 和 IE8 的模式渲染页面，微软修改了在兼容模式下的 CSS 解

析器，导致使用 ? 的方案失效。由于 CSS 解释器是从下往上解析的，所以在上面添加一个不带问号的 src 属性便可以解决此问题



### 3-2 兼容性

**IE6-8** 仅支持 embedded-opentype（.eot）

**firefox3.5** 支持truetype（.ttf）、opentype（.otf）

**firefox3.6** 支持truetype（.ttf）和opentype（.otf）、WOFF（.woff）

**chrome **支持truetype（.ttf）、opentype（.otf）、WOFF（.woff）、svg（.svg）

**safari **支持truetype（.ttf）、opentype（.otf）、WOFF（.woff）、svg（.svg）

**opera** 支持truetype（.ttf）、opentype（.otf）、WOFF（.woff）、svg（.svg）



### 3-3 格式介绍

目前最主要的几种网络字体(web font)格式包括WOFF，SVG，EOT，OTF/TTF

**WOFF**

WOFF是Web Open Font Format几个词的首字母简写。这种字体格式专门用于网上，由Mozilla联合其它几大组织共同开发。WOFF字体通常比其它字体加载的要

快些，因为使用了OpenType (OTF)和TrueType (TTF)字体里的存储结构和压缩算法。这种字体格式还可以加入元信息和授权信息。这种字体格式有君临天下的趋

势，因为所有的现代浏览器都开始支持这种字体格式。

**SVG / SVGZ**

Scalable Vector Graphics (Font). SVG是一种用矢量图格式改进的字体格式，体积上比矢量图更小，适合在手机设备上使用。

**EOT**

Embedded Open Type。这是微软创造的字体格式。这种格式只在IE6-IE8里使用。

**OTF / TTF**

OpenType Font 和 TrueType Font。部分的因为这种格式容易被复制(非法的)，这才催生了WOFF字体格式。然而，OpenType有很多独特的地方，受到很多设计

者的喜爱



![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211202210100.png)

## 4. title 图标

设置网页图标是通过在 `<head>`  标签中添加 `<link>` 标签设置 link 标签属性

在网页标题左侧显示：`<link rel="icon" href="图标地址" type="image/x-icon">`

在收藏夹显示图标：`<link rel="shortcut icon" href="图标地址" type="image/x-icon">`

```html
<head>
    <link rel="icon" href="图标地址" type="image/x-icon">
    <link rel="shortcut icon" href="图标地址" type="image/x-icon">
</head>
```



type 属性可以指定不同的文件类型（MIME 类型）

图片格式转换网站链接：[Convert image format Online - Image Converter Online - 100% free (jinaconvert.com)](https://jinaconvert.com/)

| 缩写                                                         | 文件格式                                                   | MIME 类型       | 文件拓展名                                 | 浏览器兼容性                                            |
| :----------------------------------------------------------- | :--------------------------------------------------------- | :-------------- | :----------------------------------------- | :------------------------------------------------------ |
| [APNG](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#apng) | Animated Portable Network Graphics **动态便携式网络图像**  | `image/apng`    | `.apng`                                    | Chrome, Edge, Firefox, Opera, Safari                    |
| [AVIF](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#avif) | AV1 Image File Format AV1 图像文件格式                     | `image/`avif    | `.`avif                                    | Chrome, Opera, Firefox (feature flag)                   |
| [BMP](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#bmp) | Bitmap file **位图**文件                                   | `image/bmp`     | `.bmp`                                     | Chrome, Edge, Firefox, Internet Explorer, Opera, Safari |
| [GIF](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#gif) | Graphics Interchange Format 图像互换格式                   | `image/gif`     | `.gif`                                     | Chrome, Edge, Firefox, Internet Explorer, Opera, Safari |
| [ICO](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#ico) | Microsoft Icon 微软图标                                    | `image/x-icon`  | `.ico`, `.cur`                             | Chrome, Edge, Firefox, Internet Explorer, Opera, Safari |
| [JPEG](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#jpeg) | Joint Photographic Expert Group image 联合影像专家小组图像 | `image/jpeg`    | `.jpg`, `.jpeg`, `.jfif`, `.pjpeg`, `.pjp` | Chrome, Edge, Firefox, Internet Explorer, Opera, Safari |
| [PNG](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#png) | Portable Network Graphics **便携式网络图像**               | `image/png`     | `.png`                                     | Chrome, Edge, Firefox, Internet Explorer, Opera, Safari |
| [SVG](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#svg) | Scalable Vector Graphics **可缩放矢量图形**                | `image/svg+xml` | `.svg`                                     | Chrome, Edge, Firefox, Internet Explorer, Opera, Safari |
| [TIFF](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#tiff) | Tagged Image File Format 标签图像文件格式                  | `image/tiff`    | `.tif`, `.tiff`                            | Safari                                                  |
| [WebP](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#webp) | Web Picture format 万维网图像格式                          | `image/webp`    | `.webp`                                    | Chrome, Edge, Firefox, Opera, Safari                    |



## 5. 垂直居中



## 6. 布局技巧



## 7. 精灵图



## 8. css三角形



## 9. 字体溢出省略



## 10. 鼠标样式



## 11. 文本域样式属性



## 12. 图片底部空白





