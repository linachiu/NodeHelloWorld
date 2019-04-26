# 檔案上傳
## 前端代碼
 ```
        <div class="row cl">
            <label class="form-label col-xs-4 col-sm-2">缩略图：</label>
            <div class="formControls col-xs-8 col-sm-9">
                <div class="uploader-thum-container">
                    <input type="file" name="pic" id="pic">
                </div>
            </div>
        </div>
```

## ajax代碼


```
<script type="text/javascript">
    $(function(){
    
    var ue = UE.getEditor("ueditor");
    
    $("#button1").click(function(){
        var title = $("#title").val();
        var author = $("#author").val();
        var source = $("#source").val();
        var cateid = $("#cateid").val();
        var zhaiyao = $("#textarea").val();
        var timer = $("#datemin").val();
        var pic = $("#pic")[0].files[0];
        var content = ue.getContent();//获取文本编辑器的内容。

        var formData = new FormData();
        formData.append("pic",pic);
        formData.append("title",title);
        formData.append("author",author);
        formData.append("source",source);
        formData.append("cateid",cateid);
        formData.append("zhaiyao",zhaiyao);
        formData.append("timer",timer);
        formData.append("content",content);


        $.ajax({
            url:"/admin/upload",
            type:"post",
            data:formData,
            contentType:false,  
            processData:false, 
            success:function(data){
                console.log(data.ok);
                if(data.ok == 200){
                    alert("添加成功！");
                    window.location.href = "http://127.0.0.1:3000/admin/article-list";
                }else{
                    alert("添加失败！");
                }
            }
        });
    });

    })
</script>
```


## node.js 代碼


```
router.post('/upload',function(req,res){

    var d = req.body;
    console.log(d);
    var f = req.files;
    var ext = path.extname(f.pic[0].originalname);
    var filename = f.pic[0].path;
    var picname = f.pic[0].filename + ext;
    fs.rename(filename,filename+ext);

    query('insert into antcontent(title,author,source,timer,content,pic,zhaiyao,views,cateid) values(?,?,?,?,?,?,?,?,?)',[d.title,d.author,d.source,d.timer,d.content,picname,d.zhaiyao,0,d.cateid],function(err,data){
        if(!err){
            res.json({ok:200});
        }
    });

});
```


## app.js 配置上傳項


```
var multer = require('multer');

var upload = multer({dest:'./public/uploads'}).fields([{name:'files',maxCount:1},{name:'pic',maxCount:3}]);

app.use("/ueditor/ue",ueditor(path.join(__dirname,'public'),function(req,res,next){

  // ueditor 客戶發起上傳圖片請求
  if(req.query.action === 'uploadimage'){

    // 這裏你可以獲得上傳圖片的信息
    var foo = req.ueditor;
    console.log(foo.filename); // exp.png
    console.log(foo.encoding); // 7bit
    console.log(foo.mimetype); // image/png

    // 下面填寫你要把圖片保存到的路徑 （ 以 path.join(__dirname, 'public') 作為根路徑）
    var img_url = '/yourpath';
    res.ue_up(img_url); //你只要輸入要保存的地址 。保存操作交給ueditor來做
  }
  //  客戶端發起圖片列表請求
  else if (req.query.action === 'listimage'){
    var dir_url = 'your img_dir'; // 要展示給客戶端的
    文件夾路徑
    res.ue_list(dir_url) // 客戶端會列出 dir_url 目錄下的所有圖片
  }
  // 客戶端發起其它請求
  else {
    res.setHeader('Content-Type', 'application/json');
    // 這裏填寫 ueditor.config.json 這個文件的路徑
    res.redirect('/ueditor/ueditor.config.json');
  }

}));
app.use(upload);
```
--------------------- 
作者：summer_你 
来源：CSDN 
原文：https://blog.csdn.net/sunjuansos/article/details/78687398 


===================================================================
[成功案例](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/252843/)

