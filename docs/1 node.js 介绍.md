## 本节内容

- [1 nodeJs 是什么](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md#1-nodeJs-是什么)
- [2 nodeJs 和 浏览器 JavaScript 的区别](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md#2-nodeJs-和-浏览器-JavaScript-的区别)
- [3 V8 引擎](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md#3-V8-引擎)
- [4 nodeJs 的特点](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md#4-nodeJs-的特点)
- [5 nodeJs 能做什么](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md#5-nodeJs-能做什么)
- [6 nodeJs 最常用的场景](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md#6-最常用的场景)
- [7 nodeJs 应用会用到的技术](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/1%20node.js%20%E4%BB%8B%E7%BB%8D.md#node-7-应用会用到的技术)

<br/>

## 1 nodeJs 是什么

要想学好一门语言，首先我们需要弄清楚它们到底是什么，对其有一个整体的把握。

首先我们应该明却一件事：Node.js 不是一门新的语言，也不是一个库或框架。它只是一个基于 Chrome V8 引擎的 EcmaScript 运行环境（runtime）。

而这个环境的唯一作用，就是在完全脱离浏览器的环境中，解析和执行 EcmaScript 代码。

<br/>

## 2 nodeJs 和 JavaScript

> nodeJS 和 JavaScript 有什么区别呢？

| 功能                 | JavaScript | nodeJs |
| -------------------- | ---------- | ------ |
| EcmaScript           | √          | √      |
| BOM                  | √          |        |
| DOM                  | √          |        |
| 文件读写（file）模块 |            | √      |
| 网络服务（http）模块 |            | √      |
| 操作系统（OS）模块   |            | √      |
| 路径操作（path）模块 |            | √      |

**注意：**

nodeJs 完全没有 BOM 和 DOM 这两块实现，所以不能使用 BOM 或者 DOM 的属性和方法。

浏览器中的 JavaScript 是没有文件操作等能力的，而 nodejs 中提供了实现这些功能的服务器级别 API。

<br/>

## 3 V8 引擎

从小到大，我们听到过太多的”引擎“，诸如：汽车引擎、飞机引擎、图像渲染引擎、代码引擎......汽车引擎可以驱动汽车在马路上前行，飞机引擎可以驱动飞机在天空中翱翔，那 V8 引擎的作用是什么呢？

其实，代码只是具有特定格式的字符串而已。而 引擎 可以认识它们，帮你去解析和执行这些字符串。上文提到过，引擎的唯一作用，就是在完全脱离浏览器的环境中，解析和执行 EcmaScript 代码。

通俗一点，就是：Node.js 的作者，把 Chrome V8 引擎从浏览器中移植了出来，开发了一个 独立于浏览器 的 JavaScript 运行环境。而你的代码，能在这个环境中跑起来。



**V8 的特点：**

Google Chrome 的 V8 ，是目前公认解析执行 JavaScript 代码 最快最高效 的引擎。



<br/>

## 4 nodeJs 的特点

- event-driven 事件驱动

  事件驱动程序的基本结构，是由一个事件收集器、一个事件发送器和一个事件处理器组成。说白了，如果我们要完成一个点击事件，就需要这三个步骤：点击 ---> 触发 ---> 作用。

- non-blocking I- /O model 非阻塞 I/O 模型

  意思就是：支持异步操作

- lightweight and efficient 轻量和高效

<br/>

## 5 nodeJs 能做什么

**web 服务器后台：**

用 node 为应用开发后台，处理请求与响应。当然，本书就是以开发 web 服务器为切入点，对 nodeJS 进行讲解。

**命令行工具：**

npm（使用 node 开发的）

hexo（使用 node 开发的）

git（使用 C 语言开发的，不过也可以用 node 进行开发）

**可视化（桌面）应用程序：**

可以使用原生的 Node.js 开发环境来开发 Electron 应用。

为了打造一个Electron桌面程序的开发环境，你只需要安装好的Node.js、npm、一个顺手的代码编辑器以及对你的操作系统命令行客户端的基本了解。

<br/>

## 6 最常用的场景

在本教程中，我们主要是使用别人开发的一些 nodeJS 工具，完成我们对 BS 项目的搭建，比如：

- webpack
- npm

但作为一个 node 开发工程师，我们不光要会使用他人开发的命令行工具。我们自己也可以写一些工具，放到 npm 上供他人使用。

<br/>

## 7 node 应用会用到的技术

> 下面如果涉及到一些不太理解的概念，不要着急，在随后的学习中，我们会一个一个全部弄懂它们。

**B/S 编程模型：**

Browser-Server，也就是 浏览器-服务器 编程模型。任何服务端技术诸如 Java、PHP 的 B/S 编程模型都是一样的。和语言无关，nodejs 只是实现它的一个工具而已。



**模块化编程：**

以前认识的 JavaScript 只能通过 `script` 标签来加载，但在 nodeJs 中，我们可以使用 `@import()` 来引用和加载文件。



**异步编程：**

回调函数、Promise、async await、generator，是我们学习异步编程的四个重点！在随后的学习中，我们会对这些概念进行重点讲解。



**Express 框架：**

当然，除了 Express也有很多其它的优秀框架值得我们去学习，比如：koa2。不过，在本教程中，我们将借助 express 框架，进行服务器的开发与讲解。



**EcmaScript 6：**

ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的新一代标准，已经在 2015 年 6 月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

相比 es5，它提供了一些新的功能：

- 模板字符串
- Promise
- 键值对简写
- Class 语法
- ......

<br/>

---

<br/>

上一章：没有了

<br/>

下一章：《[第二章 NPM](https://github.com/stevecchow/The-overview-of-node/blob/master/docs/2%20npm.md)》