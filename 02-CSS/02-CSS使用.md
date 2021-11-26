# CSS 使用



## 1. css 样式表

- 外部样式表（External style sheet）
  * 采用`<link>`标签
  * 采用 @import，必须写在`<style>`标签中，并且必须在第一行书写
  
- 内部样式表（Internal style sheet）
  * 在页面的 head 里采用`<style>` 标签。范围针对此页面
- 内联样式（Inline style）
  * 在某个特定的标签里采用 style 属性。范围只针对此标签



## 2. 外部样式表

外部样式在 HTML 页面 `<head>` 部分内的 `<link>` 元素中进行定义，通过 `<link>` 标签的 `href` 属性引用你的文件系统中的一个 .css 文件

每张 HTML 页面必须在 head 部分的 `<link>` 元素内包含对外部样式表文件的引用

也可以通过 @import 引用外部样式表，必须写在`<style>`标签中，并且必须在第一行书写

```html
<!-- 通过link标签引入 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>创建样式表</title>
    <!-- 外部样式表 -->
    <link rel="stylesheet" href="mystyle.css">
</head>
<body>
    <h1>外部样式表</h1>
</body>
</html>
```

```html
<!-- 通过@import方法引入 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@import引用外部样式表</title>
    <style type="text/css">
        /* @import语法引用 */
        @import url("mystyle.css");
    </style>
</head>
<body>
    <h1>@import引用外部样式表</h1>
</body>
</html>
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211021230824.png)



## 3. 什么是外部样式表

外部样式表是指将 CSS 编写在扩展名为`.css` 的单独文件中，并从HTML`<link>` 元素引用它

```css
@charset "utf-8";

/**
 * @desc mystyle.css
 * @author jsx
 * @date 2021-10-19
 */

/* mystyle.css */
body {
    color: #fff;
    font-size: 16px;
}

h1 {
    text-align: center;
}
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211019210926.png)



## 4. 内部样式表

* 内部样式表的范围只针对此页面，可以对单个页面的样式进行统一设置
* 内部样式在 HTML 页面的 `<head>` 部分内的 `<style>` 元素中进行定义
* `<style>` 标签有一个属性 type 表示类型，值为 text/css 指示内容是标准的 CSS

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>内部样式表</title>
    <!-- 内部样式表 -->
    <style type="text/css">
        h1 {
            text-align: center;
            font-size: 18px;
            color: #4396db;
        }
    </style>
</head>
<body>
    
</body>
</html>
```

有的时候，这种方法会比较有用（比如你使用的内容管理系统不能直接编辑CSS文件)，但该方法和外部样式表比起来更加低效 。在一个站点里，你不得不在每个页面里重复添加相同的CSS，并且在需要更改CSS时修改每个页面文件。



## 5. 行内样式表

* 内联样式表存在于HTML元素的 style 属性之中。其特点是每个CSS表只影响一个元素
* 如需使用行内样式，请将 style 属性添加到相关元素（style 属性可包含任何 CSS 属性）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>行内样式表</title>
</head>
<body>
    <!-- 行内样式表 -->
    <h1 style="display: block; color: #4396db; text-align: center;">行内样式表</h1>
</body>
</html>
```

> **除非你有充足的理由，否则不要这样做！**它难以维护（在需要更新时，你必须在修改同一个文档的多处地方），并且这种写法将文档结构和文档表现混合起来了，这使得代码变得难以阅读和理解。将不同类型的代码分开存放在不同的文档中，会让我们的工作更加清晰。

