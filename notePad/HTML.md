### 1、svg标签加title属性提示不生效
直接在标签加属性是不生效的，需要加 `<title>xxx</title>`：
```html
<svg class="icon" aria-hidden="true">
  <title>移动</title>
  <use xlink:href="#icon-move"></use>
</svg>
```
也可以通过外层包一层span或div来加title属性，如果svg同时存在标签内和标签外title，显示标签内也就是 `<title>xxx</title>`。
