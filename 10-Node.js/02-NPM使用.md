# NPM使用

## 1. npm起步

### 1-1 npm简介

`npm` 的全称是 `Node Package Manager`，是 `NodeJS` 标准的包管理和分发工具

新版的 `Nodejs` 已经集成了 `npm` ，在安装 `Node.js` 时 `npm` 会连带被安装，它很方便让 `JavaScript` 开发者下载、安装、上传以及管理已经安装的包

```bash
# 查看 npm 的版本 
npm -v 

# 查看各个命令的简单用法
npm -l 
 
# 查看 npm 命令列表
npm help

# 查看 npm 的配置
npm config list -l
```

如果安装的是旧版本的 `npm`，可以通过 `npm` 命令来升级

```bash
# 安装最新版本的 npm
npm install npm@latest -g

# 安装最新的官方测试版 npm
npm install npm@next -g
```



### 1-2 npm初始化

`npm init` 用来初始化生成一个新的 `package.json` 文件，它会向用户提问一系列问题，如果觉得不用修改默认配置可以直接回车默认跳过

* 尾缀带`-f`（代表 `force`）、`-y`（代表 `yes`），则跳过提问阶段直接生成一个新的 `package.json` 文件
* 不带尾缀的话，默认有提问阶段

```bash
# 初始化package.json文件
D:\My Study\08-Node.js\01-npm使用\01-npm初始化>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.

# 模块名称(必须是英文)
package name: (01-npm初始化) npm-init

# 模块版本
version: (1.0.0)

# 模块描述
description: npm初始化

# 模块的入口文件 默认index.js
entry point: (index.js)

# 项目启动时执行的命令
test command:

# 将该模块放入github仓库中
git repository:

# 模块关键字
keywords:

# 模块作者
author: jsx

# 模块发布时需要的证书
license: (ISC)

# package.json 内容
About to write to D:\My Study\08-Node.js\01-npm使用\01-npm初始化\package.json:

{
  "name": "npm-init",
  "version": "1.0.0",
  "description": "npm初始化",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "jsx",
  "license": "ISC"
}

# 确定配置是否创建
Is this OK? (yes) yes
```

```json
// package.json
{
  "name": "npm-init",
  "version": "1.0.0",
  "description": "npm初始化",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "jsx",
  "license": "ISC"
}
```



### 1-3 默认配置

在初始化 `npm init` 时，每次都都会默认询问需要配置的值，可以通过 `npm set` 对需要配置项的值进行预设

> `npm set` 用来设置环境变量，等于为 `npm init` 设置了默认值

```bash
npm set key=value [key=value...]
npm set key value [key=val...]

# 预设作者姓名
npm set init-author-name <Your name>
# 预设作者邮箱
npm set init-author-email <Your email>
# 预设作者网站主页url
npm set init-author-url <Your url>
# 预设作者项目许可证
npm set init-license <Your license>
# 预设项目版本号
npm set init-version <Your version>
```

上面的指令等于为 `npm init` 设置了默认值，以后执行 `npm init` 的时候，`package.json` 的作者姓名、邮件、主页、许可证就会自动写入预设的值。这些信息会存放在用户主目录的 `~/.npmrc` 文件，使得用户不用每个项目都输入



### 1-4 修改配置

如果某个项目有不同的设置，可以针对该项目运行 `npm config` 修改默认配置

> 根据 `npm set` 设置的属性进行修改，如果以 `-` 连接属性名则需要保持一致性以免造成预设混乱

```bash
npm config set key=value [key=value...]
npm config set key value [key=val...]

npm config set init-author-email <Your email>
npm config set init-author-name  <Your name>
npm config set init-author-url  <Your url>
npm config set init-license <Your license>
npm config set init-version <Your version>
```



### 1-5 查看配置

当需要查看自己预设的值时可以通过 `npm config list` 查看当前配置

* `-l` 则显示所有默认值配置
* `--json` 以 `json` 格式显示设置

```bash
npm config list
npm config list -l
```



## 2. package.json指南

### 2-1 文件解构

`package.json` 文件是项目的清单，它是用于工具的配置中心，也是 `npm` 和 `yarn` 存储所有已安装软件包的名称和版本的地方

> `package.json` 必须遵守 `JSON` 格式，否则，尝试以编程的方式访问其属性的程序则无法读取它

```json
{
  "name": "test-project",
  "version": "1.0.0",
  "description": "A Vue.js project",
  "main": "src/main.js",
  "private": true,
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "test": "npm run unit",
    "lint": "eslint --ext .js,.vue src test/unit",
    "build": "node build/build.js"
  },
  "dependencies": {
    "vue": "^2.5.2"
  },
  "devDependencies": {
    "autoprefixer": "^7.1.2",
    "babel-core": "^6.22.1",
    "babel-eslint": "^8.2.1",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-jest": "^21.0.2",
    "babel-loader": "^7.1.1",
    "babel-plugin-dynamic-import-node": "^1.2.0",
    "url-loader": "^0.5.8",
    "vue-jest": "^1.0.2",
    "vue-loader": "^13.3.0",
    "vue-style-loader": "^3.0.1",
    "vue-template-compiler": "^2.5.2",
    "webpack": "^3.6.0",
    "webpack-bundle-analyzer": "^2.9.0",
    "webpack-dev-server": "^2.9.1",
    "webpack-merge": "^4.1.0"
  },
  "engines": {
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0"
  },
  "browserslist": ["> 1%", "last 2 versions", "not ie <= 8"]
}
```

- `version` 表明了当前的版本
- `name` 设置了应用程序/软件包的名称
- `description` 是应用程序/软件包的简短描述
- `main` 设置了应用程序的入口点
- `private` 如果设置为 `true`，则可以防止应用程序/软件包被意外地发布到 `npm`
- `scripts` 定义了一组可以运行的 `node` 脚本
- `dependencies` 设置了作为依赖安装的 `npm` 软件包的列表
- `devDependencies` 设置了作为开发依赖安装的 `npm` 软件包的列表
- `engines` 设置了此软件包/应用程序在哪个版本的 `Node.js` 上运行
- `browserslist` 用于告知要支持哪些浏览器（及其版本）



### 2-2 属性分类

`name`  —— 设置软件包的名称

名称必须少于 `214` 个字符，且不能包含空格，只能包含小写字母、连字符（`-`）或下划线（`_`）

```json
"name": "nodejs_cn"
```



`author` —— 列出软件包的作者名称信息

```json
{ "author": "NodeJS中文网 <mail@nodejs.cn> (http://nodejs.cn)" }
```



`contributors` —— 除作者外，该项目可以有一个或多个贡献者。 此属性是列出他们的数组

```json
{ "contributors": ["NodeJS中文网 <mail@nodejs.cn> (http://nodejs.cn))", ...] }
```



`bugs` —— 链接到软件包的问题跟踪器，最常用的是 `GitHub` 的 `issues` 页面

```json
{ "bugs": "https://github.com/nodejscn/node-api-cn/issues" }
```



`homepage` —— 设置软件包的主页

```json
{ "homepage": "http://nodejs.cn" }
```



`version` —— 指定软件包的当前版本

* 此属性遵循版本的语义版本控制 `(semver)` 记法，这意味着版本始终以 3 个数字表示：`x.x.x`
* 第一个数字是主版本号，第二个数字是次版本号，第三个数字是补丁版本号
* 仅修复缺陷的版本是补丁版本，引入向后兼容的更改的版本是次版本，具有重大更改的是主版本

```json
"version": "1.0.0"
```



`license` —— 指定软件包的许可证

```json
"license": "MIT"
```



`keywords` —— 此属性包含与软件包功能相关的关键字数组

```json
"keywords": ["email", "machine learning", "ai"]
```



`description` —— 此属性包含了对软件包的简短描述

```json
"description": "NodeJS中文网入门教程"
```



`repository` —— 此属性指定了此程序包仓库所在的位置

* 可以显示的设置版本控制系统

```json
"repository": {
  "type": "git",
  "url": "https://github.com/nodejscn/node-api-cn.git"
}
```



`main` —— 设置软件包的入口点

当在应用程序中导入此软件包时，应用程序会在该位置搜索模块的导出

```json
"main": "src/main.js"
```



`private` —— 如果设置为 `true`，则可以防止应用程序/软件包被意外发布到 `npm` 上

```json
"private": true
```



`script` —— 定义一组可以运行的 `node` 脚本

*  可以通过调用 `npm run XXXX` 或 `yarn XXXX` 来运行它们，其中 `XXXX` 是命令的名称
*  可以为命令使用任何的名称，脚本也可以是任何操作

```json
"scripts": {
  "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
  "start": "npm run dev",
  "unit": "jest --config test/unit/jest.conf.js --coverage",
  "test": "npm run unit",
  "lint": "eslint --ext .js,.vue src test/unit",
  "build": "node build/build.js"
}
```



`dependencies` —— 设置作为依赖安装的 `npm` 软件包的列表

* 当使用 `npm install <package name>` 或 `yarn add <package name>` 安装软件包时，该软件包会被自动地插入此列表中

```json
"dependencies": {
  "vue": "^2.5.2"
}
```



`devDependencies` —— 设置作为开发依赖安装的 `npm` 软件包的列表

> 它们不同于 `dependencies`，因为它们只需安装在开发机器上，而无需在生产环境中运行代码

* 当使用 `npm install --save-dev <package name>` 或 `yarn add -dev <package name>` 该软件包会被自动地插入此列表中

```json
"devDependencies": {
  "autoprefixer": "^7.1.2",
  "babel-core": "^6.22.1"
}
```



`engines` —— 设置此软件包/应用程序要运行的 `Node.js` 或其他命令的版本

```json
"engines": {
  "node": ">= 6.0.0",
  "npm": ">= 3.0.0",
  "yarn": "^0.13.0"
}
```



`browserslist` —— 用于告知要支持哪些浏览器（及其版本）

```json
"browserslist": [
  "> 1%",
  "last 2 versions",
  "not ie <= 8"
]
```



### 2-3 package-lock.json文件

当我们在安装版本依赖时，默认是兼容最新版本，当不同用户初始化包版本不一致的情况下就会造成错误

在版本 `npm 5.0` 中，`npm` 引入了 `package-lock.json` 文件，`package-lock.json` 文件解决了 `package.json` 不同用户在不同环境下包安装版本不长一智问题

* `package-lock.json` 用来固定包的版本、包的源地址等信息，保证在不同的用户开发环境中加载的是相同的包
* `package-lock.json` 文件需要被提交到 `Git` 仓库，以便被其他人获取
* 当用户使用 `npm install` 命令来安装我们的依赖项时，会从 `package.lock.json` 文件中进行安装

> 当运行 `npm update` 时，`package-lock.json` 文件中的依赖的版本会被更新

```json
{
  "requires": true,
  "lockfileVersion": 1,
  "dependencies": {
    "ansi-regex": {
      "version": "3.0.0",
      "resolved": "https://registry.npmjs.org/ansi-regex/-/ansi-regex-3.
0.0.tgz",
      "integrity": "sha1-7QMXwyIGT3lGbAKWa922Bas32Zg="
    },
    "cowsay": {
      "version": "1.3.1",
      "resolved": "https://registry.npmjs.org/cowsay/-/cowsay-1.3.1.tgz"
,
      "integrity": "sha512-3PVFe6FePVtPj1HTeLin9v8WyLl+VmM1l1H/5P+BTTDkM
Ajufp+0F9eLjzRnOHzVAYeIYFF5po5NjRrgefnRMQ==",
      "requires": {
        "get-stdin": "^5.0.1",
        "optimist": "~0.6.1",
        "string-width": "~2.1.1",
        "strip-eof": "^1.0.0"
      }
    },
    "get-stdin": {
      "version": "5.0.1",
      "resolved": "https://registry.npmjs.org/get-stdin/-/get-stdin-5.0.
1.tgz",
      "integrity": "sha1-Ei4WFZHiH/TFJTAwVpPyDmOTo5g="
    },
    "is-fullwidth-code-point": {
      "version": "2.0.0",
      "resolved": "https://registry.npmjs.org/is-fullwidth-code-point/-/
is-fullwidth-code-point-2.0.0.tgz",
      "integrity": "sha1-o7MKXE8ZkYMWeqq5O+764937ZU8="
    },
    "minimist": {
      "version": "0.0.10",
      "resolved": "https://registry.npmjs.org/minimist/-/minimist-0.0.10
.tgz",
      "integrity": "sha1-3j+YVD2/lggr5IrRoMfNqDYwHc8="
    },
    "optimist": {
      "version": "0.6.1",
      "resolved": "https://registry.npmjs.org/optimist/-/optimist-0.6.1.tgz",
      "integrity": "sha1-2j6nRob6IaGaERwybpDrFaAZZoY=",

      "requires": {
        "minimist": "~0.0.1",
        "wordwrap": "~0.0.2"
      }
    },
    "string-width": {
      "version": "2.1.1",
      "resolved": "https://registry.npmjs.org/string-width/-/string-width-2.1.1.tgz",
      "integrity": "sha512-nOqH59deCq9SRHlxq1Aw85Jnt4w6KvLKqWVik6oA9ZklXLNIOlqg4F2yrT1MVaTjAqvVwdfeZ7w7aCvJD7ugkw==",
      "requires": {
        "is-fullwidth-code-point": "^2.0.0",
        "strip-ansi": "^4.0.0"
      }
    },
    "strip-ansi": {
      "version": "4.0.0",
      "resolved": "https://registry.npmjs.org/strip-ansi/-/strip-ansi-4.0.0.tgz",
      "integrity": "sha1-qEeQIusaw2iocTibY1JixQXuNo8=",
      "requires": {
        "ansi-regex": "^3.0.0"
      }
    },
    "strip-eof": {
      "version": "1.0.0",
      "resolved": "https://registry.npmjs.org/strip-eof/-/strip-eof-1.0.0.tgz",
      "integrity": "sha1-u0P/VZim6wXYm1n80SnJgzE2Br8="
    },
    "wordwrap": {
      "version": "0.0.3",
      "resolved": "https://registry.npmjs.org/wordwrap/-/wordwrap-0.0.3.tgz",
      "integrity": "sha1-o9XabNXAvAAI03I0u68b7WMFkQc="
    }
  }
}
```

`require` 属性表示该模块所依赖的包，每个依赖都有 `version` 字段、指向软件包位置的 `resolved` 字段、以及用于校验软件包的 `integrity` 字符串



## 3. 安装模块

### 3-1 安装命令

如果项目具有 `package.json` 文件，则可通过 `npm install` 命令去安装项目所需依赖，并且会在 `node_modules` 文件夹（如果尚不存在则会创建）中安装项目所需的所有东西

```bash
# 根据package.json文件安装所需模块
npm install
```



### 3-2 全局与本地安装

每个模块可以全局安装，也可以本地安装

* 全局安装 —— 将一个模块安装到系统目录中，各个项目都可以调用
* 本地安装 —— 将一个模块下载到当前项目的 `node_modules` 子目录，然后只有在项目目录之中，才能调用这个模块

```bash
# 本地安装
npm install <packageName>
# 简写
npm i <packageName>
# 安装多个模块 空格分隔
npm install <packageName> <packageName>
```

```bash
# 全局安装
npm install <packageName> --global
# 简写
npm i <packageName> -g
```

```bash
# 强制重新安装
$ npm install <packageName> --force

# 强制重新安装所有模块，删除 node_modules 目录，重新执行 npm install
$ rm -rf node_modules
$ npm install
```



### 3-3 不同版本安装

`npm install` 指令会安装模块的最新版，如果要安装模块的特定版本，可以在模块名后面加上@和版本号

```bash
npm install <packageName>@<version>
```



全局安装的模块也可以指定安装的版本，通过 `--global` 或 `-g` 标识

```bash
npm install <packageName>@<version> -g
```



可以使用 `npm view`，列出软件包所有的以前的版本

```bash
npm view <packageName> versions
[
  '0.0.0',          '0.6.0',                '0.7.0',
  '0.7.1',          '0.7.3',                '0.7.4',
  '0.7.5',          '0.7.6',                '0.8.0',
  '0.8.1',          '0.8.2',                '0.8.3',
  '0.8.4',          '0.8.6',                '0.8.7',
  '0.8.8',          '0.9.0',                '0.9.1',
  '0.9.2',          '0.9.3',                '0.10.0',
  '0.10.1',         '0.10.2',               '0.10.3',
  '0.10.4',         '0.10.5',               '0.10.6',
  '0.11.0-rc',      '0.11.0-rc2',           '0.11.0-rc3',
  '0.11.0',         '0.11.1',               '0.11.2',
  '0.11.3',         '0.11.4',               '0.11.5',
  '0.11.6',         '0.11.7',               '0.11.8',
  '0.11.9',         '0.11.10',              '0.12.0-beta1',
  '0.12.0-beta2',   '0.12.0-beta3',         '0.12.0-beta4',
  '0.12.0-beta5',   '0.12.0-csp',           '0.12.0-rc',
  '0.12.0-rc2',     '0.12.0',               '0.12.1-csp',
  '0.12.1-csp.1',   '0.12.1-csp.2',         '0.12.1',
  '0.12.2',         '0.12.3',               '0.12.4',
  '0.12.5-csp',     '0.12.5',               '0.12.6-csp',
  '0.12.6',         '0.12.7-csp',           '0.12.7',
  '0.12.8-csp',     '0.12.8',               '0.12.9-csp',
  '0.12.9',         '0.12.10-csp',          '0.12.10',
  '0.12.11-csp',    '0.12.11',              '0.12.12-csp',
  '0.12.12',        '0.12.13-csp',          '0.12.13',
  '0.12.14-csp',    '0.12.14',              '0.12.15-csp',
  '0.12.15',        '0.12.16-csp',          '0.12.16',
  '1.0.0-alpha.1',  '1.0.0-alpha.2',        '1.0.0-alpha.3',
  '1.0.0-alpha.4',  '1.0.0-alpha.5',        '1.0.0-alpha.6',
  '1.0.0-alpha.7',  '1.0.0-alpha.8',        '1.0.0-beta.1',
  '1.0.0-beta.2',   '1.0.0-beta.3',         '1.0.0-beta.4',
  '1.0.0-csp',      '1.0.0-migration',      '1.0.0-rc.1',
  '1.0.0-rc.2',     '1.0.0-rc.2-migration', '1.0.0',
  '1.0.1',          '1.0.2',                '1.0.3',
  '1.0.4',          '1.0.5',                '1.0.6',
  '1.0.7',          '1.0.8',                '1.0.9',
  '1.0.10-csp',     '1.0.10',               '1.0.11-csp',
  '1.0.11',         '1.0.12-csp',           '1.0.12-csp-1',
  '1.0.12',         '1.0.13-csp',           '1.0.13',
  '1.0.14-csp',     '1.0.14',               '1.0.15-csp',
  '1.0.15',         '1.0.16-csp',           '1.0.16',
  '1.0.17-csp',     '1.0.17',               '1.0.18-csp',
  '1.0.18',         '1.0.19-csp',           '1.0.19',
  '1.0.20-csp',     '1.0.20',               '1.0.21-csp',
  '1.0.21',         '1.0.22-csp',           '1.0.22',
  '1.0.23-csp',     '1.0.23',               '1.0.24-csp',
  '1.0.24',         '1.0.25-csp',           '1.0.25',
  '1.0.26-csp',     '1.0.26',               '1.0.27-csp',
  '1.0.27',         '1.0.28-csp',           '1.0.28',
  '2.0.0-alpha.1',  '2.0.0-alpha.2',        '2.0.0-alpha.3',
  '2.0.0-alpha.4',  '2.0.0-alpha.5',        '2.0.0-alpha.6',
  '2.0.0-alpha.7',  '2.0.0-alpha.8',        '2.0.0-beta.1',
  '2.0.0-beta.2',   '2.0.0-beta.3',         '2.0.0-beta.4',
  '2.0.0-beta.5',   '2.0.0-beta.6',         '2.0.0-beta.7',
  '2.0.0-beta.8',   '2.0.0-rc.1',           '2.0.0-rc.2',
  '2.0.0-rc.3',     '2.0.0-rc.4',           '2.0.0-rc.5',
  '2.0.0-rc.6',     '2.0.0-rc.7',           '2.0.0-rc.8',
  '2.0.0',          '2.0.1',                '2.0.2',
  '2.0.3',          '2.0.4',                '2.0.5',
  '2.0.6',          '2.0.7',                '2.0.8',
  '2.1.0',          '2.1.1',                '2.1.2',
  '2.1.3',          '2.1.4',                '2.1.5',
  '2.1.6',          '2.1.7',                '2.1.8',
  '2.1.9',          '2.1.10',               '2.2.0-beta.1',
  '2.2.0-beta.2',   '2.2.0',                '2.2.1',
  '2.2.2',          '2.2.3',                '2.2.4',
  '2.2.5',          '2.2.6',                '2.3.0-beta.1',
  '2.3.0',          '2.3.1',                '2.3.2',
  '2.3.3',          '2.3.4',                '2.4.0',
  '2.4.1',          '2.4.2',                '2.4.3',
  '2.4.4',          '2.5.0',                '2.5.1',
  '2.5.2',          '2.5.3',                '2.5.4',
  '2.5.5',          '2.5.6',                '2.5.7',
  '2.5.8',          '2.5.9',                '2.5.10',
  '2.5.11',         '2.5.12',               '2.5.13',
  '2.5.14',         '2.5.15',               '2.5.16',
  '2.5.17-beta.0',  '2.5.17',               '2.5.18-beta.0',
  '2.5.18',         '2.5.19',               '2.5.20',
  '2.5.21',         '2.5.22',               '2.6.0-beta.1',
  '2.6.0-beta.2',   '2.6.0-beta.3',         '2.6.0',
  '2.6.1',          '2.6.2',                '2.6.3',
  '2.6.4',          '2.6.5',                '2.6.6',
  '2.6.7',          '2.6.8',                '2.6.9',
  '2.6.10',         '2.6.11',               '2.6.12',
  '2.6.13',         '2.6.14',               '2.7.0-alpha.1',
  '2.7.0-alpha.2',  '2.7.0-alpha.3',        '2.7.0-alpha.4',
  '2.7.0-alpha.5',  '2.7.0-alpha.6',        '2.7.0-alpha.7',
  '2.7.0-alpha.8',  '2.7.0-alpha.9',        '2.7.0-alpha.10',
  '2.7.0-alpha.11', '2.7.0-alpha.12',       '2.7.0-beta.1',
  '2.7.0-beta.2',   '2.7.0-beta.3',         '2.7.0-beta.4',
  '2.7.0-beta.5',   '2.7.0-beta.6',         '2.7.0-beta.7',
  '2.7.0-beta.8',   '2.7.0',                '2.7.1',
  '2.7.2',          '2.7.3',                '2.7.4',
  '2.7.5',          '2.7.6',                '2.7.7',
  '2.7.8',          '3.0.0-alpha.0',        '3.0.0-alpha.1',
  '3.0.0-alpha.2',  '3.0.0-alpha.3',        '3.0.0-alpha.4',
  '3.0.0-alpha.5',  '3.0.0-alpha.6',        '3.0.0-alpha.7',
  '3.0.0-alpha.8',  '3.0.0-alpha.9',        '3.0.0-alpha.10',
  '3.0.0-alpha.11', '3.0.0-alpha.12',       '3.0.0-alpha.13',
  '3.0.0-beta.1',   '3.0.0-beta.2',         '3.0.0-beta.3',
  '3.0.0-beta.4',   '3.0.0-beta.5',         '3.0.0-beta.6',
  '3.0.0-beta.7',   '3.0.0-beta.8',         '3.0.0-beta.9',
  '3.0.0-beta.10',  '3.0.0-beta.11',        '3.0.0-beta.12',
  '3.0.0-beta.13',  '3.0.0-beta.14',        '3.0.0-beta.15',
  '3.0.0-beta.16',  '3.0.0-beta.17',        '3.0.0-beta.18',
  '3.0.0-beta.19',  '3.0.0-beta.20',        '3.0.0-beta.21',
  '3.0.0-beta.22',  '3.0.0-beta.23',        '3.0.0-beta.24',
  '3.0.0-rc.1',     '3.0.0-rc.2',           '3.0.0-rc.3',
  '3.0.0-rc.4',     '3.0.0-rc.5',           '3.0.0-rc.6',
  '3.0.0-rc.7',     '3.0.0-rc.8',           '3.0.0-rc.9',
  '3.0.0-rc.10',    '3.0.0-rc.11',          '3.0.0-rc.12',
  '3.0.0-rc.13',    '3.0.0',                '3.0.1',
  '3.0.2',          '3.0.3',                '3.0.4',
  '3.0.5',          '3.0.6',                '3.0.7',
  '3.0.8',          '3.0.9',                '3.0.10',
  '3.0.11',         '3.1.0-beta.1',         '3.1.0-beta.2',
  '3.1.0-beta.3',   '3.1.0-beta.4',         '3.1.0-beta.5',
  '3.1.0-beta.6',   '3.1.0-beta.7',         '3.1.0',
  '3.1.1',          '3.1.2',                '3.1.3',
  '3.1.4',          '3.1.5',                '3.2.0-beta.1',
  '3.2.0-beta.2',   '3.2.0-beta.3',         '3.2.0-beta.4',
  '3.2.0-beta.5',   '3.2.0-beta.6',         '3.2.0-beta.7',
  '3.2.0-beta.8',   '3.2.0',                '3.2.1',
  '3.2.2',          '3.2.3',                '3.2.4',
  '3.2.5',          '3.2.6',                '3.2.7',
  '3.2.8',          '3.2.9',                '3.2.10',
  '3.2.11',         '3.2.12',               '3.2.13',
  '3.2.14',         '3.2.15',               '3.2.16',
  '3.2.17',         '3.2.18',               '3.2.19',
  '3.2.20',         '3.2.21',               '3.2.22',
  '3.2.23',         '3.2.24',               '3.2.25',
  '3.2.26',         '3.2.27',               '3.2.28',
  '3.2.29',         '3.2.30',               '3.2.31',
  '3.2.32',         '3.2.33',               '3.2.34-beta.1',
  '3.2.34',         '3.2.35',               '3.2.36',
  '3.2.37'
]
```



### 3-4 生产依赖与开发依赖

使用 `npm install <package-name>` 安装 `npm` 软件包时，是将其安装为生产依赖项

> 开发依赖是仅用于开发的程序包，在生产环境中并不需要

* `npm install <package-name>` —— 安装并默认添加条目到 `package.json` 文件的 `dependencies`
* `--save` 或 `-S` —— 在 `npm5.0` 版本之前必须手动指定该参数才会安装到 `package.json` 文件的 `dependencies`
* `--save-dev` 或 `-D` —— 安装为开发依赖项，添加条目到 `package.json` 文件的 `devDependencies `

```bash
# 生产依赖
npm install <packageName>
npm install <packageName> --save
npm install <packageName> -S

# 开发依赖
npm install <packageName> --save-dev
npm install <packageName> -D
```



`npm install` 默认会安装 `dependencies` 生产依赖项和 `devDependencies` 开发依赖项中的所有模块

需要设置 `--production` 标志，以避免安装这些开发依赖项，只安装 `dependencies` 生产依赖的模块

```bash
npm install --production
# 或者
NODE_ENV=production npm install
```



## 4. 卸载模块

### 4-1 卸载命令

卸载本地安装的模块，需要从项目的根文件夹（包含 `node_modules` 文件夹的文件）中执行指令

```bash
npm uninstall <packageName>
```



### 4-2 全局与本地卸载

卸载本地依赖，需要在项目更目录运行该命令

```bash
npm uninstall <packageName>
```



如果需要卸载全局安装的模块，则要添加 `-g` 或 `--global` 标志从文件中移除

```bash
npm uninstall -g <package-name>
```



### 4-3 卸载依赖与生产依赖

使用 `-S` 或 `--save` 标志，则此操作还会移除 `package.json` 文件中的引用

```bash
npm uninstall <packageName> --save
npm uninstall <packageName> -S
```



卸载本地安装开发依赖的模块，必须使用 `-D` 或 `--save-dev` 标志从文件中移除

```bash
npm uninstall <package-name> --save-dev
npm uninstall <package-name> -D
```



## 5. 更新模块

### 5-1 全局与本地更新

`npm update` 命令可以更新安装的模块

```bash
# 升级当前项目的指定模块
$ npm update <package-name>

# 升级全局安装的模块
$ npm update -global <package-name>
$ npm update -g <package-name>
```



### 5-2 更新版本规则

使用 `npm update` 会根据 `package.json` 文件里模块的更新规则来进行版本控制

* `^` —— 只会执行不更改最左边非零数字的更新（如果写入的是 `^0.13.0`，则当运行 `npm update` 时，可以更新到 `0.13.1`、`0.13.2` 等，但不能更新到 `0.14.0` 或更高版本。 如果写入的是 `^1.13.0`，则当运行 `npm update` 时，可以更新到 `1.13.1`、`1.14.0` 等，但不能更新到 `2.0.0` 或更高版本）
* `~` —— 只能更新到补丁版本（ 如果写入的是 `〜0.13.0`，则当运行 `npm update` 时，会更新到补丁版本：即 `0.13.1` 可以，但 `0.14.0` 不可以）

```json
// package.json
{
  "dependencies": {
    "cowsay": "^1.3.1"
  }
}
```

```json
// package-lock.json
{
  "requires": true,
  "lockfileVersion": 1,
  "dependencies": {
    "cowsay": {
      "version": "1.3.1",
      "resolved": "https://registry.npmjs.org/cowsay/-/cowsay-1.3.1.tgz",
      "integrity": "sha512-3PVFe6FePVtPj1HTeLin9v8WyLl+VmM1l1H/5P+BTTDkMAjufp+0F9eLjzRnOHzVAYeIYFF5po5NjRrgefnRMQ==",
      "requires": {
        "get-stdin": "^5.0.1",
        "optimist": "~0.6.1",
        "string-width": "~2.1.1",
        "strip-eof": "^1.0.0"
      }
    }
  }
}
```

`cowsay` 模块安装版本是 `1.3.1`，并且更新的规则是 `^1.3.1`，表示版本控制规则可以更新到补丁版本和次版本：即 `1.3.2`、`1.4.0`、依此类推



### 5-3 更新依赖与生产依赖

> `npm update` 只会对新的次版本或补丁版本更新，`npm-check-updates` 可以对模块主版本更新

如果有新的次版本或补丁版本，并且输入了 `npm update`，则已安装的版本会被更新，并且 `package-lock.json` 文件会被新版本填充，`package.json` 文件则保持不变

如需文件更新可以使用 `-S` 或 `--save` 或 `-D` 或 `--save-dev` 参数，可以在安装的时候更新 `package.json` 里面依赖与开发依赖模块的版本号

```bash
# 更新依赖环境下的版本号，并更新模块
npm update --save <packageName>
npm update -S <packageName>
# 更新开发依赖环境下的版本号，并更新模块
npm update --save-dev <packageName>
npm update -D <packageName>
```



### 5-4 查看过时模快

`npm outdated` —— 查看是否有任何或特定安装的软件包当前已过时，会列出项目中过时的模块信息列表

当前版本（`current version`）、应当安装的版本（`wanted version`）和最新发布的版本（`latest version`）

```bash
# 查看当前项目过时的模块
npm outdated
# 查看全局安装的过时模块
npm outdated -g
npm outdated --global
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/202208011212071.png)



### 5-5 更新主版本

`npm update` 不会更新主版本有更新的模块，若要将所有软件包更新到新的主版本，则全局地安装 `npm-check-updates` 软件包

```bash
npm install -g npm-check-updates
```



运行 `ncu -u`  会升级 `package.json` 文件的 `dependencies` 和 `devDependencies` 中的所有版本，以便 `npm` 可以安装新的主版本

```bash
ncu -u
```



### 5-6 递归更新

从 `npm v2.6.1` 开始，`npm update` 只更新顶层模块，而不更新依赖的依赖，以前版本是递归更新的。如果想取到老版本的效果，要使用下面的命令

```bash
$ npm --depth 9999 update
```



## 5. 查看模块

### 5-1 搜索模块

`npm search` 命令用于搜索npm仓库，它后面可以跟字符串，也可以跟正则表达式

```bash
npm search <packageName>
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/202208011232723.png)



### 5-2 模块信息

`npm info` 命令可以查看每个模块的具体信息

```bash
# 查看模块详细信息
npm info <packageName>
# 查看模块的描述
npm info <packageName> description
# 查看模块的主页
npm info <packageName> homepage
# 查看模块的版本信息
npm info <packageName> version
```

![](https://raw.githubusercontent.com/xiaofeilalala/DocsPics/main/imgs/202208011235886.png)



### 5-3 依赖关系

`npm list` 命令以树型结构列出当前项目安装的所有模块，以及它们依赖的模块

```bash
# 本地安装所有模块信息
npm list
```

```bash
❯ npm list
/Users/joe/dev/node/cowsay
└─┬ cowsay@1.3.1
  ├── get-stdin@5.0.1
  ├─┬ optimist@0.6.1
  │ ├── minimist@0.0.10
  │ └── wordwrap@0.0.3
  ├─┬ string-width@2.1.1
  │ ├── is-fullwidth-code-point@2.0.0
  │ └─┬ strip-ansi@4.0.0
  │   └── ansi-regex@3.0.0
  └── strip-eof@1.0.0
```



获取顶层的软件包（基本上就是告诉 npm 要安装并在 `package.json` 中列出的软件包），则运行 `npm list --depth=0`

```bash
BASH
❯ npm list --depth=0
/Users/joe/dev/node/cowsay
└── cowsay@1.3.1
```



指定名称来获取特定软件包的版本, 也适用于安装的软件包的依赖

```bash
BASH
❯ npm list cowsay
/Users/joe/dev/node/cowsay
└── cowsay@1.3.1
```

```bash
BASH
❯ npm list minimist
/Users/joe/dev/node/cowsay
└─┬ cowsay@1.3.1
  └─┬ optimist@0.6.1
    └── minimist@0.0.10
```



查看软件包在 `npm` 上最新的可用版本

```bash
BASH❯ npm view cowsay version
1.3.1
```



### 5-4 语义化版本控制

语义版本控制始终以 3 个数字表示：`x.x.x`，第一个数字是主版本号，第二个数字是次版本号，第三个数字是补丁版本号

当发布新的版本时，必须遵循以下规则：

- 当进行不兼容的 `API` 更改时，则升级主版本
- 当以向后兼容的方式添加功能时，则升级次版本
- 当进行向后兼容的缺陷修复时，则升级补丁版本

版本更新规则符号：

- `^`: 只会执行不更改最左边非零数字的更新。 如果写入的是 `^0.13.0`，则当运行 `npm update` 时，可以更新到 `0.13.1`、`0.13.2` 等，但不能更新到 `0.14.0` 或更高版本。 如果写入的是 `^1.13.0`，则当运行 `npm update` 时，可以更新到 `1.13.1`、`1.14.0` 等，但不能更新到 `2.0.0` 或更高版本
- `~`: 如果写入的是 `〜0.13.0`，则当运行 `npm update` 时，会更新到补丁版本：即 `0.13.1` 可以，但 `0.14.0` 不可以
- `>`: 接受高于指定版本的任何版本
- `>=`: 接受等于或高于指定版本的任何版本
- `<=`: 接受等于或低于指定版本的任何版本
- `<`: 接受低于指定版本的任何版本
- `=`: 接受确切的版本
- `-`: 接受一定范围的版本。例如：`2.1.0 - 2.6.2`
- `||`: 组合集合。例如 `< 2.1 || > 2.6`
- 无符号: 仅接受指定的特定版本（`1.2.1`）
- `latest`: 使用可用的最新版本



## 6. 其它命令

### 6-1 清除缓存

`npm cache` 清理 `npm` 缓存文件夹

* `clean` —— 从缓存文件夹中删除所有数据
* `verify` —— 验证缓存文件夹的内容，垃圾收集任何不需要的数据，并验证缓存索引和所有缓存数据的完整性

```bash
# 验证缓存文件
npm cache verify
# 清理缓存文件
npm cache clean
# 强制清理（简写：npm cache clean -f）
npm cache clean --force
```



### 6-2 运行命令

`npm` 不仅可以用于模块管理，还可以用于执行脚本。`package.json` 文件有一个 `scripts` 字段，可以用于指定脚本命令，供 `npm` 直接调用

* `npm run` 是 `npm run-script` 的缩写
* `npm run` 命令会自动在环境变量 `$PATH` 添加 `node_modules/.bin` 目录，所以 `scripts` 字段里面调用命令时不用加上路径
* `npm run` 如果不加任何参数，直接运行，会列出 `package.json` 里面所有可以执行的脚本命令
* `npm` 内置了两个命令简写，`npm test` 等同于执行 `npm run test`，`npm start` 等同于执行 `npm run start`

```bash
npm run <task-name>
```

```json
{
  "scripts": {
    "watch": "webpack --watch --progress --colors --config webpack.conf.js",
    "dev": "webpack --progress --colors --config webpack.conf.js",
    "prod": "NODE_ENV=production webpack -p --config webpack.conf.js",
  },
}
```

```bash
npm run watch
npm run dev
npm run prod
```



### 6-2 模块发布

`npm publish` 用于将当前模块发布到 [http://npmjs.com](https://link.zhihu.com/?target=http%3A//npmjs.com)。执行之前，需要向 [http://npmjs.com](https://link.zhihu.com/?target=http%3A//npmjs.com) 申请用户名

```bash
npm adduser
```



如果已经注册过，就使用下面的命令登录

```bash
npm login
```



登录以后，就可以使用 `npm publish` 命令发布

```bash
npm publish
```



撤销发布的模块 `npm unpublish`

```bash
# 删除某个版本
npm unpublish <packageName>@<version>  
# 删除整个npm市场的包
npm unpublish <packageName> --force 
```



### 6-3 npx

`node` 安装后会提供 `npx` 命令，使用 `npx` 命令可以直接调用模块命令

下面是不使用 `npx` 调用 `mockjs` 中的 `random` 命令方式

```bash
node_modules/mockjs/bin/random
```



使用 `npx` 可以简化调用，他会在 `node_modules/.bin` 目录和环境变量 `$PATH` 中查找命令并执行

```bash
npx random
```

