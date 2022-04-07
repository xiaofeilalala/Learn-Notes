# PicGo+GitHub+Gitee图床搭建



## 1. PicGo介绍

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210522213331.png)

**PicGo: 一个用于快速上传图片并获取图片 URL 链接的工具**



**PicGo功能**

- 支持拖拽图片上传
- 支持快捷键上传剪贴板里第一张图片
- Windows 和 macOS 支持右键图片文件通过菜单上传 (v2.1.0+)
- 上传图片后自动复制链接到剪贴板
- 支持自定义复制到剪贴板的链接格式
- 支持修改快捷键，默认快速上传快捷键：`command+shift+p`（macOS）| `control+shift+p`（Windows\Linux)
- 支持插件系统，已有插件支持 Gitee、青云等第三方图床
  - 更多第三方插件以及使用了 PicGo 底层的应用可以在 [Awesome-PicGo](https://github.com/PicGo/Awesome-PicGo) 找到。欢迎贡献！
- 支持通过发送 HTTP 请求调用 PicGo 上传（v2.2.0+)



**PicGo下载地址**

* GitHub地址  https://github.com/Molunerfinn/PicGo
* PicGo官网地址  https://molunerfinn.com/PicGo



## 2. PicGo+GitHub图床搭建

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210522215546.png)

### 2-1 创建仓库

登录GitHub创建一个新的仓库，用来存放PicGo上传的图片

* 在GitHub上点击右上方`+`按钮，点击 New repository 创建新仓库
* 设置仓库名称，描述，是否为私有库，是否添加README.md文件，模板，安全证书
* 注意设置为私有库后只有自己能够访问，图片上传上去之后是没法显示的
* 创建一个文件夹设置存放目录（在Github上无法创建文件夹，你可以在创建文件的时候通过`/`来创建一个文件夹）

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210522220200.png)



### 2-2 申请token

在GitHub个人设置页面，创建一个访问令牌

* 在GitHub上点击头像找到 settings -> Developer settings -> Personal access tokens
* 创建一个token用来配置GitHub图床 点击Generate new token
* 设置token名称，选者token的权限repo（一般设置可以访问个人信息以及查看，编辑，新增，删除权限就可以啦）
* 创建好后会生成一串字符串这就是token复制它（一定要保存好！）

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210522221700.png)



### 2-3 配置GitHub图床

在PicGo上对GitHub图床进行设置

* 仓库名格式为`GitHub用户名/仓库名`
* 分支名为main（GitHub更新后分支名从master改为了main）
* token为刚创建的token，粘贴过来就可以了
* 路径（如仓库内创建了目录填写目录文件名称即可）
* 自定义域名`https://raw.githubusercontent.com/用户名/仓库名/分支名`


![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210522223545.png)



### 2-4 PicGo设置

对PicGo进行设置

* 打开上传提示（可以看到上传信息，上传失败时会有提示）
* 打开时间戳重命名，防止同名文件上传时，上传失败或者导致图片无法显示

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210522223828.png)





## 3. PicGo+Gitee图床搭建

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210522215535.png)

### 3-1 下载Gitee插件

在PicGo上下载第三方插件Gitee

* 点击插件设置搜索Gitee
* 选者`Gitee2.0.3`版本的第三方插件

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210523124005.png)



### 3-2 创建仓库

登录Gitee创建一个新的仓库，用来存放PicGo上传的图片

* 在Gitee上点击右上方`+`按钮，点击新建仓库创建新的仓库
* 设置仓库名称，路径，仓库介绍，是否为私有库，设置语言、.gitignore、开源许可证，模板，分支
* 注意设置为私有库后只有自己能够访问，图片上传上去之后是没法显示的
* 创建一个文件夹设置存放目录（Gitee在新建文件夹时会提示不能创建空的文件夹，会添加一个.keep文件，选者确定就行了）

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210523124237.png)

### 3-3 申请私人令牌

在Gitee设置里，创建一个私人令牌

* 在Gitee上点击头像找到设置 -> 私人令牌 -> 生成新的令牌
* 创建一个新的令牌用来配置Gitee图床 点击生成新的令牌
* 设置私人令牌描述，选者私人令牌的权限projects（访问你的个人信息、最新动态，查看、创建、更新你的项目）
* 创建好后会生成一串字符串这就是私人令牌复制它（一定要保存好！）

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210523124947.png)



### 3-4 Gitee图床配置

在PicGo上对Gitee图床进行设置

* owner：Gitee用户名
* repo：新建仓库的名称
* path：图片存放在仓库的目录文件夹名称
* token：申请的私人令牌
* message：在提交上传图片时，对本次提交的内容的描述

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/20210523125447.png)





