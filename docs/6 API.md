### 6、API（模块）

##### 6.0 种类

- 核心模块：
  - `fs`、`http`、`os`、`path`、`url`
- 第三方模块：
  - 例如：
    - art-template（需要npm下载）
  - 注意：
    - 第三方模块，经 npm 的下载后，会被保存到 node_modules 目录中
    - 使用 require 引用的时候，会自动到 node_modules 里面寻找。之所以这么做，是为了方便大家少写路径
    - `var template = require('art-template')`
      - 如果包不在 node_modules 目录下，此简便方式无效
      - 如果 node_modules 目录，与 app.js(入口文件) 不在同一目录，此简便方式无效，这种情况的引用为：`var template = require(./a/node_modules/art-template/index.js);`
- 自定义模块：
  - 自己创建的模块



##### 6.1 commonJS模块规范

###### 6.1.1 模块化

- 如果一个平台支持以下**两件事情**，那么就符合模块化
  - 文件作用域
  - 通信规则



###### 6.1.2 JavaScript 与 模块

- JavaScript 本身是不支持模块化的，但Node中的JavaScript是一个模块系统，因为它支持：
  - 模块作用域
  - 使用 require 方法用来加载模块
  - 使用 exports 接口对象用来导出模块中的成员



###### 6.1.3 require 简介

- 语法：`var 自定义模块变量 = require('模块名')`
- 作用：
  - 执行被加载模块中的代码
  - 得到被加载模块中的 `exports` 导出的 **接口对象**



###### 6.1.4 exports 简介

- 概念：导出接口对象

- 作用：

  - nodejs 的作用域为 模块作用域，所以该文件中的所有成员，只有在当前文件内有效。要想被其它模块访问此模块中的成员，需要把这些想要公开的成员，挂载到 exports 接口对象中。

- 使用方式：

  - 导出多个成员：

    - 方式一：

    - ```javascript
      exports.a = 123;
      exports.b = 'hello';
      exports.c = function(){
          console.log('我是c');
      }
      exports.d = {
          foo:'bar';
      }
      ```

    - 方式二：

    - ```javascript
      module.exports = {
          a = 123,
          b = 'hello',
          c = function(){
          	console.log('我是c');
      	}
      	d = {
          	foo:'bar';
      	}
      }
      ```

  - 导出单个成员：（拿到的就是函数、字符串）

    - 格式：

    - ```javascript
      module.exports = 'hello world';
      ```

    - 注意：

      - `module.exports = 'hello world';`

        这种写法是 正确 的，能够生效

      - `exports = 'hello world';`

        这种写法是 错误 的，不能生效

    - 覆盖：

    - ```javascript
      module.exports = 'hello world';
      
      // 这个覆盖了前面那一个 module.exports
      module.exports = function(x,y){
          return x + y;
      }
      ```



###### 6.1.5 exports 与 module.exports 的区别

- 两者的指向相同

- ```javascript
  // 结果为 true，代表他们指向同一个地址
  exports == module.exports; 
  
  // 其实是因为，在系统里面的设定为如下，module.exports 的地址传给了 exports
  exports = module.exports;
  ```

- 类比

- ```javascript
  module.exports = {
      name:'steve',
      age:18
  }
  
  // change 指向obj开辟的内存地址
  var change = module.exports;	
  
  change.class = '小学一年级';
  
  // change 指向改变
  change = 10;	
  
  // 结果为 10
  console.log(change); 
  
  // 结果为 { name: 'steve', age: 18, class: '小学一年级' }
  console.log(module.exports); 
  ```



###### 6.1.6 require 加载规则

```javascript
//------------ main.js -----------------//
require('./a')
require('./b')

//-------------- a.js ------------------//
require('./b')
console.log('a.js 被加载了')

//-------------- b.js ------------------//
console.log('b.js 被加载了')

//---------- node main.js 结果 ----------//
// b.js 被加载了
// a.js 被加载了
```

- 规则：
  - 加载模块的时候，优先从 缓存加载
  - 由于 a.js 中已经加载过 b.js 了，所以当 main.js 加载 b.js 的时候，并不重复执行
- 优点：
  - 这样做的目的，是为了避免重复加载，提高模块的加载效率



##### 6.2 fs 文件操作模块

###### 6.2.1 概念

fs 是 fileSystem 的缩写，就是 文件系统 的意思。要想进行 文件操作，就必须引入 `fs` 这个核心模块



###### 6.2.2 引入模块

`var fs = require('fs');`



###### 6.2.3 readFile()

   - 作用：

        - 读取文件

   - 格式：

     `fs.readFile(filename[, options], callback)`

   - 参数：
     - 参数一：文件路径
     - 参数二：为具体选项配置，包括数据的编码方式，比如 utf-8，代表将读取的数据，转化成 utf-8 编码格式
     - 参数三：回调函数

   - 案例：

   - ```javascript
     fs.readFile(path.join(__dirname, 'a.js'), (err,data) => {
         if (err) throw err;
         // 这里将 16进制字符 转化成我们 能够认识的 文字或字符
         console.log(data.toString());
     });
     ```

   - error

     读取文件的时候，error 和 data 的值

     | 成功        | 失败            |
     | ----------- | --------------- |
     | data：数据  | data：undefined |
     | error：null | error：错误对象 |

   - 注意：

     - 书写文件路径参数的时候，就算是在同一目录下，也一定要在路径前面加上 `./` ，不然可能读取不到文件

   - 动态绝对路径：

     - 当前 node进程 的执行路径：

     - ```javascript
       // 结果为 /usr/local/bin/node
       var nPath = process.execPath;	
       ```

     - 当前代码的绝对路径：

     - ```javascript
       // 结果为 /Users/zhouhongming/Desktop/test/nodejs/
       var wwwDir = __dirname; 
       
       // 结果为 /Users/zhouhongming/Desktop/test/nodejs/
       var wwwDir = process.cwd(); 
       
       // 结果为 /Users/zhouhongming/Desktop/test/nodejs/a.js
       var fileDir = __filename;
       ```

     - 使用绝对路径的好处：

       - 为了尽量避免相对路径问题，建议文件操作的相对路径，都转为：**动态的绝对路径**

       - 绝对路径，是绝对不会出错的




###### 6.2.4 writeFile()

- 作用：

  - 写入文件

- 格式：

  `fs.writeFile(filename, data[, options], callback)`

- 参数：

  - 参数一：文件路径
  - 参数二：文件内容
  - 参数三：为具体的保存文件配置，编码格式等
  - 参数四：回调函数

- 案例：

- ```javascript
  fs.writeFile('haha.js','大家好，我是node.js',(err) => {
      if(err){
          console.log('文件创建失败')
      }else{
          console.log('文件创建成功');
      }
  });
  ```



###### 6.2.5 readdir()

- 作用：

  - 读取目录，并获取 文件列表数组

- 格式：

  `fs.readdir(path[, options], callback)`

- 参数：

  - 参数一：目录路径
  - 参数二：为具体选项配置，包括数据的编码方式
  - 参数三：回调函数

- 案例：

- ```javascript
  var fs = require('fs');
  // 该方法是为了判断目录 是否存在 ，并获取目录中的 所有文件
  fs.readdir(__dirname, (err,files) => {
      if (err) {
          return console.log('目录不存在');
      }
      // files 是数组，亦是对象
      // ['1.png', '2.png', 'a.js', 'b.js', 'test.js']
      console.log(files);  
  });
  ```



##### 6.3 http 网络操作模块

###### 6.3.1 概念

http 模块是专门用来操作网络请求的模块



###### 6.3.2 引入模块

 `var http = require('http');`



###### 6.3.3 最简单的 http 服务

```javascript
// 1、加载 http 核心模块
var http = require('http');

// 2、使用http.createServer()方法创建一个Web服务器
var server = http.createServer();

// 3、为服务器绑定 监听事件
server.on('request',function (req,res) {
    console.log('收到客户端的请求了');
});

// 4、绑定端口号，启动服务
server.listen(3000,function () {
    console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问');
});
```



###### 6.3.4 完整的 http 服务

```javascript
// 1、加载 http 核心模块
var http = require('http');

// 2、使用http.createServer()方法创建一个 Web 服务器
var server = http.createServer();

// 3、为服务器绑定 监听事件
server.on('request', function(req, res) {
	
    // 这里 url 取的值，是地址栏中链接的第一个 / 开始的地方。
    var url = req.url;
    console.log('收到客户端的请求了，请求路径是:' + url);
    
    // 根据请求url，书写内容
    if (url == '/') {
        res.write('index.html');
    }
    if (url == '/login') {
        res.write('this is login page!');
    }
    if (url == '/regist') {
        res.write('this is regist page!');
    }
    
    // 写完后，一定要有结束函数!
    // end 函数内可以传字符串，代替 write 函数
    res.end();
});

// 4、绑定端口号，这里绑定的是3000，但也可以80，这样直接用127.0.0.1就可以访问
server.listen(3000, function() {
    console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问');
});
```



###### 6.3.5 request 与 response

  - request
    - 该对象为请求对象，为callback的第一个参数
    - 该对象用来获取客户端的请求信息
  - response
    - 该对象为响应对象，为callback的第二个参数
    - 该对象用来给客户端响应信息（注意：只能响应 二进制 与 字符串，数字、数组、对象、布尔等通通不行



###### 6.3.7 Content-Type

- 案例：

- ```javascript
  server.on('request',function (req,res) {
      
      // 打印结果为：hello 涓栫晫
      res.end('hello 世界');
  });
  ```

- 分析：

  - 我们发现，页面上出现了乱码
  - 因为服务器用 UTF-8 包装的字符串，被 nodejs 默认使用 当前计算机系统的默认编码（GBK）进行了解析，所以，我们需要设置响应头的响应内容类型

- 设置 Content-Type

  - 设置 response 的响应头，告诉浏览器使用正确的 传输内容编码格式

  - ```javascript
    res.setHeader('Content-Type','text/plain;charset=utf-8');
    ```

  - 参数一：

    - 表示设置 响应头 的某个键

  - 参数二：

    - 第一个字符串：（MIME类型）
      - text/plain：文本文档
      - text/html：HTML 文档
      - image/jpeg：jpg 文件
    - 第二个字符串：（编码格式）
      - utf-8：表示用 utf-8 编码模式进行解析
      - GBK：表示用 GBK 编码模式进行解析



##### 6.4 os 系统操作模块

###### 6.4.1 概念

os 模块是专门用来获取 系统信息 的模块



###### 6.4.2 引入模块

`var os = require('os')`



###### 6.4.3 获取系统信息

```javascript
var os = require('os');
// 获取当前机器的 CPU 信息
console.log(os.cpus());
// 获取内存信息
console.log(os.totalmem());
```



##### 6.5 path 路径操作模块

###### 6.5.1 概念

path 模块是专门用来 操作文件路径 的模块



###### 6.5.2 引入模块

`var path = require('path')`



###### 6.5.3 常用方法

- `path.join()`

  - 功能：拼接 绝对路径

  - 案例：

  - ```javascript
    // 结果为：Users/zhouhongming/Desktop/test/nodejs/index.html
    path.join(__dirname, 'index.html'); 
    ```

- `path.extname()`

  - 功能：获取文件 后缀名

  - 案例：

  - ```javascript
    // 结果为：txt
    console.log(path.extname('hello.txt'));
    ```



##### 6.6 url 模块

###### 6.6.1 概念

url 模块提供了一些实用函数，用于 URL 处理与解析。



###### 6.6.2 地址栏 url

- 概念：
  - 地址栏的 url 是 资源定位符，不一定是文件路径！
  - 它只是一个标示，你可以随意取名字，但最好还是取有意义的名字
  - 所定位的资源种类不限，可以是：图片（jpg、png）、文件（mp3）、文档（js、md、java）等等
- 举例：
  - 我们可以用 http://127.0.0.1:3000/steve 来定位任何资源



###### 6.6.3 引入模块

`var url = require('url')`



###### 6.6.4 常用方法

- `url.parse('url地址',boolean)`

  - 功能：该方法将 url地址字符串，转化为 Url对象 ，对象里面有该url的 各种参数

  - 用法：

    - `url.parse('http://apple.com:8080?name=steve&age=18');`

      ```javascript
      Url {
        protocol: 'http:',
        slashes: true,
        auth: null,
        host: 'apple.com:8080',
        port: '8080',
        hostname: 'apple.com',
        hash: null,
        search: '?name=steve&age=18',
        query: 'name=steve&age=18',
        pathname: '/',
        path: '/?name=steve&age=18',
        href: 'http://apple.com:8080/?name=steve&age=18' }
      ```

    - `url.parse('http://apple.com:8080?name=steve&age=18',true);`

      - 这种方式下的 query 属性，会变为一个对象 {name:'steve'，age:'18'}

      - ```javascript
        Url {
          protocol: 'http:',
          slashes: true,
          auth: null,
          host: 'apple.com:8080',
          port: '8080',
          hostname: 'apple.com',
          hash: null,
          search: '?name=steve&age=18',
          query: [Object: null prototype] { name: 'steve', age: '18' },
          pathname: '/',
          path: '/?name=steve&age=18',
          href: 'http://apple.com:8080/?name=steve&age=18' }
        ```



