# Yarn 使用

## 1. yarn简介

`Yarn` 是 `facebook` 发布的一款取代 `npm` 的包管理工具

* 速度超快 —— `Yarn` 缓存了每个下载过的包，所以再次使用时无需重复下载。 同时利用并行下载以最大化资源利用率，因此安装速度更快
* 超级安全 —— 在执行代码之前，`Yarn` 会通过算法校验每个安装包的完整性
* 超级可靠 —— 使用详细、简洁的锁文件格式和明确的安装算法，`Yarn` 能够保证在不同系统上无差异的工作



## 2. yarn安装与更新

### 2-1 全局安装

通过 `npm install -g` 全局去安装 `yarn` 包管理工具，默认安装的版本是 `yarn 1` 版本

```bash
# 全局安装
npm install -g yarn

# 查看yran安装版本
yarn --version

# 显示命令列表
yarn help
```



### 2-2 项目安装

在项目中需要使用 `yarn 2`，可以在项目更目录安装333

> *"Berry" 是 Yarn 2 发布序列的代号，同时也是我们的* [代码仓库](https://github.com/yarnpkg/berry) *的名称！*

```bash
yarn set version berry
```



### 2-3 yarn更新

将 `yarn` 更新到最新版本，`yarn` 会从我们的网站下载最新的二进制文件，并将其安装在您的项目中

> 将项目中的包管理工具升级为 `Yarn 2`，此后如果需要对此 `Yarn 2` 进行升级，则可以使用 `yarn set version latest` 进行升级，否则仍是对 `Yarn 1` 进行操作

```bash
yarn set version latest
```



### 2-4 安装maste分支最新版

尝试最新的 `master` 代码分支

```bash
yarn set version from sources
```



可以使用 `--branch` 参数来指定要安装特定的分支节点

```bash
yarn set version from sources --branch 1211
```



## 3. 镜像管理

### 3-1 安装淘宝镜像

修改国内镜像后可以加快软件包安装速度



查看当前使用的镜像

```bash
yarn config get registry
```



添加 `yarn` 的淘宝镜像

```bash
yarn config set registry https://registry.npm.taobao.org -g

# 恢复默认
yarn config set registry http://registry.npmjs.org/

# 安装sass
yarn config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
```



### 3-2 yrm镜像管理

`yrm` 是管理镜像的工具，可以列出可以使用的镜像，非常方便



安装 `yrm`

```bash
npm install -g yrm
```



列出可以使用的镜像

```bash
yrm ls
```



使用淘宝镜像

```bash
yrm use taobao
```



测试镜像速度

```bash
yrm test taobao
```



## 4. yarn使用

### 4-1 初始化项目

`yarn init` 用来初始化生成一个新的 `package.json` 文件

```bash
D:\My Study\08-Node.js\02-yarn>yarn init
yarn init v1.22.19
question name (02-yarn): yarn-init
question version (1.0.0):
question description: 初始化配置
question entry point (index.js):
question repository url:
question author (jsx <2738389567@qq.com> (https://github.com/xiaofeilalala)):
question license (MIT):
question private:
success Saved package.json
Done in 29.32s.
```

```json
{
  "name": "yarn-init",
  "version": "1.0.0",
  "description": "初始化配置",
  "main": "index.js",
  "author": "jsx <2738389567@qq.com> (https://github.com/xiaofeilalala)",
  "license": "MIT"
}

```



### 4-2 设置配置项

通过 `yarn config` 去设置显示删除配置项

```bash
yarn config list // 显示所有配置项
yarn config get <key> //显示某配置项
yarn config delete <key> //删除某配置项
yarn config set <key> <value> [-g|--global] //设置配置项
```



### 4-3 安装依赖

安装所有依赖

```bash
yarn install
```



强制重新下载所有包

```bash
yarn install --force
```





添加依赖项，会自动更新到 `package.json` 和 `yarn.lock` 文件中

```bash
# 安装最新版本
yarn add [packageName] 

# 安装指定版本
yarn add [packageName]@<version>

# 安装指定tag版本 beta,next或者latest
yarn add [packageName]@<tag>
```



安装包的精确版本，例如： `yarn add foo@1.2.3` 会接受 `1.9.1` 版本，但是 `yarn add foo@1.2.3 --exact` 只能安装指定 `1.2.3` 版本

```bash
 yarn add [packageName]@<version> --exact
 yarn add [packageName]@<version> -E
```



安装包的次要版本里的最新版，例如：`yarn add foo@1.2.3 --title` 会接受 `1.2.9`，但不接受 `1.3.0`

```bash
yarn add [packageName]@<version> --title
yarn add [packageName]@<version> -T
```



### 4-4 不同依赖类

在一个 `Node.js` 项目中，`package.json` 几乎是一个必须的文件，它的主要作用就是管理项目中所使用到的外部依赖包，同时它也是 `npm` 命令的入口文件

`npm` 目前支持以下几类依赖包管理：

- `dependencies`
- `devDependencies`
- `peerDependencies`
- `optionalDependencies`
- `bundledDependencies` / `bundleDependencies`

`dependencies`

应用依赖，或者叫做业务依赖，这是我们最常用的依赖包管理对象！它用于指定应用依赖的外部包，这些依赖是应用发布后正常执行时所需要的，但不包含测试时或者本地打包时所使用的包。



`devDependencies`

开发环境依赖，仅次于 `dependencies` 的使用频率！它的对象定义和 `dependencies` 一样，只不过它里面的包只用于开发环境，不用于生产环境，这些包通常是单元测试或者打包工具等，例如`gulp`, `grunt`, `webpack`, `moca`, `coffee`等



`peerDependencies`

同等依赖，或者叫同伴依赖，用于指定当前包（也就是你写的包）兼容的宿主版本。如何理解呢？ 试想一下，我们编写一个 `gulp` 的插件，而 `gulp` 却有多个主版本，我们只想兼容最新的版本，此时就可以用同等依赖（`peerDependencies`）来指定



`optionalDependencies`

可选依赖，如果有一些依赖包即使安装失败，项目仍然能够运行或者希望npm继续运行，就可以使用 `optionalDependencies`。另外`optionalDependencies` 会覆盖 `dependencies` 中的同名依赖包，所以不要在两个地方都写



`bundledDependencies` / `bundleDependencies`

打包依赖，`bundledDependencies` 是一个包含依赖包名的数组对象，在发布时会将这个对象中的包打包到最终的发布包里



不指定依赖类型默认安装到 `dependencies` 里，你也可以指定依赖类型

```bash
# 添加到 devDependencies 依赖项
yarn add [package]@[version] --dev
yarn add [package]@[version] -D

# 添加到 peerDependencies 依赖项
yarn add [package]@[version] --peer
yarn add [package]@[version] -P

# 添加到 optionalDependencies 依赖项
yarn add [package]@[version] --optional
yarn add [package]@[version] -O
```



### 4-5 升级依赖

根据需要将安装好的依赖包进行升级

```bash
# 更新所有软件包
yarn up

# 升级到最新版本
yarn up [packageName]

# 升级到指定版本
yarn up [packageName]@[version]

# 升级到指定tag版本
yarn up [packageName]@[tag]
```



### 4-6 删除依赖

从项目中删除依赖项 `dependencies`，会自动更新 `package.json` 和 `yarn.lock`

```bash
yarn remove [packageName] 
```



删除 `yarn` 全局软件包

```bash
yarn remove -g [packageName] 
```



### 4-7 发布模块

`yarn publish` 用于将当前模块发布到 [http://npmjs.com](https://link.zhihu.com/?target=http%3A//npmjs.com)



如果已经注册过，就使用下面的命令登录

```bash
yarn login
```



退出登录 `npm` 仓库

```bash
yarn logout
```



登录以后，就可以使用 `npm publish` 命令发布

```bash
yarn publish
```



撤销发布的模块 `npm unpublish`

```bash
# 删除某个版本
yarn unpublish [packageName]@<version>  
# 删除整个npm市场的包
yarn unpublish [packageName] --force 
```



### 4-8 运行命令

`yarn run` 用来执行在 `package.json` 中 `scripts` 属性下定义的脚本

```
// package.json
{
  "scripts": {
  "dev": "node app.js",
  "start": "node app.js"
  }
}
```



`yarn` 与 `npm` 一样 可以有 `yarn start` 和 `yarn test` 两个简写的运行脚本方式

```bash
# yarn 执行 dev 对应的脚本 node app.js
yarn run dev 
npm run

yarn start # yarn
npm start # npm
```



### 4-9 缓存控制

列出已缓存的每个包

```bash
yarn cache list 
```



全局缓存位置

```bash
yarn cache dir
```



清除缓存

```bash
yarn cache clean
```



### 4-10 模块信息

`yarn info` 可以用来查看某个模块的最新版本信息

```bash
yarn info [packageName] # yarn 
npm info [packageName] # npm

yarn info [packageName] --json # 输出 json 格式
npm info [packageName]  --json # npm

yarn info [packageName] readme # 输出 README 部分
npm info [packageName] readme
```
