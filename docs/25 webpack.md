### 25、webpack

##### 25.1 概念

- webpack 就是模块打包工具，分析你的项目结构，将各类源代码（TypeScript、SCSS）转化 成浏览器可以直接执行的 JavaScript、CSS代码等
- 功能详解
  - 代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS
  - 文件优化：压缩JavaScript、CSS、HTML代码，压缩合并图片等
  - 代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载
  - 模块合并：在采用模块化的项目里会有很多歌模块和文件，需要构建功能吧模块分类合并成一个文件
  - 自动刷新：监听本地源代码的变化，自动重新构建、刷新浏览器
  - 代码校验：在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过
  - 自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统
- 在 webpack 中，所有的文件都是模块



##### 25.2 安装 webpack

###### 25.2.1 全局安装 webpack

- 安装

  `npm i webpack webpack-cli -g`

- 注意
  - 这里我们安装的是全局 webpack
  - 如果项目为合作项目，需要经常在不同的电脑上进行编辑，最好在该项目内进行 webpack 的本地安装和使用，以免版本不同造成不必要的麻烦



###### 25.2.2 本地安装 webpack

- 初始化 package.json

  `npm init -y`

- 安装

  `npm i webpack webpack-cli -D`

  -D 的意思是，只会在本地使用，不会在线上使用（打包完后，就不用它了）



##### 25.3 webpack 中间件

###### 25.3.1 webpack-dev-server 服务器

- 作用

  webpack 服务器

- 安装命令

  `npm i webpack-dev-server -D`

- 配置 package.json 快捷启动

- ```javascript
  "scripts": {
      "start": "webpack-dev-server"
    }
  ```

- 执行命令

  `npm run start`



###### 25.3.2 安装 html-webpack-plugin

- 作用

  使其支持 html 模板

- 安装命令

  `npm i html-webpack-plugin -D`

- 配置如：25.4 详细案例



###### 25.3.3 安装 clean-webpack-plugin

- 作用

  使代码在更新之前，删除先前的打包目标目录

- 安装命令

  `npm i clean-webpack-plugin -D`

- 配置如：25.4 详细案例



###### 25.3.4 CSS 处理

- 作用

  使 webpack 支持 css、less、sass 等处理

- 安装命令

  `npm i style-loader css-loader less less-loader node-sass sass-loader -D`

- 配置如：25.4 详细案例



###### 25.3.5 CSS 样式代码抽离

- 作用

  - 如果 CSS 代码全部以 style 样式镶嵌在 目标 js 文件里面，那整个文件就太大了，会严重影响加载速度
  - 我们要做的，就是将 CSS 样式从 目标js 文件里面抽离出来

- 安装命令

  `npm i extract-text-webpack-plugin@next mini-css-extract-plugin -D`

- 注意

  - extract-text-webpack-plugin 的 @next 版本是专门用来支持第4版 webpack 的
  - mini-css-extract-plugin 是用来代替 extract-text-webpack-plugin 的



###### 25.3.6 安装 CSS 代码精简器

- 作用

  - 比如一个 bootstrap，我们可能就用了个 btn，其它样式都没有用，但是却要加载几万行的代码，这样就造成了资源浪费
  - 我们要做的，就是把 CSS 代码给精简到只包含正在使用的代码

- 安装命令

  `npm i purifycss-webpack purify-css glob -D`

- 注意

  - 某些时候，有些使用了的 CSS 代码，依然会被删除掉，所以，这个插件请慎用
  - 比如说服务器即时动态发送过来的数据，代码精简器是扫描不到的，它会以为你没有使用该对应项的 CSS 代码



###### 25.3.7 给 CSS 代码加前缀

- 作用

  - 不同的浏览器，支持的 CSS 代码格式不一定相同，比如：-webkit-transform 与 transform
  - 我们这里，就需要对代码前缀进行动态删减

- 安装命令

  `npm i postcss-loader autoprefixer -D`

- 配置 postcss.config.js 到根目录

- ```javascript
  module.exports = {
      plugins:[
          require('autoprefixer')
      ]
  };
  ```



###### 25.3.8 拷贝插件

- 作用

  - 一般用于拷贝 静态文件

- 安装命令

  `npm i copy-webpack-plugin -D`

- 配置如：25.4 详细案例



##### 25.4 零配置

- 注：零配置只适用于 webpack 4.0 +
- 使用方法：
  - 被打包的 js 文件，默认放在 src 目录
  - 默认入口文件命名为 index.js
  - 打包命令：（以下两个命令是同样的效果）
    - `npx webpack`
    - `npm run build`（第要配置"build"："webpack"）
- 效果：
  - src 里面的代码，被默认打包到了根目录下的 dist 目录下面，生成 main.js



##### 25.5 详细案例

- src/index.html

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <!-- 注意，这里的htmlWebpackPlugin首字母一定为小写 -->
      <title><%=htmlWebpackPlugin.options.title%></title>
  </head>
  <body>
      <div id="app"></div>
  </body>
  </html>
  ```



- src/a.js

- ```javascript
  var msg = "hello world";
  module.exports = {
      msg:msg
  };
  ```



- src/index.js

- ```javascript
  let a = require('./a');
  document.getElementById('app').innerHTML =a.msg;
  import './index.css'
  import './style.scss'
  
  if (module.hot) {
      module.hot.accept()
  }
  ```



- src/index.css

- ```css
  body {
    background: red;
  }
  ```



- src/style.scss

- ```css
  body {
    background: yellow;
    color: blueviolet;
    transform:rotate(7deg);
  }
  
  #app {
    font-size: 30px;
    color: blueviolet;
  }
  
  .hello {
    font-size: 30px;
    color: blueviolet;
  }
  ```



- package.json

- ```javascript
  {
    "name": "webpacker",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "build": "webpack",
      "start": "webpack-dev-server"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
      "autoprefixer": "^9.4.3",
      "clean-webpack-plugin": "^1.0.0",
      "copy-webpack-plugin": "^4.6.0",
      "css-loader": "^2.1.0",
      "extract-text-webpack-plugin": "^4.0.0-beta.0",
      "glob": "^7.1.3",
      "html-webpack-plugin": "^3.2.0",
      "mini-css-extract-plugin": "^0.5.0",
      "node-sass": "^4.11.0",
      "postcss-loader": "^3.0.0",
      "purify-css": "^1.2.5",
      "purifycss-webpack": "^0.7.0",
      "sass-loader": "^7.1.0",
      "style-loader": "^0.23.1",
      "webpack": "^4.28.2",
      "webpack-cli": "^3.1.2",
      "webpack-dev-server": "^3.1.14"
    }
  }
  ```



- webpack.config.js

- ```javascript
  let path = require('path');
  let HtmlWebpackPlugin = require('html-webpack-plugin');
  let CleanWebpackPlugin = require('clean-webpack-plugin');
  let webpack = require('webpack');
  let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin');
  let ScssExtract = new ExtractTextWebpackPlugin('css/style.css');
  let CssExtract = new ExtractTextWebpackPlugin('css/index.css');
  let PurifycssWebpack = require('purifycss-webpack');
  let glob = require('glob');
  let CopyWebpackPlugin = require('copy-webpack-plugin');
  
  module.exports = {
      entry: './src/index.js',					// 入口
      output: {								    // 出口
          filename: 'main.[hash:8].js',		    // 给代码加上随机哈希串，用来清缓存，默认20位
          // 此处设置为8位
          path: path.resolve('./dist')			// 这里必须用绝对路径
      },
      devServer: {								// 开发 webpack 服务器
          contentBase: './dist',				    // 以 ./dist 这个目录，作为静态文件目录
          port: 3000,							    // 端口号默认为8080，我们设置为3000
          compress: true,						    // 服务器压缩
          open: true,							    // 打开服务后，自动打开浏览器
          hot: true
      },
      module: {								    // 模块配置
          rules: [
              {
                  test: /\.css$/, use: CssExtract.extract({
                      use: [
                          {
                              loader: 'css-loader'
                          }
                      ]
                  })
              },
              {
                  test: /\.scss$/, use: ScssExtract.extract({
                      use: [
                          {
                              loader: 'css-loader'
                          }, {
                              loader: 'sass-loader'
                          }, {
                              loader: 'postcss-loader'
                          }
                      ]
                  })
              }
          ]
      },
      plugins: [								// 插件的配置
          CssExtract,                  		// 配置 CSS 抽离插件
          ScssExtract,						// 配置 SCSS 抽离插件
          new CopyWebpackPlugin([             // 拷贝插件，一般用于拷贝静态文件
              {
                  from:'./src/doc',
                  to:'public'
              }
          ]),
          new webpack.HotModuleReplacementPlugin(),	// 热加载插件
          new CleanWebpackPlugin(['./dist']),	// 每次打包之前，先清空 dist 目录
          new HtmlWebpackPlugin({
              template: './src/index.html',	// 打包后的html文件会在末尾自动添加
              								// 正确的JavaScript引用
              title: '首页',
              hash: true,						// 给代码加上随机哈希串，用来清缓存
              minify: {						// 压缩代码
                  removeAttributeQuotes: true,	// 删除属性双引号
                  collapseWhitespace: true		// 删除所有空白，将代码压缩成一行
              }
          }),
          new PurifycssWebpack({                  // 未使用的 CSS 会被消除掉，这个插件一定要在
              									//上一个插件之后
              paths:glob.sync(path.resolve('src/*.html'))
          })
      ],
      mode: 'development',	// 可以更改模式(开发者模式不会压缩代码，更利于学习)
      resolve: {}			// 配置解析
  };
  ```

- postcss.config.js

- ```javascript
  module.exports = {
      plugins:[
          require('autoprefixer')
      ]
  };
  ```



  - entry的写法

    - 字符串：`entry:'./src/index.js'`

      - 引入单个入口文件，合并为一个 js 文件

    - 数组：`entry:['./src/index.js','./src/hello.js']`

      - 引入多个入口文件，合并为一个 js 文件

    - 对象：多入口配多出口

    - ```javascript
      entry:{
          index:'./src/index.js',
          hello:'./src/hello.js' 
      },
      output:{
          filename:'[name].[hash:8].js',
          path:path.resolve('./dist')
      },
      plugins:[
          new CleanWebpackPlugin(['./dist']),	
          new HtmlWebpackPlugin({
              filename: "a.html",
              template:'./src/index.html',
              title:'首页',
              hash:true,						
              chunks:['index'],
              minify:{						
              removeAttributeQuotes:true,	
              collapseWhitespace:true		
          }
      }),
          new HtmlWebpackPlugin({	
              filename: "b.html",
              template:'./src/b.html',
      
              title:'B页面',
              hash:true,						
              chunks:['hello'],
              minify:{					
              removeAttributeQuotes:true,
              collapseWhitespace:true	
          	}
      	})
      ]
      ```

