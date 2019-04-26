# 檔案系統
## 讀取TestFile.txt檔案
```
var fs = require('fs');
 
fs.readFile('TestFile.txt', function (err, data) {
    if (err) throw err;
 
    console.log(data.toString());
});
```

## 同步讀取檔案
```
var fs = require('fs'); 
var data = fs.readFileSync('dummyfile.txt', 'utf8');
console.log(data);
```
* 同步讀取會讓整個系統等待檔案讀完才有回應

## 寫入檔案
```
var fs = require('fs');
 
fs.writeFile('test.txt', '您好嗎?', function (err) {
    if (err)
        console.log(err);
    else
        console.log('Write operation complete.');
});

```

## 新增內容至檔案
```
var fs = require('fs');
 
fs.appendFile('test.txt', '我很好！', function (err) {
    if (err)
        console.log(err);
    else
        console.log('Append operation complete.');
});
```

## 開啟讀取檔案
```
var fs = require('fs');
 
fs.open('TestFile.txt', 'r', function (err, fd) {
 
    if (err) {
        return console.error(err);
    }
 
    var buffr = new Buffer(1024);
 
    fs.read(fd, buffr, 0, buffr.length, 0, function (err, bytes) {
 
        if (err) throw err;
 
        // Print only read bytes to avoid junk.
        if (bytes > 0) {
            console.log(bytes+" 字元被讀取");
            console.log(buffr.slice(0, bytes).toString());
        }
 
        // Close the opened file.
        fs.close(fd, function (err) {
            if (err) throw err;
        });
    });
});
```

## 刪除文件
```
var fs = require('fs');
 
fs.unlink('test.txt', function () {
    console.log('已經刪除檔案!');
});
```
