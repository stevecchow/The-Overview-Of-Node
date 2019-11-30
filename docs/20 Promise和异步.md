### 20、Promise和异步

##### 20.1 概念

- Promise 是 EcmaScript 6 中新增的 API
- 它跟 Node 与 Mongoose 等都没有关系，不过能够很好地支持它们



##### 20.2 回调地狱

![1.png](/Users/stevechow/Desktop/CircleLife/学习/后端/3-Node.js/pics/1.png)

###### 20.2.1 回调地狱是什么

- 当需要操作的异步数据过多时，代码嵌套就会过于冗余，这就是回调地狱



###### 20.2.2 为何要解决

- 例一：过多的嵌套不易维护，两三个还好，万一有七八个以上呢？比如：

- ```javascript
  get('http://127.0.0.1:3000/users/4', function (userData) {
      get('http://127.0.0.1:3000/jobs', function (jobsData) {
          // 这里需要操作两个函数的参数，故 不得不 使用 函数嵌套
          console.log(userData,jobsData)
      })
  })
  ```

- 例二：异步方法不能保证代码的执行顺序，要想保证顺序，就必须嵌套。比如：

- ```javascript
  fs.readFile('./a.txt','utf8',function (err,data) {
      if (err) {
          throw err;
      } 
      console.log(data);
      fs.readFile('./b.txt','utf8',function (err,data) {
          if (err){
              throw  err;
          } 
          console.log(data);
      })
  })
  ```



##### 20.3 如何解决（使用Promise）

- 使用 EcmaScript 6 新增的 `Promise API`



###### 20.3.1 声明 Promise 对象

```javascript
var fs = require('fs')

var p1 = new Promise(function (resolve, reject) {
    fs.readFile('./data/a.txt', 'utf8', function (err, data) {
        if (err) {
            // 如果读取文件错误，那么将 err 传给 reject 进行处理
            reject(err)
        } else {
            // 如果读取文件成功，那么将 data 的结果传给 resolve 函数进行处理
            // resolve 会将其传递给 then() 的回调函数的第一个参数
            resolve(data)	
        }
    })
})

var p2 = new Promise(function (resolve, reject) {
    fs.readFile('./data/b.txt', 'utf8', function (err, data) {
        if (err) {
            reject(err)
        } else {
            // 如果将 a.txt 删除，这里是能够正确读出 b.txt 的，但是下面 p2 
            // then 里面的 回调函数 中的 data ，打印为 undefined
            resolve(data)
        }
    })
})

var p3 = new Promise(function (resolve, reject) {
    fs.readFile('./data/c.txt', 'utf8', function (err, data) {
        if (err) {
            reject(err)
        } else {
            resolve(data)
        }
    })
})

p1                 
	// 只要它是一个 Promise 对象，它就可以 then             
    .then(function (data) {
    console.log(data)
    // 在这里 return 了 p2 , p2 为 Promise 对象
    return p2                   
}, function (err) {
    console.log('读取文件失败了', err)
})
	// 如果 a.txt 被删除，则 a.txt 读取失败。
	// 这里也是可以执行的，只不过，不是执行的 p2，因为 p2 没有被成功 return
	// 如果成功 return 了 p2 ，那么这个 function 成了 p2 的 resolve
    .then(function (data) {
    // 如果 p2 被成功return，这里打印为 b.txt，不成功为 undefined
    console.log(data)
    // 在这里 return 了 p3
    return p3
})
	// 那么，自然这个 function 成了 p3 的 resolve
    .then(function (data) {		
    console.log(data)
    console.log('end')
})
```



###### 20.3.2 构造 Promise 方法

```javascript
var fs = require('fs')

function pReadFile(filePath) {
    return new Promise(function (resolve, reject) {
        fs.readFile(filePath, 'utf8', function (err, data) {
            if (err) {
                reject(err)
            } else {
                resolve(data)
            }
        })
    })
}

pReadFile('./data/a.txt')       // 只要它是一个 Promise 对象，它就可以 then
    .then(function (data) {
    console.log(data)
    return pReadFile('./data/b.txt')
})
    .then(function (data) {
    console.log(data)
    return pReadFile('./data/c.txt')
})
    .then(function (data) {
    console.log(data)
})
```



###### 20.3.3 jQuery 的 Promise

- 官方用法一：

- ```javascript
  var data = {};
  $.get('http://127.0.0.1:3000/users/4')
      .then(function (user) {
          data.user = user
          return $.get('http://127.0.0.1:3000/jobs')
      })
      .then(function (jobs) {
          data.jobs = jobs
          var htmlStr = template('tpl', data)
          document.querySelector('#user_form').innerHTML = htmlStr
      })
  ```



- 官方用法二：

- ```javascript
  $(function () {
      $('#btn').on('click', () => {
          $.ajax({
              url: './user.json',
              type: 'get',
              dataType: 'json'
          }).then((data) => {
              data.user =
          })
      })
  })
  ```



- 自定义Promise get()：

- ```javascript
  // 定义
  function pGet(url, callback) {
      return new Promise(function(resolve, reject) {
          var xhr = new XMLHttpRequest()
          // 当请求加载成功之后要调用指定的函数
          xhr.onload = function() {
              // 我现在需要得到这里的 xhr.responseText
              callback && callback(JSON.parse(xhr.responseText))
              resolve(JSON.parse(xhr.responseText))
          }
          xhr.onerror = function(err) {
              reject(err)
          }
          xhr.open("get", url, true)
          xhr.send()
      })
  }
  
  // 使用一
  var data = {}
  pGet('http://127.0.0.1:3000/users/4')
      .then(function (user) {
      data.user = user
      return pGet('http://127.0.0.1:3000/jobs')
  })
      .then(function (jobs) {
      data.jobs = jobs
      var htmlStr = template('tpl', data)
      document.querySelector('#user_form').innerHTML = htmlStr
  })
  
  // 使用二
  pGet('http://127.0.0.1:3000/users/4', function (data) {
      console.log(data)
  })
  ```



##### 20.4 async await


