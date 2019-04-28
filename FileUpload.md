# 檔案上傳
## 前端代碼
 ```
 <form action="/fileUpload" method="post" enctype="multipart/form-data">
			<input name="fileUpload_img" type="file" />
			<img src="" alt="">
			<button type="submit">上傳</button>
		</form>
```


## node.js 代碼


```
app.post('/fileUpload', upload.single('fileUpload_img'), function (req, res, next) {
    const { file } = req;
        fs.readFile(file.path, function(err, data) {
           var tmpPath=`${UPLOAD_PATH}/${file.originalname}`;
            fs.writeFile(tmpPath, data, function (err) {
            if (err) res.json({ err })
              // fs.unlink(tmpPath, function() {
                   
              //      console.log('File Uploaded to ');
              //  });
            res.json({
            msg: '上傳成功'
            });
        });
    })
})

```


## app.js 配置上傳項


```
const multer = require('multer');
const fs = require('fs');
const UPLOAD_PATH = './uploads'
var upload = multer({ dest: UPLOAD_PATH });

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({
    extended:true
}));

```



===================================================================
[成功案例](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/252843/)

