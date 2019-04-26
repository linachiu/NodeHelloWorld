# GET & POST

## 前端（canvas.html）
```
   $.ajaxSetup({
        contentType: "application/x-www-form-urlencoded; charset=utf-8"
    });

    $.post("/GetMyPic", { name: "linalina"},
         function(data){
                console.log(data.dataInfo);
                $("#dOutput").html(
                    $("<img />", { src: data.dataInfo,
                                "class": "output"
                    }));
          }, "json");
```

## 後端（app.js）
* 為了解析body 需要引用
```
var bodyParser = require('body-parser');
```
```
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({
    extended:true
}));
```

* 接收
```
app.post('/GetMyPic', function(req, res) {
 	//name
 	console.log("req="+req.body);
 	console.log("req="+req.body.name);
	readPic(res,req.body.name+".txt");
 
});
```
## 參考資料
[四種請求方式](https://www.jianshu.com/p/606802e40fd5)
[詳解](https://i5ting.github.io/node-http/#10801)
