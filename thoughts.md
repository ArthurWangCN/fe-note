### 1、style标签媒介查询
偶然在视频里看到一种媒介查询的方式：
```html
<style media="(min-width: 300px) and (max-width: 500px)">
    #box {
      color: aqua;
    }
  </style>
  <style media="(min-width: 200px) and (max-width: 300px)">
    #box {
      color: red;
    }
  </style>
```
没想到 `style` 标签也可以加媒介查询的条件，涨姿势了～


### 2、遮罩遮不住滚动条的问题
有这样一个需求：需要手动写一个弹出层，用到了fixed布局，但是遮罩遮不住页面的滚动条。这时候滚动条很容易造成误解，不清楚是弹出层内部内容的滚动条还是页面的滚动条，所以就需要处理滚动条。

看了很多，但是觉得如果是全屏的滚动条，最好的方案就是在弹出层显示的时候把 body 的 overflow 设为 hidden 把滚动条先隐藏，在关闭弹出层的时候再重新显示。

当然，如果业务逻辑允许，滚动条加在页面内的一个元素上，而不是 body 上。
