### 15、CRUD 项目实战

##### 15.1 概念

- CRUD 是指在做计算处理时的增加(Create)、读取查询(Retrieve)、更新(Update)和删除(Delete)几个单词的首字母简写。主要被用在描述软件系统中 数据库 或者 持久层 的基本操作功能。



##### 15.2 模块化思想

- 模块职责要单一
- Vue、Angular、React 等框架，都是模块化框架



##### 15.3 路由设计

| 请求方法 | 请求路径         | get 参数 | post 参数                      | 备注             |
| -------- | ---------------- | -------- | ------------------------------ | ---------------- |
| GET      | /students        |          |                                | 渲染首页         |
| GET      | /students/new    |          |                                | 渲染添加学生页面 |
| POST     | /students/new    |          | name、age、gender、hobbies     | 处理添加学生请求 |
| GET      | /students/edit   | id       |                                | 渲染编辑页面     |
| POST     | /students/edit   |          | id、name、age、gender、hobbies | 处理编辑请求     |
| GET      | /students/delete | id       |                                | 处理删除请求     |



##### 15.4 模块设计

###### 15.4.1 入口 模块

```javascript
/**
 * app.js 入口模块
 * 职责：
 *   1 创建服务
 *   2 做一些服务相关配置
 *     模板引擎
 *     body-parser 解析表单 post 请求体
 *     提供静态资源服务
 *   3 挂载路由
 *   4 监听端口启动服务
 */

var express = require('express')
var router = require('./router')
var bodyParser = require('body-parser')

var app = express()

// 开放静态文件域
app.use('/node_modules/', express.static('./node_modules/'))
app.use('/public/', express.static('./public/'))

app.engine('art', require('express-art-template'))

// 配置模板引擎和 body-parser 一定要在 app.use(router) 挂载路由之前
// 创建 application/x-www-form-urlencoded 解析
app.use(bodyParser.urlencoded({ extended: false }))
// 创建 application/json 解析
app.use(bodyParser.json())

// 把路由容器挂载到 app 服务中
app.use(router)

app.listen(3000, () => {
  console.log('running 3000...')
})

module.exports = app
```



###### 15.4.2 路由 模块

```javascript
/**
 * router.js 路由模块
 * 职责：
 *   1 处理路由
 *   2 根据不同的请求方法+请求路径设置具体的请求处理函数
 * 注意：
 *   1 模块职责要单一，不要乱写
 * 目的：  
 *   1 我们划分模块的目的就是为了增强项目代码的可维护性
 *   2 提升开发效率
 */

var fs = require('fs')
var Student = require('./student')
var express = require('express')

// Express 提供了一种方式，专门用来包装路由的，不再使用 app.get() 等
// 1. 创建一个路由容器
var router = express.Router()

// 2. 把路由都挂载到 router 路由容器中

/*
 * 渲染学生列表页面
 */
router.get('/students', (req, res) => {
  Student.find((err, students) => {
    if (err) {
      res.status(500).send('Server error.')
    }
    res.render('index.art', {
      fruits: [
        '苹果',
        '香蕉',
        '橘子'
      ],
      students: students
    })
  })
})

/*
 * 渲染添加学生页面
 */
router.get('/students/new', (req, res) => {
  res.render('new.art')
})

/*
 * 处理添加学生
 */
router.post('/students/new', (req, res) => {
  // 1. 获取表单数据
  // 2. 处理
  //    将数据保存到 db.json 文件中用以持久化
  // 3. 发送响应
  Student.save(req.body, (err) => {
    if (err) {
      res.status(500).send('Server error.')
    }
    res.redirect('/students')
  })
})

/*
 * 渲染编辑学生页面
 */
router.get('/students/edit', (req, res) => {
  // 1. 在客户端的列表页中处理链接问题（需要有 id 参数）
  // 2. 获取要编辑的学生 id
  // 
  // 3. 渲染编辑页面
  //    根据 id 把学生信息查出来
  //    使用模板引擎渲染页面
  Student.findById(parseInt(req.query.id), (err, student) => {
    if (err) {
      res.status(500).send('Server error.')
    }
    res.render('edit.art', {
      student: student
    })
  })
})

/*
 * 处理编辑学生
 */
router.post('/students/edit', (req, res) => {
  // 1. 获取表单数据
  //    req.body
  // 2. 更新
  //    Student.updateById()
  // 3. 发送响应
  Student.updateById(req.body, (err) => {
    if (err) {
      res.status(500).send('Server error.')
    }
    res.redirect('/students')
  })
})

/*
 * 处理删除学生
 */
router.get('/students/delete', (req, res) => {
  // 1. 获取要删除的 id
  // 2. 根据 id 执行删除操作
  // 3. 根据操作结果发送响应数据

  Student.deleteById(req.query.id, function (err) {
    if (err) {
      res.status(500).send('Server error.')
    }
    res.redirect('/students')
  })
})

// 3. 把 router 导出
module.exports = router
```



###### 15.4.3 数据操作 模块

```javascript
/**
 * student.js 数据操作模块
 * 职责：
 * 1 操作文件中的数据，只处理数据，不关心业务
 *
 * 这里才是我们学习 Node 的精华部分：奥义之所在:封装异步 API
 */

var fs = require('fs')
var dbPath = './db.json'

/**
 * 获取学生列表
 * @param  {Function} callback 回调函数
 */
exports.find = (callback) => {
    fs.readFile(dbPath, 'utf8', (err, data) => {
        if (err) {
            callback(err)
        }
        callback(null, JSON.parse(data).students)
    })
}

/**
 * 根据 id 获取学生信息对象
 * @param  {Number}   id       学生 id
 * @param  {Function} callback 回调函数
 */
exports.findById = (id, callback) => {
    fs.readFile(dbPath, 'utf8', (err, data) => {
        if (err) {
            callback(err)
        }
        var students = JSON.parse(data).students
        var ret = students.find((item) => {
            return item.id == id
        })
        callback(null, ret)
    })
}

/**
 * 添加保存学生
 * @param  {Object}   student  学生对象
 * @param  {Function} callback 回调函数
 */
exports.save = (student, callback) => {
    fs.readFile(dbPath, 'utf8', (err, data) => {
        if (err) {
            callback(err)
        }
        var students = JSON.parse(data).students

        // 添加 id ，唯一不重复
        if (students.length != 0) {
            student.id = students[students.length - 1].id + 1
        } else {
            student.id = 1
        }

        // 把用户传递的对象保存到数组中
        students.push(student)

        // 把对象数据转换为字符串
        var fileData = JSON.stringify({
            students: students
        })

        // 把字符串保存到文件中
        fs.writeFile(dbPath, fileData, (err) => {
            if (err) {
                // 错误就是把错误对象传递给它
                return callback(err)
            }
            // 成功就没错，所以错误对象是 null
            callback(null)
        })
    })
}

/**
 * 更新学生
 */
exports.updateById = (student, callback) => {
    fs.readFile(dbPath, 'utf8', (err, data) => {
        if (err) {
            callback(err)
        }
        var students = JSON.parse(data).students

        // 注意：这里记得把 id 统一转换为数字类型
        student.id = parseInt(student.id)

        // 你要修改谁，就需要把谁找出来
        // EcmaScript 6 中的一个数组方法：find
        // 需要接收一个函数作为参数
        // 当某个遍历项符合 item.id === student.id 条件的时候，find 会终止遍历，同时返回遍历项
        var stu = students.find((item) => {
            return item.id == student.id
        })

        // 这种方式你就写死了，有 100 个难道就写 100 次吗？
        // stu.name = student.name
        // stu.age = student.age

        // 遍历拷贝对象
        for (var key in student) {
            stu[key] = student[key]
        }

        // 把对象数据转换为字符串
        var fileData = JSON.stringify({
            students: students
        })

        // 把字符串保存到文件中
        fs.writeFile(dbPath, fileData, (err) => {
            if (err) {
                // 错误就是把错误对象传递给它
                callback(err)
            }
            // 成功就没错，所以错误对象是 null
            callback(null)
        })
    })
}

/**
 * 删除学生
 */
exports.deleteById = (id, callback) => {
    fs.readFile(dbPath, 'utf8', (err, data) => {
        if (err) {
            callback(err)
        }
        var students = JSON.parse(data).students

        // findIndex 方法专门用来根据条件查找元素的下标
        var deleteId = students.findIndex((item) => {
            return item.id === parseInt(id)
        })

        // 根据下标从数组中删除对应的学生对象
        students.splice(deleteId, 1)

        // 把对象数据转换为字符串
        var fileData = JSON.stringify({
            students: students
        })

        // 把字符串保存到文件中
        fs.writeFile(dbPath, fileData, (err) => {
            if (err) {
                // 错误就是把错误对象传递给它
                callback(err)
            }
            // 成功就没错，所以错误对象是 null
            callback(null)
        })
    })
}
```



###### 15.4.4 数据库 文件

```javascript
{
    "students": [
    {
        "id": 4,
        "name": "张三三",
        "gender": "0",
        "age": "22",
        "hobbies": "吃饭、睡觉、打豆豆"
    },
    {
        "name": "stevechow",
        "gender": "0",
        "age": "24",
        "hobbies": "piano",
        "id": 5
    },
    {
        "name": "stevejobs",
        "gender": "0",
        "age": "20",
        "hobbies": "hello",
        "id": 6
    },
    {
        "name": "LEO",
        "gender": "0",
        "age": "23",
        "hobbies": "play game",
        "id": 7
    }]
}
```



###### 15.4.5 模板 文件

- index.art

- ```html
  <!DOCTYPE html>
  <html lang="en">
  
  <head>
      <meta charset="UTF-8">
      <title>学生信息-首页</title>
      <link rel="stylesheet" href="/public/css/bootstrap.css">
      <link rel="stylesheet" href="/public/css/main.css">
  </head>
  
  <body>
      <div class="box">
          <h1>学生信息表</h1>
          <div class="cla">班级：{{clas}}</div>
          <a href="/add"><div class="btn btn-primary">添加</div></a>
          <table class="table table-bordered">
              <thead>
                  <tr>
                      <th scope="col">编号</th>
                      <th scope="col">ID</th>
                      <th scope="col">姓名</th>
                      <th scope="col">性别</th>
                      <th scope="col">年龄</th>
                      <th scope="col">爱好</th>
                      <th scope="col">操作</th>
                  </tr>
              </thead>
              <tbody>
                  {{ each students }}
                  <tr>
                      <td>{{ $index + 1 }}</td>
                      <td>{{ $value.id }}</td>
                      <td>{{ $value.name }}</td>
                      <td>{{ $value.gender }}</td>
                      <td>{{ $value.age }}</td>
                      <td>{{ $value.hobbies }}</td>
                      <td>
                          <a href="/edit?id={{ $value.id }}"><div class="btn btn-info">编辑</div></a>
                          <a href="/delete"><div class="btn btn-danger">删除</div></a>
                      </td>
                  </tr>
                  {{ /each }}
              </tbody>
          </table>
      </div>
  </body>
  
  </html>
  ```

- add.art

- ```html
  <!DOCTYPE html>
  <html lang="en">
  
  <head>
      <meta charset="UTF-8">
      <title>学生信息-添加学生</title>
      <link rel="stylesheet" href="/public/css/bootstrap.css">
      <link rel="stylesheet" href="/public/css/main.css">
  </head>
  
  <body>
      <div class="box">
          <h1>添加学生</h1>
          <form action="/add" method="post">
              <div class="form-group">
                  <label for="name">姓名</label>
                  <input type="text" class="form-control" id="name" name="name" placeholder="例：周泓名">
              </div>
              <div class="form-group">
                  <label for="sex">性别</label>
                  <input type="text" class="form-control" id="sex" name="gender" placeholder="例：男">
              </div>
             <div class="form-group">
                  <label for="age">年龄</label>
                  <input type="text" class="form-control" id="age" name="age" placeholder="例：24">
              </div>
              <div class="form-group">
                  <label for="hobby">爱好</label>
                  <input type="text" class="form-control" id="hobby" name="hobbies" placeholder="例：钢琴">
              </div>
              <button type="submit" class="btn btn-default">提交</button>
          </form>
      </div>
  </body>
  
  </html>
  ```

- edit.art

- ```html
  <!DOCTYPE html>
  <html lang="en">
  
  <head>
      <meta charset="UTF-8">
      <title>学生信息-编辑</title>
      <link rel="stylesheet" href="../public/css/bootstrap.css">
      <link rel="stylesheet" href="../public/css/main.css">
  </head>
  
  <body>
      <div class="box">
          <h1>编辑学生</h1>
          <form action="/edit" method="post">
          	<div class="hidden">
                  <label for="id">ID</label>
                  <input type="text" class="form-control" id="id" name="id" value="{{ student.id }}">
              </div>
              <div class="form-group">
                  <label for="name">姓名</label>
                  <input type="text" class="form-control" id="name" name="name" value="{{ student.name }}">
              </div>
              <div class="form-group">
                  <label for="sex">性别</label>
                  <input type="text" class="form-control" id="sex" name="gender" value="{{ student.gender }}">
              </div>
             <div class="form-group">
                  <label for="age">年龄</label>
                  <input type="text" class="form-control" id="age" name="age" value="{{ student.age }}">
              </div>
              <div class="form-group">
                  <label for="hobby">爱好</label>
                  <input type="text" class="form-control" id="hobby" name="hobby" value="{{ student.hobbies }}">
              </div>
              <button type="submit" class="btn btn-default">提交</button>
          </form>
      </div>
  </body>
  
  </html>
  ```



##### 15.5 项目步骤

1. 入口模块设计（app.js）
   1. 引入 express
   2. 引入 router
   3. 引入 bodyParser
   4. 创建 express 的 app
   5. 开放 静态资源
   6. 设置模板引擎文件格式
   7. 设置 urlencoded 与 json 的解析方式
   8. 挂载 router 到 app 中
   9. 设置服务器端口
   10. 导出 app
2. 路由模块设计（router.js）
   1. 引入 fs
   2. 引入 数据操作模块（student.js）
   3. 引入 express
   4. 创建 express 的 router
   5. 创建路由 get/post 
      1. req.query
      2. req.body
   6. 导出 router
3. 数据操作模块设计（student.js）
   1. 引入 fs
   2. 创建 数据库路径
   3. 创建 数据库操作方法（增删查改）
4. 处理模板
   1. index.art
   2. add.art
   3. edit.art
