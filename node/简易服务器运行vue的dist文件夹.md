Vue项目开发的时候打包生成打包文件dist，但是因为某些原因需要线下运行该文件。此时可以通过Node构建一个简易服务，使打包文件跑起来。

**app.js：**

```js
var express = require('express');
var fs = require('fs');
var http = require('http');
var app = express();
app.use(express.static('./dist'));
app.use(function (req, res,next) {
  res.sendfile('./dist/index.html');  //路径根据自己文件配置
});
var httpsServer = http.createServer(app);
httpsServer.listen(8081, function () {
  var port = httpsServer.address().port;
  console.log('app listening at http://localhost:%s', port);
});
```

运行：
```bash
node app.js
```
