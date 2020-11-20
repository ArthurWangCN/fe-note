## Element UI
### 修改el-pagination中的默认'前往'两字
```js
document.getElementsByClassName('el-pagination__jump')[0].childNodes[0].nodeValue = '跳至'
```

## 工具方法
### JS将unicode码转中文方法
将unicode的 `\u` 先转为 `%u`，然后使用 **unescape** 方法转换为中文:

```js
var str = "\u7434\u5fc3\u5251\u9b44\u4eca\u4f55\u5728\uff0c\u6c38\u591c\u521d\u6657\u51dd\u78a7\u5929\u3002";
document.write(unescape(str.replace(/\\u/g, '%u')));
```
