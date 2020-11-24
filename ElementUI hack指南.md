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
