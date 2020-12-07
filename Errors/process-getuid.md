## 问题描述
今天项目打包的时候突然报了一个错：

```bash
TypeError: process.getuid is not a function
```
很奇怪，上周五还可以正常打包，于是发现了这个奇葩的问题。

看到有一个地方说把计算机本地时间改成周二试试，改了之后，发现打包正常，于是发现是和周一有关。



## 解决方案
1. 将日期更改为星期一以外的任何一天:
```js
const now = new Date();
if (now.getDay() === MONDAY) {
    const { access, constants, statSync, utimesSync } = require("fs");
    const lastPrint = statSync(openCollectivePath).atime;
    const lastPrintTS = new Date(lastPrint).getTime();
    const timeSinceLastPrint = now.getTime() - lastPrintTS;
    if (timeSinceLastPrint > SIX_DAYS) {
        require(openCollectivePath);
        // On windows we need to manually update the atime
        access(openCollectivePath, constants.W_OK, e => {
            if (!e) utimesSync(openCollectivePath, now, now);
        });
    }
}
```


2.删除该条件 `fileOwnerId === process.getuid())`，但对Windows用户不起作用。

3.将 package.json 中 "webpack-cli" 升级到 "^3.3.5"。


## 参考
+ https://www.samyoc.com/single/3961
+ https://github.com/webpack/webpack-cli/issues/962

