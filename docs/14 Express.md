### 14、Express

##### 14.1 概念

- Express 为 nodejs 的一个框架，框架的目的就是提高效率，让我们的代码更高度统一
- 原生 http 在某些方面的表现，不足以应对我们的开发需求，所以我们就需要使用 Express 来加快我们的开发效率
- 在 Node 中，有很多 Web 开发框架，我们这里以学习 express 为主



##### 14.2 安装

```shell
npm install --save express
```



##### 14.3 hello world

```javascript
// 1、引入 express
var express = require('express')

// 2、创建app
var app = express()

// 3、打开静态资源，路径访问权限
app.use('/public/',express.static('./public/'))
app.use('/static/',express.static('./static/'))

// 4、基本路由
app.get('/',(req,res) => {
    // 这里依然可以用 res.write() 与 res.end()，只不过没必要了
    // 发送过去的字符串，直接就是代码文件
	res.send('<h1>hello world express!</h1>')
})

app.get('/about',(req,res) => {
	res.send('<h1>this is about</h1>')
})

// 5、404 路由
// use 函数里面没有 第一个路径参数，所以任意未指定路径的访问，都会处理该 回调函数
app.use((req,res) => {
    res.send('<h1>404</h1>')
})

// 6、设置端口，提供服务
app.listen('3000',() => {
	console.log('app is running at port 3000...')
})
```



##### 14.4 路由

###### 14.4.1 概念

- 路由，其实就是一张表（地图），这张表里面有着各 url 具体的映射关系
- 可以理解为，当请求 /home 的时候，执行某函数。请求 /about 的时候，执行另一函数



###### 14.4.2 路由的构成

1. 请求方法
2. 请求路径
3. 处理函数



###### 14.4.3 get 与 post 样式

- get：

- ```javascript
  app
  	// 当你以 GET 方法请求 / 的时候，执行对应的处理函数
      .get('/',function (req,res) {
  		res.send('hello index');
      })
      .get('/login',function (req,res) {
  		res.send('hello login');
      })
      .get('/about',function (req,res) {
  		res.send('hello about');
      })
  ```

- post：

- ```javascript
  app
  	// 当你以 POST 方法请求 / 的时候，执行对应的处理函数
      .post('/',function (req,res) {
  		res.send('get a post request');
      })
      .post('/login',function (req,res) {
  		res.send('hello login');
      })
      .post('/about',function (req,res) {
  		res.send('hello about');
      })
  ```



###### 14.4.4 静态资源服务

- 方式一：路由 定位 盘符

  - 功能：根据设定的路由，来访问相应路径下的静态资源

  - 代码：

  - ```javascript
    app.use('/a/',express.static('./public/'));
    ```

  - 浏览器访问：

  - ```javascript
    127.0.0.1:3000/a/login.html
    ```

- 方式二：开放 该域内容

  - 功能：直接打开该域，其中所有的静态资源都可以直接用 / 进行访问（下级目录需要继续加 /xxx ）

  - 代码：

  - ```javascript
    app.use(express.static('./static/'));
    ```

  - 浏览器访问：

  - ```javascript
    127.0.0.1:3000/1.png
    ```

- 方式三：路由定位盘符下的特定资源

  - 功能：根据设定的路由，来访问相应路径下的指定静态资源

  - 代码：

  - ```javascript
    app.use('/a',express.static('./public/a.js'));
    app.use('/a.html',express.static('./public/a.js'));
    ```

  - 浏览器访问：

  - ```javascript
    127.0.0.1:3000/a
    127.0.0.1:3000/a.html
    ```



##### 14.5 配置 模板引擎

###### 14.5.1 安装

```shell
npm install --save art-template
npm install --save express-art-template
```



###### 14.5.2 配置

```javascript
app.engine('art',require('express-art-template'));
```



###### 14.5.3 使用

```javascript
app.get('/',(req,res) => {
    // express 默认会去项目中的 views 目录找 index.html
    res.render('./views/index.art',{
        title:'hello world'
    });
})
```



###### 14.5.4 views 目录

- 改变：

  - 在未使用 express 框架的情况下，要想 art-template 默认访问 views 目录，是需要设定绝对路径的
  - 使用 express 框架与 express-art-template 框架之后，views 变成了默认 视图渲染文件 保存目录，不需要再使用绝对路径，直接用 ./tem.art 即可访问，各种include、extend 依赖等，也只需根据相对路径设置即可

- 手动修改默认的 views 目录

- ```javascript
  // 注意：第一个参数 views 千万不要写错
  app.set('views',目录路径)
  ```



##### 14.6 获取 GET 请求体

###### 14.6.1 格式

```javascript
var q = req.query
```



##### 14.7 获取 POST 请求体

###### 14.7.1 安装

- 在 Express 中，没有内置获取 POST请求体 的 API，需要安装第三方包（中间件） `body-parser`

- 安装方式：

- ```shell
  npm install --save body-parser
  ```



###### 14.7.2 配置

```javascript
var express = require('express')
// 引包
var bodyParser = require('body-parser')

var app = express()
// 配置 body-parser
// 只要加入这个配置，则在 req 请求对象上会多出来一个属性：body
// 也就是说，你就可以直接通过 req.body 来获取表达 POST 请求体数据了

// 创建 application/x-www-form-urlencoded 解析
app.use(bodyParser.urlencoded({ extended: false }))

// 创建 application/json 解析
app.use(bodyParser.json())

app.use((req, res) => {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  // 可以通过 req.body 来获取表单 POST 请求体数据
  res.end(JSON.stringify(req.body, null, 2))
})
```



##### 14.8 响应方法

###### 14.8.1 res.send()

```javascript
app.use((req,res) => {
    res.send('<h1>404</h1>')
})

// 响应结果为：HTML文档
```



###### 14.8.1 res.(500).json(...)

```javascript
// 这里的 res 返回了 json 格式的数据，json() 在这里也包含 send() 的作用
// 以后，如果是返回错误信息，全部用 json()，不要用 send() 了
return res.status(500).json({	
    err_code: 500,
    message: 'Internal error.'
})
// 结果为：
// {"err_code":500,"message":"Internal error."}
```



##### 14.9 文件操作路径 和 模块标识路径

###### 14.9.1 文件操作路径

```javascript
// 相当于当前目录
./data/a.txt

// 相当于当前目录
data/a.txt

// 绝对路径，当前文件模块所处的 磁盘根目录
/data/a.txt

// 适用环境
fs.readFile('./data/a.txt',(error,data) => {
    console.log(data.toString());
})
```



###### 14.9.2 模块标识符路径

```javascript
// 绝对路径
require('/data/foo.js');		

// 相对路径，模块加载的路径中的相对路径
// 不能省略 ./
require('./data/foo.js');
```
