# nvm 使用

## 1. nvm介绍

`nvm` 全英文也叫 `node.js version management`，是一个 `nodejs` 的版本管理工具

`nvm` 和 `npm` 都是 `node.js` 版本管理工具，为了解决 `node.js` 各种版本存在不兼容现象可以通过它可以安装和切换不同版本的 `node.js`



![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208151709030.png)



## 2. 安装与配置

### 2-1 nvm下载

根据自身系统选择 `windows` 或 `mac` 版本，可在点此在 [github](https://github.com/coreybutler/nvm-windows/releases) 上下载最新版本（最新版本 `1.1.9`）

- [nvm 1.1.7-setup.zip](http://nvm.uihtm.com/nvm1.1.7-setup.zip)：安装版，推荐使用
- [nvm 1.1.7-noinstall.zip](http://nvm.uihtm.com/nvm1.1.7-noinstall.zip): 绿色免安装版，但使用时需进行配置



![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208152122692.png)





### 2-2 nvm安装

官网上推荐卸载之前的 `node` 后安装 `nvm`，也可直接运行 `nvm-setup.exe` 安装

1. 选择同意许可证明 `I accept the agreement`

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208152129941.png)

2. 选择 `nvm` 安装路径。推荐 `D:/nvm`

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208152131734.png)

3. 选择当前安装的 `node.js` 的文件目录，一般为 `D:/nodejs`

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208152133208.png)

4. 确认安装

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208152134124.png)

5. 安装完成查看是否安装成功，`cmd` 运行 `nvm version`

```bash
nvm version
```

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208152137719.png)



### 2-3 nvm版本问题

当通过 `nvm` 切换 `nodejs` 版本为 `16` 以上时，`npm install [package]` 报错：

该问题不是 `npm` 的问题，也不是 `nodejs` 的问题，是 `nvm-windows` 的问题

```
Unexpected token '.'
```

解决方法：`nvm-windows` 已经更新版本解决了这个问题，通过更新 `nvm-windows` 到版本 `1.19` 完美解决

> **Tips：** `nvm` 更新完成后，出现问题的 `nodejs` 版本需要 `uninstall` 重装才能解决问题

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208152349944.png)



### 2-4 文件与环境配置

由于网络或者是墙的原因导致使用 `nvm` 下载 `node` 和 `npm` 很慢甚至失败

设置 `settings.txt` 设置 `node_mirro` 与 `npm_mirror` 为国内镜像地址，更换国内镜像源，加快下载速度

```bash
root: D:\nvm
path: D:\nodejs
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

当你安装完 `nvm`，会自动在你电脑上的本地环境配置变量 

* `NVM_HOME` —— 安装的 `NVM` 的路径

* `NVM_SYMLINK` —— 安装 `nvm` 时创建储存 `nvm` 依赖的文件夹

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208160659521.png)



## 3. nvm命令使用

### 3-1 命令提示

- `nvm arch`：显示 `node` 是运行在32位还是64位
- `nvm install <version> [arch]` ：安装 `node`， `version` 是特定版本也可以是最新稳定版本 `latest`。可选参数 `arch` 指定安装32位还是64位版本，默认是系统位数。可以添加 --`insecure` 绕过远程服务器的 `SSL`
- `nvm list [available]` ：显示已安装的列表。可选参数 `available`，显示可安装的所有版本。`list` 可简化为 `ls`
- `nvm on` ：开启 `node.js` 版本管理
- `nvm off` ：关闭 `node.js` 版本管理
- `nvm proxy [url]` ：设置下载代理。不加可选参数 `url`，显示当前代理。将 `url` 设置为 `none` 则移除代理
- `nvm node_mirror [url]` ：设置 `node` 镜像。默认是 https://nodejs.org/dist/。如果不写 `url`，则使用默认 `url`。设置后可至安装目录`settings.txt`文件查看，也可直接在该文件操作
- `nvm npm_mirror [url]` ：设置 `npm` 镜像。https://github.com/npm/cli/archive/。如果不写 `url`，则使用默认 `url`。设置后可至安装目录 `settings.txt` 文件查看，也可直接在该文件操作
- `nvm uninstall <version>` ：卸载指定版本 `node`
- `nvm use [version] [arch]` ：使用制定版本 `node`。可指定32/64位
- `nvm root [path]` ：设置存储不同版本 `node` 的目录。如果未设置，默认使用当前目录
- `nvm version` ：显示 `nvm` 版本。`version`可简化为 `v`



### 3-2 显示可安装版本

`nvm list available` 显示可下载版本的部分列表



![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208160714513.png)

### 3-3 安装node

`nvm install latest`安装最新版本 ( 安装时可以在上面看到 `node.js` 、 `npm` 相应的版本号 ，不建议安装最新版本)

`nvm install` 版本号 安装指定的版本的 `nodejs`

```bash
# 安装最新版
nvm install latest

# 安装指定版
nvm install 16.15.0
```



### 3-4 切换node版本

`nvm list` 或 `nvm ls` 查看目前已经安装的版本（当前版本号前面没有 `*` ， 此时还没有使用任何一个版本，这时使用 `node.js` 时会报错）

`nvm use` 版本号 使用指定版本的 `nodejs`（这时会发现在启用的 `node` 版本前面有 `*` 标记，这时就可以使用 `node.js`）

```bash
# 查看当前已安装的版本
nvm ls

# 切换版本
nvm use 16.15.0
```

![](https://gitee.com/xiaofeia/docs-pics/raw/master/imgs/202208160722090.png)



### 3-5 ndoe版本切换问题

切换问题：当使用 `nvm use` 命令切换版本时会乱码 `exit status 1 xxxxxx`

问题原因：没有权限操作，控制台权限不够

解决方法：使用管理员运行（`win10` 系统可以右键 `win` 图标, 选择 `"Windows PowerShell(管理员)`）

```bash
C:\Users\阿匪>nvm use 14.19.0
exit status 1: ��û���㹻��Ȩ��ִ�д˲�����
```

