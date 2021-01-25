### 1、编辑器内容插入页面后样式不生效
以居中为例，生成的dom是 `<p class="ql-align-center">align center text</p>`，插件给内容的dom外层封装了一个类名为 `ql-editor` 的div，所以页面中也需要加上并且引入quill.core.css：

```html
<div class="detail-content ql-editor" v-html="notice.content"></div>
```

```js
import 'quill/dist/quill.core.css'
```
