
# NodeHelloWorld

## 安裝Node
如果是 mac 或 windows 系統可以直接在網路上找載點做安裝
```
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -yum -y install nodejs
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

## Express 路由
路由（Routing）是由一個 URI（或者叫路徑）和一個特定的 HTTP 方法（GET、POST 等）組成的，涉及到應用如何響應客戶端對某個網站節點的訪問。

每一個路由都可以有一個或者多個處理器函數，當匹配到路由時，這些函數將被執行。

路由的定義由如下結構組成：

app.METHOD(PATH, HANDLER)

app 是一個 express 實例；
METHOD 是某個 HTTP 請求方式 中的一個
PATH 是服務器端的路徑；
HANDLER 是當路由匹配到時需要執行的函數。
一個簡單的 Express 路由
修改 hello 項目
返回開始創建​​的 hello 項目：

cd /data/release/hello
編輯 app.js，參考修改如下：

示例代碼：/data/release/hello/app.js
```
var express = require('express');
var app = express();

// 對網站首頁的訪問返回 "Hello World!" 字樣
app.get('/', function (req, res) {
  res.send('Hello World!');
});

// 網站首頁接受 POST 請求
app.post('/', function (req, res) {
  res.send('Got a POST request');
});

// /user 節點接受 PUT 請求
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user');
});

// /user 節點接受 DELETE 請求
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user');
});

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```


## 創建靜態目錄
* 在專案下創建 public 目錄：
* 在 public 目錄下，創建 hello.html，然後復制下列代碼到 hello.html 中：

示例代碼：/data/release/hello/public/hello.html
```
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
```

編輯 app.js，參考修改如下：
示例代碼：/data/release/hello/app.js
```
var express = require('express');
var app = express();

app.use(express.static('public'));

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```
我們在 app.js 中將靜態資源文件所在的目錄作為參數傳遞給 express.static 中間件，這樣就可以提供靜態資源文件的訪問了。

* 在瀏覽器中打開 http://<您的 CVM IP 地址>:3000/hello.html 網址就可以看到這個文件了。

你還可以將本地的文件通過拖拽至左邊目錄樹的 public 目錄上傳文件來測試。

* 假設在 public 目錄放置了圖片、CSS 和 JavaScript 文件，你就可以從瀏覽器中訪問：
```
http://<您的 CVM IP 地址>:3000/images/kitten.jpg
http://<您的 CVM IP 地址>:3000/css/style.css
http://<您的 CVM IP 地址>:3000/js/app.js
http://<您的 CVM IP 地址>:3000/images/bg.png
http://<您的 CVM IP 地址>:3000/hello.html
```
## static 中間件更多用法
多個目錄
如果你的靜態資源存放在多個目錄下面，你可以多次調用 express.static 中間件：
```
app.use(express.static('public'));
app.use(express.static('files'));
```
訪問靜態資源文件時，express.static 中間件會根據目錄添加的順序查找所需的文件。

## 指定路徑
如果你希望所有通過express.static 訪問的文件都存放在一個“虛擬（virtual）”目錄（即目錄根本不存在）下面，可以通過為靜態資源目錄指定一個掛載路徑的方式來實現，如下所示：

編輯 app.js，參考修改如下：
```
示例代碼：/data/release/hello/app.js
var express = require('express');
var app = express();

app.use('/static', express.static('public'));

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

現在，你就可以通過帶有 “/static” 前綴的地址來訪問 public 目錄下面的文件了。如：

http://<您的 CVM IP 地址>:3000/static/hello.html
