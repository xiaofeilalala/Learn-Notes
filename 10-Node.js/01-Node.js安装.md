# Node.js安装



### 1-1 下载安装

`Node.js` 官网下载 [Node.js 下载地址](https://nodejs.org/zh-cn/)

可以根据不同平台系统选择你需要的 `Node.js` 安装包，`Node.js` 提供 `windows`、`macOS`、`Linux` 3个版本安装包



![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601211532250.png)



1. 打开下载的 `node-v16.15.0.x64.msi` 安装包进行安装
2. 勾选同意协议选项



<div style='text-align: center'>
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212125924.png">
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212159233.png">
</div>


3. 选择 `Node.js` 安装路径，默认为 `C盘` ,建议安装到 `D盘`
4. 在 `D盘` 创建文件夹，再点击 `change` 切换到当前创建的目录下



<div style='text-align: center'>
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212220472.png">
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212304181.png">
</div>


5. 安装扩展工具 —— 安装运行环境、`npm` 包管理工具、在线文档快捷方式、添加环境变量

6. 是否安装本机模块工具，选择不勾选



<div style='text-align: center'>
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212559457.png">
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212712052.png">
</div>


7. 告诉用户如果设置不需要修改或已确认配置，则要开始准备安装了
8. 安装完成界面。点击 `Finish` 关闭



<div style='text-align: center'>
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212729422.png">
    <img src="https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212800031.png">
</div>


### 1-2 查看版本

当安装完 `node.js` 后，可以在 `CMD` 命令行界面查看当前 `Node.js` 和 `npm` 版本信息

这里推荐微软商店的 `Windows Terminal` ，该命令行界面美观简洁非常好用，直接在微软商城下载就可以了

```shell
# 显示安装的nodejs版本
node -v 

# 显示安装的npm版本
npm -v
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601212926573.png)



![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601213027789.png)



### 1-3 文件配置

`Node.js` 默认将全局安装的包放在 `C盘` 里，为了减少 `C盘` 占用空间，将创建缓存和全局模块两个文件夹

* 默认地址 `C:\Users\用户名\AppData\Roaming\npm`
* 在 `D盘` `Node.js` 安装的目录下，创建两个文件夹
* 全局模块文件夹 `node_global` 用于全局安装包的存放
* 缓存文件夹 `node_cache` 用于缓存文件的存放

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601213309032.png)



打开 `D:\nodejs\node_modules\npm\` 目录下的 `npmrc` 文件添加配置项，用于环境变量的配置

> 当 `npmrc` 文件无法修改时，可以先删除该文件，创建一个同名文件配置内容添加进去

```shell
prefix = 创建的node_global文件夹所在路径
cache  = 创建的node_cache文件夹所在路径

prefix=D:\nodejs\node_global
cache=D:\nodejs\node_cache
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601214638195.png)



通过 `CMD` 设置 `Node.js` 中 `npm` 的配置，设置以后 `npm` 全局包的安装以及缓存文件的存放路径

```shell
npm config set prefix "D:\nodejs\node_global"
npm config set cache "D:\nodejs\node_cache"
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601213440616.png)



### 1-4 环境配置

设置 `Node.js` 环境变量，使 `node` 与 `npm` 命令可以在任意目录下使用

1. 右键桌面上我的电脑，打开属性设置 -> 高级系统设置 -> 环境变量

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601214800772.png)



2. 配置系统变量，在系统变量下新建，变量名：`NODE_PATH` 变量值: `D:\nodejs\node_global\node_modules` (`node_global` 文件夹下的 `node_modules` 文件夹)

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601215154674.png)



3. 配置用户变量中 `Path`  变量，将 `npm` 默认 `C盘` 的路径 `C:\Users\用户名\AppData\Roaming\npm` 修改为  `D:\nodejs\node_global\node_modules` (`node_global` 文件夹下的 `node_modules` 文件夹)

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601215625635.png)





### 1-5 安装淘宝镜像cnpm

`npm` 默认的 `registry` ,也就是下载 `npm` 包是从国外的服务器下载的，国内很慢

可以配置国内淘宝镜像 `cnpm` 加快 `npm` 包的下载安装

> 安装淘宝镜像 `cnpm` 可能会报错出现权限错误，解决方案：通过管理员 `powerShell` 命令行界面去执行命令

```shell
# 查看npm registry 包下载源
npm config get registry

# 更换镜像为淘宝镜像
npm config set registry https://registry.npm.taobao.org/

# 全局安装淘宝镜像cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601220541266.png)



如果想还原 `npm registry` 地址  ，只需再把地址配置成 `npm` 镜像就可以了

```shell
npm config set registry https://registry.npmjs.org/
```



![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220601220614269.png)



由于在管理员命令行界面下不能运行 `cnpm` 命令，通过非管理员 `CMD` 查看当前 `cnpm` 安装版本信息

```shell
cnpm -v
```



### 1-6 权限问题

使用 `npm install` 命令全局安装包时会出现 `error` 错误，原因是由于权限访问限制导致 `user` 用户下无法对 `node_modules` 文件内容进行读写编辑，通过 `path` 可以看到报错路径，可以通过管理员权限去安装全局安装包也可以配置用户权限来解决问题

* `npm ERR! path D:\nodejs\node_global\node_modules\typescript` —— 表示当前目录文件夹 `node_global` 不能编辑写入权限原因导致 `npm install` 错误
* 当 `node_cache` 文件夹也出现此问题时，只需设置当前文件夹属性安全设置，配置用户的访问权限即可



![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220605235411307.png)

1. 找到报错路径 `path` 文件夹
2. 右键点击文件夹属性，安全配置权限设置
3. 将组或用户名下的 `4` 个用户权限控制全部勾上即可

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main//imgs/image-20220606000936542.png)
