# ElementUI hack
## 滚动条组件el-scrollbar
### el-scrollbar 设置高度以显示滚动条
可以通过设置高度来显示滚动条，但是更好的实现是设置一个max-height：
```css
.el-scrollbar .el-scrollbar__wrap {
  max-height: 300px;
  overflow-x: hidden; // 隐藏横向滚动轴
}
```


## 弹窗组件el-dialog
### el-dialog在IE11中打开关闭闪一下
```css
.el-dialog__wrapper{
  &.dialog-fade-enter-active{
    -ms-animation:none;
  }
  &.dialog-fade-leave-active{
    -ms-animation:none;
  }
}
```


## 分页组件el-pagination
### 修改el-pagination中的默认'前往'两字
```js
document.getElementsByClassName('el-pagination__jump')[0].childNodes[0].nodeValue = '跳至';
```


## 树形el-tree
### 修改el-tree展开收起icon
只需改相应的content，值得注意的是，用得是阿里的iconfont，所以 `font-family` 需要设为 `iconfont`：
```css
.el-tree-node__expand-icon.expanded {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);  /* 点击展开不旋转 */
}
/* 收起 */
.el-icon-caret-right:before {
    font-family: "iconfont";
    content: "\e78b";
}
/* 展开 */
.el-tree-node__expand-icon.expanded.el-icon-caret-right:before{
    font-family: "iconfont";
    content: "\e78a";
}
```


## 表格el-table
### 只给表格内容加loading(不含表头)
开始loading：
```js
const loading = this.$loading({
   lock: true,  // 同v-loading的修饰符
   text: this.$t('tip.loading'),  // 加载文案
   backgroundColor: 'rgba(55,55,55,0.4)', // 背景色
   spinner: 'el-icon-loading',  // 加载图标
   target: document.querySelector(".el-table__body")  // loading需要覆盖的DOM节点，默认为body
})
```
注意最好放到 `this.$nextTick` 回调中，否则可能获取不到元素。

关闭loading：
```js
loading.close();  // 加载完成时调用，关闭loading效果
```

### 加载过程中，不显示“暂无数据”
```html
<el-table>
  <template slot="empty">
    <p>{{ noDataText }}</p>
  </template>
  // ...
</el-table>
```
data中定义 `noDataText` 为空，加载数据成功，再根据有无数据进行赋值。


