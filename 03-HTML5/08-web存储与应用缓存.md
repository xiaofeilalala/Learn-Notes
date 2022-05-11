# web存储与应用缓存

## 1. Cookie

### 1-1 什么是cookie

`cookie` 是客户端的解决方案，是一种网络服务器存储在计算机或移动设备上的纯文本文件，是服务器发送到 `Web` 浏览器上的一小块数据。一般大小限制在 `4kb` 以内

当浏览器从服务器上请求 `web` 页面时， 属于该页面的 `cookie` 会被添加到该请求中。服务端通过这种方式来获取用户的信息

`cookie` 主要用于以下三个方面：

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）



### 1-2 cookie创建

`cookie` 以名/值对形式存储，属性可以跟在键值对后使用分号以作分隔，可以使用 `document.cookie` 属性来创建 、读取、及删除 `cookie`

* `name=value` —— 键值对，可以设置要保存的 `Key/Value`

- `path=path` —— 服务器 `cookie` 的路径(例如 '/', '/mydir')，如果没有定义，默认为当前文档位置的路径
- `domain=domain` —— 定义域名，指定在该域名下才可以访问 `cookie`(例如 'example.com'， 'subdomain.example.com') 如果没有定义，默认为当前文档位置的路径的域名部分
- `maxAge=max-age-in-seconds` —— 最大失效时间(毫秒)，设置在多少后失效
- `expires=date-in-GMTString-format` —— 为 `cookie` 添加一个过期时间，如果没有定义，`cookie` 会在对话结束时过期
- `secure` —— 规定 `cookie` 是否通过安全的 `https` 传输

```js
document.cookie="username=jsx";
document.cookie="username=jsx; expires=Thu, 18 Dec 2043 12:00:00 GMT";
document.cookie="username=jsx; maxAge=2000000";
document.cookie="username=jsx; domain=baidu.com";
document.cookie="username=jsx; path=/mydir";
document.cookie="username=jsx; secure=true";
```



### 1-3 js-cookie使用

由于 `cookie` 值很难处理，通过 `js-cookie` 库来简化 `document.cookie` 的获取方法

```html
<!-- CDN链接 -->
<script src="https://cdn.bootcdn.net/ajax/libs/js-cookie/3.0.1/js.cookie.js"></script>
```

如果存在与名称空间 `cookie` 冲突的危险，`noConflict` 方法将允许您定义新名称空间并保留原始名称空间

```js
// 命名空间
let Cookies2 = Cookies.noConflict();
Cookies2.set('new', 'value');
console.log(Cookies2.get())
// {username: 'jsx', title: 'cookie', name: 'jsx', new: 'value'}
```

* `Cookies.set(name, value, options)` —— 创建或覆盖 `cookie`
* `Cookies.get(name, options)` —— 读取 `cookie`
* `Cookies.remove(name, options)` —— 删除 `cookie`

> **Tips：**如果值设置了路径，那么不能用简单的 `remove` 方法删除值，需要在 `delete` 时指定路径

```js
// 设置cookie键值对
Cookies.set('name', 'jsx')
// 设置cookie过期时间
Cookies.set('title', 'cookie', {
  expires: 7
})
// 设置path路径
Cookies.set('gril', 'ljj', {
  path: '/name'
})
// 获取cookie值
console.log(Cookies.get('name')) // jsx
// 获取所有cookie值
console.log(Cookies.get());
// {name: 'jsx', title: 'cookie'}

// 删除cookie
Cookies.remove('gril')
// 删除失败
console.log(Cookies.remove('gril')); // undefined
// 需要指定路径 路径指定错误删除失败
Cookies.remove('gril', {
  path: ''
})
console.log(Cookies.get())
// {name: 'jsx', title: 'cookie'}

// 命名空间
let Cookies2 = Cookies.noConflict();
Cookies2.set('new', 'value');
console.log(Cookies2.get())
// {name: 'jsx', title: 'cookie', new: 'value'}
```



### 1-4 json数据

`js-cookie` 允许你向 `cookie` 中存储 `json` 数据

通过 `set` 方法，传入 `Array` 或类似对象 `Object`，会将你传入的数据用 `JSON.stringify` 转换为 `string` 保存

用 `getJSON` 方法获取 `cookie`，那么 `js-cookie` 会用 `JSON.parse` 解析 `string` 并返回

> **Tips：**`js-cookie2.1.0`版本支持 `getJSON` 方法

```js
// js-cookie可以存储复杂类型数据 array object
// set方法会将对象通过Json.stringify序列化转换
Cookies.set('lesson', ['html', 'css', 'js'])
Cookies.set('class', {
  name: 'jsx',
  age: 22
})

// getJson方法会反序列化获得对象
// 2.1.0版本支持getJSON方法
console.log(Cookies.getJSON('lesson')); // ['html', 'css', 'js']
console.log(Cookies.getJSON('class')); // {name: 'jsx', age: 22}
```



### 1-5 options参数

* `expires` —— 定义有效期。如果传入 `Number`，那么单位为天，你也可以传入一个 `Date` 对象，表示有效期至 `Date` 指定时间。默认情况下 `cookie` 有效期截止至用户退出浏览器
* `path` —— `string`值为字符串类型，指定 `cookie` 可见路径的字符串。默认为 `'/'`
* `domain` —— `string`值为字符串类型，指定 `cookie` 可见域名的字符串。设置后 `cookie` 会对所有子域名可见。默认为对创建此 `cookie` 的域名和子域名可见
* `secure` —— `true` 或 `false`，指定 `cookie` 传输是否仅支持 `https`。默认为不要求协议必须为 `https`

```js
// options参数
// expires指定有效期 传入数字则是天数为单位
Cookies.set('expires', '有效期', {
  expires: 7
})
// path cookie可见路径
Cookies.set('path', '可见路径', {
  path: '/index'
})
// domain cookie可见域名
Cookies.set('domain', '可见域名', {
  domain: 'cn.bing.com'
})
// secure 是否支持https传输
Cookies.set('secure', 'https', {
  secure: true
})
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220510220454.png)





### 1-6 自定义覆盖

通过 `withConverter` 方法可以覆写默认的 `decode` 实现，并返回一个新的 `cookie` 实例

所有与 `decode` 有关的 `get` 操作，如 `Cookies.get()` 或 `Cookies.get('name')` 都会先执行此方法中的代码

```js
Cookies.withConverter({
    read: function (value, name) {
        // Read converter
    },
    write: function (value, name) {
        // Write converter
    }
});
```

```js
// withConverter方法可以覆盖默认的实现，返回一个新的 cookie 实例
document.cookie = 'escaped=jsx'
document.cookie = 'default=ljj'
// 自定义cookie操作
let cookies = Cookies.withConverter({
  read: function(value, name) {
    // Fall back to default for all other cookies
    return Cookies.converter.read(value, name)
  },
  write: function(value, name) {
    console.log(name === 'name'); // true
    if (name === "name") {
      return unescape('自定义set')
    }
    return Cookies.converter.write(value, name)
  }
})
cookies.set('name', 'cookie');
console.log(cookies.get('escaped')) // jsx
console.log(cookies.get('default')) // ljj
console.log(cookies.get('name')) // 自定义set
console.log(cookies.get())
// {escaped: 'jsx', default: 'ljj', name: '自定义set'}
```



## 2. Web Storage存储

在 `Web Storage` 本地存储 包括 `sessionStorage` 会话存储 和 `localStorage` 本地存储

`cookie` 和 `session` 完全是服务器端可以操作的数据，`sessionStorage` 和 `localStorage` 完全是浏览器端操作的数据

### 2-1 sessionStorage会话存储

`sessionStorage` 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据

被存储的键值对总是以 `UTF-16 DOMString` 的格式所存储，其使用两个字节来表示一个字符，对于对象整数 `key` 键值会自动转换成字符串形式

* 页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话
* 在新标签或窗口打开一个页面时会复制顶级浏览会话的上下文作为新会话的上下文，这点和 `session`、`cookies` 的运行方式不同
* 打开多个相同的 `URL` 的 `Tabs` 页面，会创建各自的 `sessionStorage`
* 关闭对应浏览器窗口（Window）/ tab，会清除对应的 `sessionStorage`



### 2-3 sessionStorage方法

保存数据到 `sessionStorage`

```js
sessionStorage.setItem('key', 'value');
```

从 `sessionStorage` 获取数据

```js
let data = sessionStorage.getItem('key');
```

从 `sessionStorage` 删除保存的数据

```js
sessionStorage.removeItem('key');
```

从 `sessionStorage` 删除所有保存的数据

```js
sessionStorage.clear();
```

从 `sessionStorage` 获取某个索引的 `key` 键名

```js
sessionStorage.key(index);
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220511125007.png)



### 2-2 localStorage本地存储

`localStorage` 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除

* 存储的数据将保存在浏览器会话中
* 存储在 `localStorage` 的数据可以长期保留
* `localStorage` 中的键值对总是以字符串的形式存储，数值类型会自动转化为字符串类型



### 2-4 localStorage方法

该语法用于添加 `localStorage` 项

```js
localStorage.setItem('myCat', 'Tom');
```

该语法用于读取 `localStorage` 项

```js
let cat = localStorage.getItem('myCat');
```

该语法用于移除 `localStorage` 项

```js
localStorage.removeItem('myCat');
```

该语法用于移除所有的 `localStorage` 项

```js
// 移除所有
localStorage.clear();
```

该语法用于获取某个索引的 `key` 键名

```js
localStorage.key(index);
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20220511124809.png)



### 2-5 三者区别

* 数据传递 —— `cookie` 在浏览器与服务器之间来回传递，`webStorage` 不会把数据发给服务器，仅在本地保存

* 作用范围 —— `cookie`、`sessionStorage`、`localStorage` 都遵循同源策略，`sessionStorage`除了协议、主机名、端口之外，还要求在同一窗口

* 存储 —— `webStorage` 拥有更大的存储量，`cookie` 大小限制 `4kb`，`webStorage` 达到 `5M` 或更大
* API —— `webStorage` 拥有`setItem`，`getItem`，`removeItem`，`clear`等`api`，`cookie` 需要自己封装
* 声明周期 —— `localStorage` 要手动清除，`sessionStorage` 在浏览器关闭后清除，`cookie` 可设置失效时间，否则默认为关闭浏览器后消失



## 3. 应用程序缓存

### 2-1 什么是应用程序缓存？

`HTML5` 引入了应用程序缓存（`Application Cache`），可对 `web` 应用进行缓存，并可在没有网络连接时进行访问

应用程序缓存为应用带来三个优势：

1. 离线浏览 - 用户可在应用离线时使用它们
2. 速度 - 已缓存资源加载得更快
3. 减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源



### 2-2 应用程序缓存使用

`HTML5` 的离线存储是基于一个 `manifest` 文件(缓存清单文件，后缀为.appcache)的缓存机制(不是存储技术)，通过这个文件上的清单解析离线存储资源，这些资源就会像 `cookie` 一样被存储了下来。之后当网络在处于离线状态时，浏览器会通过被离线存储的数据进行页面展示

* 首先在文档的 `html` 标签中设置 `manifest` 属性，引用 `manifest` 文件 
* 然后配置 `manifest` 文件，在 `manifest` 文件中编写离线存储的资源
* 必须要在服务器端正确的配置 `MIME-type`

```html
<!DOCTYPE html>
<html lang="en" manifest="index.appcache">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>引用程序缓存</title>
  <script>

  </script>
</head>

<body>

</body>

</html>
```



### 2-3 Manifest 文件

每个指定了 `manifest` 的页面在用户对其访问时都会被缓存，如果未指定 `manifest` 属性，则页面不会被缓存

`manifest` 属性可指向绝对网址或相对路径，但绝对网址必须与相应的网络应用同源

`manifest` 文件的建议的文件扩展名是：`".appcache"`

> **Tips：·**`manifest` 文件需要配置正确的 `MIME-type`，即 `"text/cache-manifest"`。必须在 `web` 服务器上进行配置

`manifest` 文件可分为三个部分：

- *CACHE MANIFEST* - 在此标题下列出的文件将在首次下载后进行缓存
- *NETWORK* - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
- *FALLBACK* - 在此标题下列出的文件规定当页面无法访问时的回退页面，第一个 URI 是资源，第二个是替补
- 以 `#` 开头的是注释行，但也可满足其他用途。应用的缓存只会在其 `manifest` 文件改变时被更新
- 可以在 `NETWORK` 使用 `*`，表示除 `CACHE` 外的所有其他资源/文件都需要因特网连接

```appcache
CACHE MANIFEST
# 2022-5-11 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
login.asp

FALLBACK:
/html/ /offline.html
```

一旦应用被缓存，它就会保持缓存直到发生下列情况：

* 用户清空浏览器缓存
* `manifest` 文件被修改
* 由程序来更新应用缓存
