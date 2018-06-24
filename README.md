# NodeHelloWorld

## 安裝Node
如果是 mac 或 windows 系統可以直接在網路上找載點做安裝
```
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
yum -y install nodejs
```
* 查看版本
```
node -v
```

## 開啟新專案

* 創一個你喜歡的資料夾名稱
* 進到資料夾內
```
cd /data/release/hello
```
* 輸入下面指令來初始化專案 
```
npm init
```
* 接下來會要求輸入幾個參數，大部分可以enter略過， 但 entry point: 是指你的進入檔案名稱，需輸入檔名，如果使用預設名為 index.js。

## 安裝 Express
這邊使用 npm 指令安裝
``
npm install express
``

## Hello world

* 創建 app.js （因為我的進入檔取名為app.js），如果是用預設則取名為index.js
* 接下來將以下代碼 copy 貼進檔案中

```
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```
* 上面的code 啟動一個服務並監聽從3000端口進入的所有連接請求。他將對所有（/）URL或路由返回“Hello World！”字符串。對其他所有路徑全部返回404 Not Found。
* req (請求) 和 res (回應) 與 Node 提供的對象完全一致，因此，你可以調用 req.pipe()、req.on('data', callback) 以及任何 Node 提供的方法。

## 啟動專案
```
node app.js
```
然後在瀏覽器中打開 http://<您的 CVM IP 地址>:3000 並查看輸出結果。
p.s 要確認主機有開啟 3000 port

Express 路由简介
路由（Routing）是由一个 URI（或者叫路径）和一个特定的 HTTP 方法（GET、POST 等）组成的，涉及到应用如何响应客户端对某个网站节点的访问。

每一个路由都可以有一个或者多个处理器函数，当匹配到路由时，这些函数将被执行。

路由的定义由如下结构组成：

app.METHOD(PATH, HANDLER)
其中：

app 是一个 express 实例；
METHOD 是某个 HTTP 请求方式 中的一个
PATH 是服务器端的路径；
HANDLER 是当路由匹配到时需要执行的函数。
一个简单的 Express 路由
修改 hello 项目
返回开始创建的 hello 项目：

cd /data/release/hello
编辑 app.js，参考修改如下：

示例代码：/data/release/hello/app.js
var express = require('express');
var app = express();

// 对网站首页的访问返回 "Hello World!" 字样
app.get('/', function (req, res) {
  res.send('Hello World!');
});

// 网站首页接受 POST 请求
app.post('/', function (req, res) {
  res.send('Got a POST request');
});

// /user 节点接受 PUT 请求
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user');
});

// /user 节点接受 DELETE 请求
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user');
});

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
启动应用
node app.js
（该步骤完成后，可使用 Ctrl + C 终止运行。）


创建静态目录
创建 public 目录：

mkdir -p /data/release/hello/public
在 public 目录下，创建 hello.html，然后复制下列代码到 hello.html 中：

示例代码：/data/release/hello/public/hello.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <h1>Hello World</h1>
</body>
</html>
修改应用
编辑 app.js，参考修改如下：

示例代码：/data/release/hello/app.js
var express = require('express');
var app = express();

app.use(express.static('public'));

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
我们在 app.js 中将静态资源文件所在的目录作为参数传递给 express.static 中间件，这样就可以提供静态资源文件的访问了。

启动应用
node app.js
在浏览器中打开 http://<您的 CVM IP 地址>:3000/hello.html 网址就可以看到这个文件了。



你还可以将本地的文件通过拖拽至左边目录树的 public 目录上传文件来测试。

假设在 public 目录放置了图片、CSS 和 JavaScript 文件，你就可以从浏览器中访问：

http://<您的 CVM IP 地址>:3000/images/kitten.jpg
http://<您的 CVM IP 地址>:3000/css/style.css
http://<您的 CVM IP 地址>:3000/js/app.js
http://<您的 CVM IP 地址>:3000/images/bg.png
http://<您的 CVM IP 地址>:3000/hello.html
static 中间件更多用法
多个目录
如果你的静态资源存放在多个目录下面，你可以多次调用 express.static 中间件：

app.use(express.static('public'));
app.use(express.static('files'));
访问静态资源文件时，express.static 中间件会根据目录添加的顺序查找所需的文件。

指定路径
如果你希望所有通过 express.static 访问的文件都存放在一个“虚拟（virtual）”目录（即目录根本不存在）下面，可以通过为静态资源目录指定一个挂载路径的方式来实现，如下所示：

编辑 app.js，参考修改如下：

示例代码：/data/release/hello/app.js
var express = require('express');
var app = express();

app.use('/static', express.static('public'));

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
启动应用：

node app.js
现在，你就可以通过带有 “/static” 前缀的地址来访问 public 目录下面的文件了。如：

http://<您的 CVM IP 地址>:3000/static/hello.html
