# iframe 框架



## 1. iframe 框架介绍

iframe 标签规定一个内联框架


一个内联框架被用来在当前 HTML 文档中嵌入另一个文档。

```html
<iframe></iframe>
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211009180429.png)



## 2. 使用注意事项

* 您可以把需要的文本放置在 `<iframe>` 和 `</iframe>` 之间，这样就可以应对不支持 `<iframe>` 的浏览器
* 使用 CSS 为 `<iframe>` （包括滚动条）定义样式



## 3. iframe 属性

| 属性        | 属性值              | 描述                                            |
| ----------- | ------------------- | ----------------------------------------------- |
| width       | pixels              | 规定 iframe 宽度                                |
| height      | pixels              | 规定 iframe 高度                                |
| frameborder | 0<br/>1             | 规定是否显示 iframe 周围的边框 **HTML5 不支持** |
| name        | text                | 规定 iframe 名称                                |
| src         | url                 | 规定在 iframe 中显示的文档的 URL                |
| scrolling   | yes<br/>no<br/>auto | 规定是否在 iframe 中显示滚动条 **HTML5 不支持** |

```html
<!-- iframe框架属性 -->
<!-- width 宽度 -->
<!-- height 高度 -->
<!-- name 名称 -->
<!-- src 显示文档的url -->
<!-- scrolling 是否显示滚动条 -->
<!-- frameborder 是否显示边框 -->
<iframe name="myiframe" src="https://www.w3school.com.cn/" width="500" height="400" scrolling="yes" frameborder="0"></iframe>
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20211009225339.png)
