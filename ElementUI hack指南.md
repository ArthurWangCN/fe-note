# ElementUI hack
## el-scrollbar
### el-scrollbar 设置高度以显示滚动条
可以通过设置高度来显示滚动条，但是更好的实现是设置一个max-height：
```css
.el-scrollbar .el-scrollbar__wrap {
  max-height: 300px;
  overflow-x: hidden; // 隐藏横向滚动轴
}
```
