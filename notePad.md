## HTML
### svg标签加title属性提示不生效
直接在标签加属性是不生效的，需要加 `<title>xxx</title>`：
```html
<svg class="icon" aria-hidden="true">
  <title>移动</title>
  <use xlink:href="#icon-move"></use>
</svg>
```
也可以通过外层包一层span或div来加title属性，如果svg同时存在标签内和标签外title，显示标签内也就是 `<title>xxx</title>`。

## CSS

## 工具方法
### JS将unicode码转中文方法
将unicode的 `\u` 先转为 `%u`，然后使用 **unescape** 方法转换为中文:

```js
var str = "\u7434\u5fc3\u5251\u9b44\u4eca\u4f55\u5728\uff0c\u6c38\u591c\u521d\u6657\u51dd\u78a7\u5929\u3002";
document.write(unescape(str.replace(/\\u/g, '%u')));
```
