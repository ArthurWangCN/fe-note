### 1、ie下隐藏滚动条IE10,IE11,IE12

```css
.scroll_content{
    -ms-scroll-chaining: chained;
    -ms-overflow-style: none;
    -ms-content-zooming: zoom;
    -ms-scroll-rails: none;
    -ms-content-zoom-limit-min: 100%;
    -ms-content-zoom-limit-max: 500%;
    -ms-scroll-snap-type: proximity;
    -ms-scroll-snap-points-x: snapList(100%, 200%, 300%, 400%, 500%);
    -ms-overflow-style: none;
    overflow: auto;
}
```
只是隐藏了滚动条，页面仍然可以滚动。

### 2、style标签媒介查询
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
没想到 `style` 标签也可以加媒介查询的条件。
