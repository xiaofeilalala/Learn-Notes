# BOM 浏览器对象模型

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220413211145.png)

浏览器对象模型（Browser Object Model），简称 `BOM`，表示由浏览器（主机环境）提供的用于处理文档（document）之外的所有内容的其他对象

* `window` 浏览器实例
* `location` 加载文档的信息和常用导航功能实例
* `navigator` 客户端标识和信息的对象实例
* `history` 当前窗口建立以来的导航历史记录
* `screen` 客户端窗口及屏幕信息



## 1. window 对象

`window` 对象是 `BOM` 的核心，是 `js` 访问浏览器的接口，也是 `ES` 规定的 `Global` 全局对象



### 1-1 window.open 弹窗

`window.open(url, name, params)` 

* `url`：要在新窗口中加载的 `URL`
* `name`：新窗口的名称
* `params`：新窗口的配置字符串
  * 新窗口位置:
    - `left/top`（数字）—— 屏幕上窗口的左上角的坐标。这有一个限制：不能将新窗口置于屏幕外（offscreen）
    - `width/height`（数字）—— 新窗口的宽度和高度。宽度/高度的最小值是有限制的，因此不可能创建一个不可见的窗口
  * 新窗口功能：
    - `menubar`（yes/no）—— 显示或隐藏新窗口的浏览器菜单
    - `toolbar`（yes/no）—— 显示或隐藏新窗口的浏览器导航栏（后退，前进，重新加载等）
    - `location`（yes/no）—— 显示或隐藏新窗口的 URL 字段。Firefox 和 IE 浏览器不允许默认隐藏它
    - `status`（yes/no）—— 显示或隐藏状态栏。同样，大多数浏览器都强制显示它
    - `resizable`（yes/no）—— 允许禁用新窗口大小调整。不建议使用
    - `scrollbars`（yes/no）—— 允许禁用新窗口的滚动条。不建议使用

```html
<button class="btn">创建弹窗</button>
<script>    
    // 创建一个弹窗
    let options = `scrollbars=no,
    resizable=no,
    status=no,
    location=no,
    toolbar=no,
    menubar=no,
    width=500,
    height=300,
    left=200,
    top=200`;
    let btn1 = document.querySelector('.btn');
    btn1.addEventListener('click',function() {
        window.open('/', 'test', options);
    })
</script>
```

设置中的省略规则：

- 如果 `open` 调用中没有第三个参数，或者它是空的，则使用默认的窗口参数
- 如果这里有一个参数字符串，但是某些 `yes/no` 功能被省略了，那么被省略的功能则被默认值为 `no`。因此，如果你指定参数，请确保将所有必需的功能明确设置为 `yes`
- 如果参数中没有 `left/top`，那么浏览器会尝试在最后打开的窗口附近打开一个新窗口
- 如果没有 `width/height`，那么新窗口的大小将与上次打开的窗口大小相同



### 1-2 阻止弹窗

如果弹窗是在用户触发的事件处理程序（如 `onclick`）之外调用的，大多数浏览器都会阻止此类弹窗

```js
// 弹窗被阻止
window.open('https://www.baidu.com');

// 弹窗被允许
button.onclick = () => {
  window.open('https://www.baidu.com');
};
```



### 1-3 弹窗与窗口访问

只有在窗口是同源的时，窗口才能自由访问彼此的内容

弹窗也可以使用 `window.opener` 来访问 `opener` 窗口，对其他所有窗口来说，`window.opener` 均为 `null`

```html
<!-- 父窗口 -->
<button class="btn">打开窗口</button>
<script>
    // 创建窗口
    let btn = document.querySelector('.btn');
    let html = `<h3 style="text-align:center;color: #3496db;">Hello World</h3>`

    btn.addEventListener('click', function (e) {
        let newWindow = window.open('/05-JavaScript/26-BOM/05-新窗口.html', 'test', `width=500,height=400`);
        console.log(newWindow.location.href);
        setTimeout(() => {
            if (newWindow.location.href === 'about:blank') {
                return false;
            } else {
                newWindow.document.body.insertAdjacentHTML('afterbegin', html);
            }
        }, 100)
    })

</script>
```

```html
<!-- 子窗口 -->
<h3>新窗口</h3>
<button class="btn">发送信息</button>

<script>
    let btn = document.querySelector('.btn');
    let html = `<h3 style="text-align:center;color: #3496db;">Hello window</h3>`
    btn.addEventListener('click', function (e) {
        window.opener.document.body.insertAdjacentHTML('afterbegin', html)
    })
</script>
```



### 1-4 关闭窗口

* `window.close()`：关闭一个弹窗
* `window.close`：检查一个窗口是否被关闭

`close()` 方法可用于任何 `window`，但是如果 `window` 不是通过 `window.open()` 创建的，那么大多数浏览器都会忽略 `window.close()`。因此，`close()` 只对弹窗起作用

如果窗口被关闭了，那么 `closed` 属性则为 `true`

```html
<button class="btn">打开弹窗</button>
<button class="btn">关闭弹窗</button>
<button class="btn">检测弹窗</button>

<script>
    // window.close() 关闭window.open创建的弹窗
    let btn = document.querySelector('.btn');
    let newWindow;
    btn.addEventListener('click', function (e) {
        newWindow = window.open('/05-JavaScript/26-BOM/05-新窗口.html', 'test', `width=500,height=400`);
        return newWindow
    })


    let btn1 = document.querySelector('.btn:nth-child(2)');
    btn1.addEventListener('click', function (e) {
        newWindow.close();
    })

    // 检测是否关闭
    let btn2 = document.querySelector('.btn:nth-child(3)');
    btn2.addEventListener('click', function (e) {
        console.log(newWindow.closed)
    })
</script>
```



### 1-5 弹窗移动与调整大小

* `window.moveBy(x,y)` —— 将窗口相对于当前位置向右移动 `x` 像素，并向下移动 `y` 像素。允许负值（向上/向左移动）

* `window.moveTo(x,y)` —— 将窗口移动到屏幕上的坐标 `(x,y)` 处

* `window.resizeBy(width,height)` —— 根据给定的相对于当前大小的 `width/height` 调整窗口大小。允许负值

* `window.resizeTo(width,height)` —— 将窗口调整为给定的大小

```html
<button class="btn">打开弹窗</button>
<button class="btn">关闭弹窗</button>
<button class="btn">moveBy</button>
<button class="btn">moveTo</button>
<button class="btn">resizeBy</button>
<button class="btn">resizeTo</button>

<script>
    // window.close() 关闭window.open创建的弹窗
    let btn = document.querySelector('.btn');
    let newWindow;
    btn.addEventListener('click', function (e) {
        newWindow = window.open('/05-JavaScript/26-BOM/05-新窗口.html', 'test', `width=500,height=400`);
        return newWindow
    })

    let btn1 = document.querySelector('.btn:nth-child(2)');
    btn1.addEventListener('click', function (e) {
        newWindow.close();
    })
    
    // 相对于当前位置移动弹窗
    let btn2 = document.querySelector('.btn:nth-child(3)');
    btn2.addEventListener('click', function (e) {
        newWindow.moveBy(100, 100)
    })
    
    // 相对于屏幕移动弹窗
    let btn3 = document.querySelector('.btn:nth-child(4)');
    btn3.addEventListener('click', function (e) {
        newWindow.moveTo(100, 100)
    })
    
    // 相对于当前大小，调整大小
    let btn4 = document.querySelector('.btn:nth-child(5)');
    btn4.addEventListener('click', function (e) {
        newWindow.resizeBy(100, 100)
    })
    
    // 直接给定窗口大小
    let btn5 = document.querySelector('.btn:nth-child(6)');
    btn5.addEventListener('click', function (e) {
        newWindow.resizeTo(100, 100)
    })
</script>
```



### 1-6 窗口滚动

* `window.scrollBy(x,y)` —— 相对于当前位置，将窗口向右滚动 `x` 像素，并向下滚动 `y` 像素。允许负值

* `window.scrollTo(x,y)` —— 将窗口滚动到给定坐标 `(x,y)`

* `elem.scrollIntoView(top = true)` —— 滚动窗口，使 `elem` 显示在 `elem.scrollIntoView(false)` 的顶部（默认）或底部

```html
<style>
    .container {
        position: fixed;
        bottom: 40px;
        left: 50%;
        transform: translateX(-50%)
    }
    body {
        width: 1200px;
        height: 1200px;
        padding: 1000px;
    }
    .box {
        width: 300px;
        height: 300px;
        background-color:#3496db;
    }
</style>

<!-- 创建box -->
<div class="box"></div>
<!-- 当窗体缩小出现滚动条 -->
<div class="container">
    <button class="btn">scrollBy</button>
    <button class="btn">scrollTo</button>
    <button class="btn">scrollIntoView</button>
</div>

<script>
    //  window.scrollBy 滚动窗口滚动条，相对于当前位置向x,y滚动
     let btn = document.querySelector('.btn');
     btn.addEventListener('click', function (e) {
        window.scrollBy(100, 100)
    })
    
    //  window.scrollTo 滚动窗口滚动条，滚动到指定的坐标
    let btn1 = document.querySelector('.btn:nth-child(2)');
     btn1.addEventListener('click', function (e) {
        window.scrollTo(100, 100)
    })
    
    // scrollIntoView 滚动窗口 将元素显示在窗口内
    let btn2 = document.querySelector('.btn:nth-child(3)');
    let elem = document.querySelector('.box')
     btn2.addEventListener('click', function (e) {
        elem.scrollIntoView({block: 'end',inline: 'end', behavior: 'smooth'})
    })
</script>
```



### 1-7 弹窗聚焦与失焦

* `window.focus()` —— 窗口获取焦点
* `window.blur()` —— 窗口失去焦点

`focus`和 `blur` 事件允许跟踪窗口的切换

```js
// blur与focus事件
 window.onfocus = function () {
     console.log('获取焦点')
};
window.onblur = function () {
     console.log('失去焦点')
};
        
window.onblur = () => window.focus();
```



## 2. location对象

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220415004905.png)



### 2-1 location 属性

* `hash` —— 标识符，开头有一个`#`
* `host` —— 服务器域名和端口号
* `hostname` —— 返回 web 主机的域名
* `href` —— 返回当前页面的 `URL`
* `origin` —— 域名的标准形式
* `pathname` —— 返回当前页面的路径和文件名，开头有一个`/`
* `port` —— 返回 `web` 主机的端口
* `protocol` —— 返回所使用的 `web` 协议（`http: ` 或 `https:` ）
* `search` —— `URL` 参数查询字符，开头有一个`?`



### 2-2 search 查询参数

`window.location.search` 获取参数信息

```html
<form method="get">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="submit" value="提交">
</form>
<script>
    // 查询字符串返回对象
    let obj = {};
    let querStr = location.search;
    let query = querStr.substring(1).split('&')
    for (let i = 0; i < query.length; i++) {
        let item = query[i].split('=');
        obj[item[0]] = item[1]
    }
    console.log(obj);
</script>
```



### 2-3 replace 和 assign 替换页面

`location.assign()` 方法会触发窗口加载并显示指定的 `URL` 的内容

`location.replace()` 方法以给定的 `URL` 来替换当前的资源，调用 `replace()` 方法后，当前页面不会保存到会话历史中 `history`

```html
<button class="btn">assign跳转</button>
<button class="btn">replace跳转</button>

<script>
    let btn = document.querySelector('.btn');
    btn.addEventListener('click', function (e) {
        // assign 触发窗口加载url内容 可以回退
        window.location.assign('08-窗口滚动.html')
    })

    let btn1 = document.querySelector('.btn:nth-child(2)');
    btn1.addEventListener('click', function (e) {
        // replace 替换url内容，页面不会保存到history中 不能回退
        window.location.replace('08-窗口滚动.html')
    })
</script>
```



### 3-4 reload 重新加载

通过 `location.reload()` 方法可以重新加载页面

- `location.reload()` : 重新加载（有可能会从缓存中加载）
- `location.reload(true)`： 重新加载（从服务器重新加载）

```html
<button class="btn">reload重新加载</button>
<button class="btn">添加文字</button>

<script>
    let btn = document.querySelector('.btn');
    btn.addEventListener('click', function (e) {
        // assign 触发窗口加载url内容 可以回退
        window.location.reload();
    })

    let btn1 = document.querySelector('.btn:nth-child(2)');
    btn1.addEventListener('click', function (e) {
        let html = `<h3>hello javascript</h3>`
        document.documentElement.insertAdjacentHTML('afterbegin', html)
    })
</script>
```



## 3. navigator 对象

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220415100540.png)



### 3-1 对象属性

`Navigator` 对象包含有关浏览器的信息

主要用来获取浏览器的属性，区分浏览器类型

| 属性              | 描述                                           |
| :---------------- | :--------------------------------------------- |
| `appCodeName`     | 返回浏览器的代码名                             |
| `appMinorVersion` | 返回浏览器的次级版本                           |
| `appName`         | 返回浏览器的名称                               |
| `appVersion`      | 返回浏览器的平台和版本信息                     |
| `browserLanguage` | 返回当前浏览器的语言                           |
| `cookieEnabled`   | 返回指明浏览器中是否启用 `cookie` 的布尔值     |
| `cpuClass`        | 返回浏览器系统的 `CPU` 等级                    |
| `onLine`          | 返回指明系统是否处于脱机模式的布尔值           |
| `platform`        | 返回运行浏览器的操作系统平台                   |
| `systemLanguage`  | 返回 `OS` 使用的默认语言                       |
| `userAgent`       | 返回由客户机发送服务器的 `user-agent` 头部的值 |
| `userLanguage`    | 返回 `OS` 的自然语言设置                       |

```js
// navigator对象
console.log(navigator)

// userAgent
console.log(navigator.userAgent)
// appVersion 浏览器的平台和版本信息
console.log(navigator.appVersion);

// 浏览器供应商
console.log(navigator.vendor)
```



### 3-2 检测浏览器

通过 `navigator.userAgent` 来检测浏览器并返回浏览器名称字符串

```js
let sBrowser, sUsrAg = navigator.userAgent;

// The order matters here, and this may report false positives for unlisted browsers.

if (sUsrAg.indexOf("Firefox") > -1) {
	sBrowser = "Mozilla Firefox";
	// "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:61.0) Gecko/20100101 Firefox/61.0"
} else if (sUsrAg.indexOf("Opera") > -1 || sUsrAg.indexOf("OPR") > -1) {
	sBrowser = "Opera";
	//"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 OPR/57.0.3098.106"
} else if (sUsrAg.indexOf("Trident") > -1) {
	sBrowser = "Microsoft Internet Explorer";
	// "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; Zoom 3.6.0; wbx 1.0.0; rv:11.0) like Gecko"
} else if (sUsrAg.indexOf("Edge") > -1) {
	sBrowser = "Microsoft Edge";
	// "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 Edge/16.16299"
} else if (sUsrAg.indexOf("Chrome") > -1) {
	sBrowser = "Google Chrome or Chromium";
	// "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36"
} else if (sUsrAg.indexOf("Safari") > -1) {
	sBrowser = "Apple Safari";
	// "Mozilla/5.0 (iPhone; CPU iPhone OS 11_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/11.0 Mobile/15E148 Safari/604.1 980x1306"
} else {
	sBrowser = "unknown";
}

console.log("当前浏览器为: " + sBrowser);
```



## 4. history 对象

`History` 对象提供了操作浏览器会话历史（浏览器地址栏中访问的页面，以及当前页面中通过框架加载的页面）的接口

在顶层页面中，浏览器的回退和前进按钮旁的下拉菜单显示了可以通过`History`对象访问到的页面会话历史（session history）列表



### 4-1 length 属性

返回一个整数，该整数表示会话历史中元素的数目，包括当前加载的页

```js
length = history.length;
```



### 4-2 back

`History.back()` 

在浏览器历史记录里前往上一页。如果没有上一页，则此方法调用不执行任何操作，等价于 `history.go(-1)`

```js
window.history.back()
```



### 4-3 forward

`History.forward()` 

在浏览器历史记录里前往下一页，使用 `delta` 参数为 1 时调用 `history.go(delta)`的效果相同

```js
window.history.forward();
```



### 4-4 go

`History.go(deleta)` 

从会话历史记录中加载特定页面。你可以使用它在历史记录中前后移动，具体取决于`delta`参数的值

* `deleta` 参数值相对于当前页面你要去往历史页面的位置。负值表示向后移动，正值表示向前移动

```js
window.history.go(delta);

// 向后一页
window.history.go(-1)

// 向前移动一页
window.history.go(1)

// 向前移动两页
window.history.go(2);

// 向后移动两页
window.history.go(-2);

// 重新加载
window.history.go();
window.history.go(0);
```



### 4-5 pushState

`history.pushState(stateObj, title, url)`

向当前浏览器会话的历史堆栈中添加一个状态，浏览器不会向服务端请求数据，浏览器会记录 `pushState` 的历史记录，可以使用浏览器的前进、后退功能作用

* `state` —— 状态对象，它与 `pushState()` 创建的新历史记录条目相关联，可以通过 `history.state` 读取
* `title` —— 传递简短的标题
* `url` —— 新历史记录条目 `URL` 

```html
<h1>pushState</h1>
<script>
    // pushState 用来添加新条目
    let stateObj = {
        foo: 'bar'
    }
    // 浏览器未发生跳转，可以使用回退前进功能
    // 不会向服务端请求数据
    history.pushState(stateObj, 'page 2', './21-replaceState( )方法.html')
    console.log(history.state)
    
</script>
```



### 4-6 replaceState

`history.replaceState(stateObj, title, url)`

`history.replaceState()` 的使用与 `history.pushState()` 非常相似，区别在于 `replaceState()` 是修改了当前的历史记录项而不是新建一个

```html
<h1>replaceState</h1>
<script>
    let stateObj = {
        title: 'pushState'
    }
    // 不会跳转，只是修改了url
    history.replaceState(stateObj, 'page 3', './22-onpopstate事件.html')
    console.log(history.state)
</script>
```



### 4-7 onpopstate 事件

`window.onpopstate = funcRef`

每当激活同一文档中不同的历史记录条目时，`popstate` 事件就会在对应的 `window` 对象上触发

如果当前处于激活状态的历史记录条目是由 `history.pushState()` 方法创建的或者是由 `history.replaceState()` 方法修改的，则 `popstate` 事件的 `state` 属性包含了这个历史记录条目的 `state` 对象的一个拷贝

```html
<h1>onpopstate事件</h1>
<div class="btn-box">
    <button class="btn">go</button>
    <button class="btn">forward下一页</button>
    <button class="btn">back上一页</button>
</div>

<script>
    window.onpopstate = function(e) {
        console.log('hello')
        console.log(e.state)
    }
    let btn1 = document.querySelector('.btn:nth-child(1)')
    let btn2 = document.querySelector('.btn:nth-child(2)')
    let btn3 = document.querySelector('.btn:nth-child(3)')
    history.pushState({page: 1}, "title 1", "./19-go( )方法.html");
    history.pushState({page: 2}, "title 2", "./18-forward( )方法.html");
    history.pushState({page: 3}, "title 3", "./17-back( )方法.html");
    btn1.addEventListener('click', function (e) {
        window.history.go(1);
    })
    btn2.addEventListener('click', function (e) {
        window.history.forward();
    })
    btn3.addEventListener('click', function (e) {
        window.history.back();
    })
</script>
```



## 5. screen 对象

`Screen` 对象返回当前渲染窗口中和屏幕有关的属性

| 属性          | 描述                                       |
| :------------ | :----------------------------------------- |
| `availTop`    | 返回屏幕上边边界的第一个像素点             |
| `availLeft`   | 返回屏幕左边边界的第一个像素点             |
| `availHeight` | 返回显示屏幕的高度 (除 Windows 任务栏之外) |
| `availWidth`  | 返回显示屏幕的宽度 (除 Windows 任务栏之外) |
| `colorDepth`  | 返回目标设备或缓冲器上的调色板的比特深度   |
| `height`      | 返回显示屏幕的高度                         |
| `pixelDepth`  | 返回显示屏幕的颜色分辨率（比特每像素）     |
| `width`       | 返回显示器屏幕的宽度                       |
| `orientation` | 返回当前屏幕的转向                         |
