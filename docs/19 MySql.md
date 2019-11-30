### 19、MySQL

##### 19.1 安装支持包

```shell
npm i -s mysql
```



##### 19.2 部署代码

```javascript
var mysql = require('mysql');

// 创建连接
var connection = mysql.createConnection({
    host:'localhost',
    user:'root',
    password:'root'，
    database:'root'
});

// 连接数据库
connection.connect();

// 执行数据操作
connection.query('SELECT * FROM `users`',function(err,results,fields){
    if(err) throw err;
    console.log('the solution is:',results);
}); 

// 关闭连接
connection.end();
```



##### 19.3 mysql常用命令

- `mysql.server start`：开启服务
- `mysql.server stop`：关闭服务
- `mysql -u root`：链接数据库 root
- `exit`：退出该链接

