## 本节内容

- [1 npm 是什么](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/2%20npm.md#1-npm-是什么)
- [2 npm 有哪些常用命令](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/2%20npm.md#2-npm-有哪些常用命令)
- [3 npm 下载速度太慢](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/2%20npm.md#3-npm-下载速度太慢)
- [4  两个 json 文件](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/2%20npm.md#4-两个-json-文件)



<br/>



## 1 npm 是什么

**概念：**

NPM（node package manager），是世界上最大的开源库 生态系统

通俗来讲，其实就是一大群开发者，在这个平台（NPM）上，把自己开发出的 包，提供给别人

NPM 平台上的包，都需要 nodeJS 提供支持，来进行下载和使用

**目的：**

为了让 开发人员 更方便 下载和使用 各种依赖包

当然，你也可以去 平台 发布 自己开发的 包，让别人来使用

**平台地址：**

[npmjs.com](npmjs.com)



<br/>



## 2 npm 有哪些常用命令

**安装依赖：**

- 一次性把 dependencies 与 devDependencies 选项中的依赖项全部安装

```shell
npm install
```

它的简写方式，为：`npm i`

- 一次性把 dependencies 选项中的依赖项全部安装，除开 devDependencies

```shell
npm install --production
```


它的简写方式，为：`npm i --production`



**查看 npm 版本号：**

```shell
npm --version
```

下面还有三种简写方式

```shell
npm -version
npm -v
npm --v
```



**升级 npm：**

```shell
npm install npm --global
```



**初始化项目：**

初始化项目，可以跳过向导，快速生成 package.json 

```shell
npm init --yes
```

`npm init -y` 是其简写形式



**安装包：**

```shell
npm install 包名
```

`npm i 包名` 是其简写形式



**安装包 并保存依赖：**

下载并且保存依赖项（package.json 文件中的 dependencies 选项）

```shell
npm install --save 包名
```

`npm i -S 包名` 是其简写形式



**删除包 不删除依赖：**

只删除包，package.json 中的 依赖项 依然会保存下来

```shell
npm uninstall 包名
```

`npm un 包名` 为其简写形式



**删除包 并删除依赖：**

不光删除包，还删除 package.json 中的 依赖项

```shell
npm uninstall --save 包名
```

`npm un -S 包名` 为其简写形式



**查看 使用帮助：**

```shell
npm help
```



**查看 指定命令 的使用帮助：**

```shell
npm 命令 --help
```

例如：当我忘记 npm 的 install 命令应该怎样使用的时候，我可以输入 `npm install --help`



<br/>



## 3 npm 下载速度太慢

因为 npm 平台地址设在国外，所以要想提升 npm 的下载速度，就得解决 npm 的 被墙问题

幸运的是，我们有如下的几种解决方案

- 开通国外的vpn
- 使用淘宝提供的 cnpm工具
- 修改 npm 工具的镜像地址



**开通国外 vpn**

开通 vpn 的好处就是，从根本上解决数据访问不到（或缓慢）的问题

这个大家都懂的，这里就不提供具体的FQ指南了，请各位亲自行百度哦



**安装并使用 cnpm**

cnpm，性质跟 npm 工具一样，也是一个 node 工具。你可以把它看成一个 能在国内使用的、 npm 工具的 替代品。安装方式如下：

```shell
npm install cnpm -g
```

`--global` 表示安装到全局，而非当前目录

[cnpm 地址](http://npm.taobao.org/)

它的用法与 npm 相似：

```shell
# 这里还是走国外的 npm 服务器，速度比较慢
npm install jquery

# 这里使用 cnpm ，镜像为国内淘宝服务器，速度较快
cnpm install jquery
```



**修改 npm 镜像地址**

如果你不想开通 vpn，又不想使用 cnpm，那么我墙裂建议您 修改 npm 镜像地址，然后继续使用 npm 工具。这样，你还是可以使用原汁原味的 npm 工具，又不必担心 慢速问题。

所谓 修改npm 镜像地址，其实就是更改 npm 工具的 数据源。说白了就是修改 npm 工具获取数据的网址，修改为 国内各组织 为了解决 npm 的 慢速问题 而搭建的 镜像地址

此类 镜像地址 一般会比国外官网的镜像 版本 存在一定差距，但版本不会太久，一般不会影响使用。

更换数据源，一般有两种方式，你可以根据自己的需要进行操作：

- 临时更改数据源

```shell
# 临时更改
npm install jquery --registry=https://registry.npm.taobao.org
```

- 永久更改数据源

```shell
  # 永久更改
npm config set registry https://registry.npm.taobao.org

# 查看 npm 配置信息
npm config list
```



<br/>



## 4 两个 json 文件

>  package.json 和 package-lock.json

| package.json                                                 | package-lock.json                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| npm 5之前就有了                                              | npm 5之后才有的                                              |
| 初始化生成                                                   | 安装包时生成                                                 |
| 使用 `npm install --save jquery` 等安装命令的时候，记得加上 `--save`，这样才能将把依赖信息放入 package.json 中 | 它会保存 mode_modules 中所有包的信息（版本、下载地址等），当你执行 `npm install` 的时候，可以更快地一次性安装完所有依赖包 |
| 建议每一个项目都要有一个 package.json 文件（包描述文件，就像产品的说明书一样），拥有描述整个项目的信息。让人能够在 node_modules 目录文件被删之后，还能根据 package.json 中的 dependencies ，将所有依赖包全部安装回来。 |                                                              |

<br/>

---

<br/>

上一章：《[第一章 nodeJS 介绍](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md)》

<br/>

下一章：《[第三章 启动](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/3%20%E5%91%BD%E4%BB%A4.md#启动)》