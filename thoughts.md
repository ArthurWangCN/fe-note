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

