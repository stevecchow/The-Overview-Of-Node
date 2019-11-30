## 1 nodejs 是什么

**概念：**

- 它不是一门语言、一个库或一个框架

- 它是一个基于 Chrome V8 引擎的 JavaScript 运行环境（runtime）

**作用：**

- 在完全脱离浏览器的环境中，解析和执行 JavaScript 代码



<br/>



## 2 nodejs 和 浏览器 JavaScript 的区别

|                      | JavaScript | nodejs |
| -------------------- | ---------- | ------ |
| EcmaScript           | √          | √      |
| BOM                  | √          |        |
| DOM                  | √          |        |
| 文件读写（file）模块 |            | √      |
| 网络服务（http）模块 |            | √      |
| 操作系统（OS）模块   |            | √      |
| 路径操作（path）模块 |            | √      |

**注意：**

- nodejs 完全就没有 BOM 和 DOM，所以不能使用 BOM 或者 DOM 的属性和方法
- 浏览器中的 JavaScript 是没有文件操作等能力的，nodejs 中提供了实现这些功能的服务器级别 API



<br/>



## 3 V8 引擎

**什么是引擎：**

- 代码只是具有特定格式的字符串而已。而 引擎 可以认识它们，帮你去解析和执行这些字符串



**V8 的特点：**

- Google Chrome 的 V8 ，是目前公认解析执行 JavaScript 代码 最快最高效 的引擎

- Node.js 的作者，把 V8 引擎移植了出来，开发了一个 独立于浏览器 的 JavaScript 运行环境



<br/>



## 4 nodejs 的特点

- event-driven 事件驱动

- non-blocking I/O model 非阻塞 I/O 模型（意思就是支持异步操作）

- lightweight and efficient 轻量和高效



<br/>



## 5 nodejs 能做什么

**web 服务器后台：**

- 用 node 为应用开发后台，处理请求与响应



**命令行工具：**

- npm（node 开发的）
- git（c语言开发的，不过也可以用 node 开发）
- hexo（node 开发的）



<br/>



## 6 最常用的场景

- 对于前端开发工程师来讲，接触 node 最多的是它的命令行工具

- 自己写工具的机会很少，主要是使用别人开发的第三方
  - webpack
  - gulp
  - npm



<br/>



## 7 node 应用会用到的技术

**B/S 编程模型：**

- Browser-Server
- back-end
- 任何服务端技术与 B/S 编程模型都是一样的，和语言无关，nodejs 只是实现它的一个工具而已



<br/>



**模块化编程：**

- 以前认识的 JavaScript 只能通过 `script` 标签来加载，在 nodejs 中可以使用 `@import()` 来引用和加载文件
- 支持模块化的 js
  - RequireJS
  - SeaJS



<br/>



**异步编程四个重点：**

- 回调函数
- Promise
- async await
- generator



<br/>



**Express 框架：**

- 借助框架，进行服务器开发



**EcmaScript 6：**

- 9.3 模板字符串
- comments:comments 可以简写为 comments



