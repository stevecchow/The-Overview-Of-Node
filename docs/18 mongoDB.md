### 18、MongoDB

##### 18.1 概念

- 是一个基于 分布式文件存储 的数据库

- 是一个介于 关系数据库 和 非关系数据库 之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的

- 它的结构就像是一个大对象

- ```javascript
  var a = {
      name:'民企资料收藏集',
      year:20181025,
      qq:{
          name:'qq',
          des:'马化腾的赚钱公司'
      },
      taobao:{
          name:'淘宝',
          des:'马云旗下的一家公司',
          add:'杭州'
      },
      baidu:{
          name:'百度',
          des:'李彦宏的公司',
          add:'背景',
          pro:['搜索','os','IT','购物']
      }
  }
  ```



##### 18.2 安装 并 连接 MongoDB

###### 18.2.1 使用 brew 安装

- 使用 brew 命令安装

```shell
brew install MongoDB
# /usr/local/Cellar/mongodb/4.0.4_1: 18 files, 259.8MB
```



- 创建 数据库 目录 并添加读写文件权限

```shell
mkdir -p /data/db

sudo chown `id -u` /data/db
```



- 添加环境变量

```shell
vi ~/.bash_profile

# 添加mongodb安装目录到环境变量中
export PATH=/usr/local/Cellar/mongodb/4.0.2/bin:${PATH}}

# 使环境变量生效
source ~/.bash_profile
```



- 更改 MongoDB 配置文件

```shell
cd /usr/local/etc
vi mongod.conf

# 复制下面内容，并覆盖 mongod.conf
# Store data in /usr/local/var/mongodb instead of the default /data/db
dbpath = /data/db
 
# Append logs to /usr/local/var/log/mongodb/mongo.log
logpath = /usr/local/var/log/mongodb/mongo.log
logappend = true
 
# Only accept local connections
bind_ip = 127.0.0.1
```



###### 18.2.2 查看版本号

```shell
mongod --version
```



###### 18.2.3 启动 MongoDB

```shell
mongod
```



###### 18.2.4 连接数据库

```shell
mongo
```



###### 18.2.5 关闭数据库服务

```shell
# 要想关掉数据库服务
# 直接在 mongod 命令下的窗口：control + c

# 只是断开连接，在 mongo 窗口使用 exit
exit
```



##### 18.3 基本命令和操作

```shell
# 查看所有数据库
show dbs 							

# 查看该库下的所有 集合
show collections                    

# 查看当前操作的数据库
db

# 切换到指定的数据库(如果没有会新建)
use 数据库名称

# 创建 键值对 数据
db.students.insert({"name":"steve","age":20});

# 查询当前 集合 下面的所有的键值对
db.students.find()

# 删除该数据库下的 所有集合
 db.dropDatabase()
 
 # 删除该数据库下的 某一集合
 db.collection.drop()
```



##### 18.4 mongoose 基础

###### 18.4.1 概念

- 它是一种模型工具，为了让我们更方便地操作 MongoDB
- 其实就是对官方MongoDB的操作，再一次进行了封装
- 为 Promise 提供了全面支持



###### 18.4.2 资料

- 官网：https://mongoosejs.com/
- 官方指南：https://mongoosejs.com/docs/guide.html
- 官方 API 文档：https://mongoosejs.com/docs/api.html



###### 18.4.3 安装

```shell
npm i mongoose --save
```



###### 18.4.4 操作流程

1. 设计 Schema
2. 发布 Model（得到模型构造函数）
   - 查询
   - 增加
   - 删除
   - 修改



###### 18.4.5 hello world 案例

```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/steve');

// 这里必须用 Cat 这类大写开头的词
// 这样数据库就会自动生成 cats 集合
const Cat = mongoose.model('Cat', {name: String});	

for (var i=0;i<100;i++){
    const kitty = new Cat({name: '喵'+i});
    kitty.save().then(() => console.log('hello world'));
}
```



##### 18.5 mongoose 操作

###### 18.5.1 增添

```javascript
// 用模型创建实例
var steve = new User({
    username: 'steve',
    password: '123456',
    email: 'admin@admin.com'
})

// 储存实例
steve.save(function (err, ret) {
    if (err) {
        console.log('保存失败')
    } else {
        console.log('保存成功')
        // ret 打印结果为刚刚 保存的这条数据 
        // { "_id" : ObjectId("5bd1820adfa42e4e8f693ebc"),..."__v" : 0 }
        console.log(ret)
    }
})
```



###### 18.5.2 删除

- 条件删除所有

```javascript
User.remove({
    username:'zhm'
},function (err) {
    if (err){
        console.log('删除失败')
    } else {
        console.log('删除成功')
    }
})
```



- 根据条件删除一个

```javascript
User.findOneAndRemove(conditions,[options],[callback])
```



- 根据 id 删除一个

```javascript
User.findByIdAndRemove(id,[options],[callback])
```



###### 18.5.3 查询

- 查询所有

```javascript
// 查询 users 集合下面的所有数据
User.find(function(err,ret){
    if (err){
        console.log('查询失败')
    } else {
        console.log(ret)
    }
})
```



- 条件查询

```javascript
User.find({
    username:'zhm',
    password:123456
},function(err,ret){
    if (err){
        console.log('查询失败')
    } else {
        console.log(ret)
    }
})
```



###### 18.5.4 修改

- 查询 id 并修改

```javascript
// 参数依次为：id，Object，callback
User.findByIdAndUpdate('5bd197d990f4c653a3eb68b0',{
    password: 'steve123'
},function (err,ret) {
    if (err){
        console.log('更新失败')
    } else {
		// 疑惑点：这里一直都是更新成功，并且 ret 第一次显示的是未更改的对象，第二次刷新才是更改后的对象
        console.log('更新成功，结果为：'+ret)
    }
});
```



- Promise 的 then() 修改

```javascript
// 因为 mongoose 已经做了 Promise 兼容，这里直接可以使用 then，User 本身就是 Promise 对象
User.findOne({
    username:'admin'
})
// 这里用到了 then，表示查询后执行的程序
    .then(function (user) {
    if (user) {
        // 用户已注册，不能注册
        console.log('用户已存在')
    } else {
        // 用户不存在，就注册
        return new User({
            username:'admin',
            password:'123',
            email:'290689305@qq.com'
        }).save()
    }
})
	// 这里之所以可以用 then，是因为上一个 then() 中返回了一个 Promise 对象
    .then(function (user) {
    // 可以在这里操作 user ，user 就为 save() 函数的结果
})
```

