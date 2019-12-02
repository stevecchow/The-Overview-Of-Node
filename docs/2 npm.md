## 本节内容

- [1 npm 是什么](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/2%20npm.md#1-npm-是什么)
- [2 npm 有哪些常用命令](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/2%20npm.md#2-npm-有哪些常用命令)
- [3 ]()

<br/>

## 1 npm 是什么

**概念：**

- NPM（node package manager），是世界上最大的开源库 生态系统
- 通俗来讲，其实就是一大群开发者，在这个平台（NPM）上，把自己开发出的 包，提供给别人
- NPM 平台上的包，都需要 nodeJS 提供支持，来进行下载和使用

**目的：**

- 为了让 开发人员 更方便 下载和使用 各种依赖包
- 当然，你也可以去 平台 发布 自己开发的 包，让别人来使用

**平台地址：**

[npmjs.com](npmjs.com)



## 2 npm 有哪些常用命令

**安装依赖：**

`npm install`

一次性把 dependencies 与 devDependencies 选项中的依赖项全部安装

简写：`npm i`

`npm install --production`

一次性把 dependencies 选项中的依赖项全部安装，除开 devDependencies

简写：`npm i --production`



**查看 npm 版本号：**

  - `npm -version`
  - `npm -v`
  - `npm --v`
  - `npm --version`
- 升级 npm
  - `npm install --global npm`

- 初始化项目
  - `npm init --yes`
  - npm init -y 是其简写，可以跳过向导，快速生成 package.json 
- 安装包
  - `npm install 包名`
  - 简写：npm i 包名
- 安装包 并保存依赖
  - `npm install --save 包名`
  - 下载并且保存依赖项（package.json 文件中的 dependencies 选项）
  - 简写：npm i -S 包名
- 删除包 不删除依赖
  - `npm uninstall 包名`
  - 只删除包，package.json 中的 依赖项 依然会保存下来
  - 简写：npm un 包名
- 删除包 并删除依赖
  - `npm uninstall --save 包名`
  - 不光删除包，还删除 package.json 中的 依赖项
  - 简写：npm un -S 包名
- 查看 使用帮助
  - `npm help`
- 查看 指定命令 的使用帮助
  - `npm 命令 --help`
  - 例如我忘了install 命令怎么用的时候，我可以输入 `npm install --help`



## 3 解决 npm 被墙问题

### 淘宝 cnpm

网址：http://npm.taobao.org/



### 安装 cnpm

```shell
# --global 表示安装到全局，而非当前目录
npm install cnpm -g
```



### 使用

```shell
# 这里还是走国外的 npm 服务器，速度比较慢
npm install jquery
# 这里使用 cnpm ，所以走国内淘宝服务器来下载，速度较快
cnpm install jquery
```



### 更改数据源

- 作用：既不必安装 cnpm ，又可以使用淘宝的源

- 方式一：临时更改数据源

- ```shell
  # 临时更改
  npm install jquery --registry=https://registry.npm.taobao.org
  ```

- 方式二：永久更改数据源

- ```shell
  # 永久更改
  npm config set registry https://registry.npm.taobao.org
  
  # 查看 npm 配置信息
  npm config list
  ```



## package.json 和 package-lock.json

| package.json                                                 | package-lock.json                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| npm 5之前就有了                                              | npm 5之后才有的                                              |
| 初始化生成                                                   | 安装包时生成                                                 |
| 使用 `npm install --save jquery` 等安装命令的时候，记得加上 `--save`，这样才能将把依赖信息放入 package.json 中 | 它会保存 mode_modules 中所有包的信息（版本、下载地址等），当你执行 `npm install` 的时候，可以更快地一次性安装完所有依赖包 |
| 建议每一个项目都要有一个 package.json 文件（包描述文件，就像产品的说明书一样），拥有描述整个项目的信息。让人能够在 node_modules 目录文件被删之后，还能根据 package.json 中的 dependencies ，将所有依赖包全部安装回来。 |                                                              |

