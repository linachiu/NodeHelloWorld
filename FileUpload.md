# 檔案上傳
* 注意:  upload.single('fileUpload_img')  && <input name="fileUpload_img" type="file" />  NAME 要相同
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


## Angular + Node.js 多個檔案上傳

* Angular  Html
```
<ngx-dropzone (change)="onSelectFile($event)">
    <ngx-dropzone-label>Drop it, baby!</ngx-dropzone-label>
    <ngx-dropzone-preview *ngFor="let f of files" [removable]="true" (removed)="onRemoveFile(f)">
        <ngx-dropzone-label>{{ f.name }} ({{ f.type }})</ngx-dropzone-label>
    </ngx-dropzone-preview>
</ngx-dropzone>
```

* Angular  Component.js
```
  onSelectFile(event) {
    console.log(event);
    this.files.push(...event.addedFiles);

    const formData = new FormData();

    for (var i = 0; i < this.files.length; i++) { 
      // formData.append("file[]", this.files[i]);
      formData.append("fileList", this.files[i]);
    }

    // formData.append("file", this.files[0]);

    console.log(this.files)

    this.http.post('http://localhost:3002/fileArrayUpload', formData)
    .subscribe(res => {
       console.log(res);
       alert('Uploaded Successfully.');
    })
  }
   onRemoveFile(event) {
      console.log(event);
      this.files.splice(this.files.indexOf(event), 1);
  }
  ```
* Node.js
```
app.post('/fileArrayUpload', upload.array('fileList', 12), function (req: any, res, next) {
      const { files } = req;
        files.forEach((file: any) => {
        fs.readFile(file.path, function(err: any, data: any) {
          const tmpPath=`${UPLOAD_PATH}/${file.originalname}`;
          fs.writeFile(tmpPath, data, function (err: any) {
          if (err) res.json({ err })
          res.json({
            msg: '上傳成功'
          });
      });
    });
  })
})
```
===================================================================
[成功案例](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/252843/)

