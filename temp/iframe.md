## iframe一些方法

### 获取iframe内容

```js
var iframe = document.getElementById("iframe1");
var iwindow = iframe.contentWindow;
var idoc = iwindow.document;
console.log("window",iwindow);//获取iframe的window对象
console.log("document",idoc);  //获取iframe的document
console.log("html",idoc.documentElement);//获取iframe的html
console.log("head",idoc.head);  //获取head
console.log("body",idoc.body);  //获取body
```



### 自适应iframe

设置宽高：

```js
var iwindow = iframe.contentWindow;
var idoc = iwindow.document;
iframe.height = idoc.body.offsetHeight;
```

去滚动条：

```html
<iframe id="google_ads_frame2" name="google_ads_frame2" width="160" height="600" frameborder="0" src="target.html" marginwidth="0" marginheight="0" vspace="0" hspace="0" allowtransparency="true" scrolling="no" allowfullscreen="true"></iframe>
```





## elementUI中tab切换

tab切换时每个标签页下的iframe重新加载:

1. 首先给t每个tabs标签页下的iframe绑定ref，ref的值与el-tab-pane的name属性的值一致
2. 通过tab-click方法取到当前选中标签页的iframe，然后执行刷新

```vue
<el-tabs v-model="activeName" @tab-click="handleClick" v-else>
    <el-tab-pane label="xxx" name="1">
        <iframe ref='1' width="100%" height="480px" :src=orderSurveyUrl></iframe ref='1'>
    </el-tab-pane>
    <el-tab-pane label="xxx" name="2">
        <iframe ref='2' width="100%" height="480px" :src=standardDeliverUrl></iframe>
    </el-tab-pane>
    <el-tab-pane label="xxx" name="3">
        <iframe ref='3' width="100%" height="480px" :src=hispb></iframe>
    </el-tab-pane>
    <el-tab-pane label="xxx" name="4">
        <iframe ref='4' width="100%" height="480px" :src=gatheringUrl></iframe>
    </el-tab-pane>
</el-tabs>
```

```js
handleClick(tab, event) {
    console.log(tab.name);
    console.log(this.$refs[tab.name])
    this.$refs[tab.name].contentWindow.location.reload()
}
```

