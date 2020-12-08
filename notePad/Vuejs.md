### 1、style属性绑定background-image
在css外设置background-image时，不能直接使用url，应该使用require：
```js
arr.forEach((item, index) => {
  item.bgImg = require(`../../assets/images/kmap/orgchart${index%6+1}.png`);
})
```

```html
<div :style="{backgroundImage: 'url(' + bgImg + ')'}"></div>
```
