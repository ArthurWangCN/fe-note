### 1、遮罩遮不住滚动条的问题
有这样一个需求：需要手动写一个弹出层，用到了fixed布局，但是遮罩遮不住页面的滚动条。这时候滚动条很容易造成误解，不清楚是弹出层内部内容的滚动条还是页面的滚动条，所以就需要处理滚动条。

看了很多，但是觉得如果是全屏的滚动条，最好的方案就是在弹出层显示的时候把 body 的 overflow 设为 hidden 把滚动条先隐藏，在关闭弹出层的时候再重新显示。

当然，如果业务逻辑允许，滚动条加在页面内的一个元素上，而不是 body 上。


### 2、输入框点击指定元素不失去焦点
例如点击按钮或者一个区域，不想让输入框失去焦点，可以考虑通过阻止默认事件来实现：

```js
//点击关闭按钮 input框不触发失去焦点事件
document.getElementById('js_close').onmousedown=function(e){
  if ( e && e.preventDefault ){
      e.preventDefault();
  }else{
      window.event.returnValue = false;
  }
  return false;

};
```

但是在Vue项目中遇到了el-date-picker在阻止默认事件后选时间组件弹不出来，所以考虑通过一个变量flag来判断是否触发blur事件之后的操作，但是似乎blur事件优先级高，所以用setTimeout时间设为0来实现。
