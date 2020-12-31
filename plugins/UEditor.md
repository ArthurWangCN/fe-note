### 1、监听事件
1. 监听鼠标聚焦事件：
  ```js
  UE.getEditor('editor').addListener('focus',function(editor){
    // do something ...
  });
  ```
2. 监听内容改变事件
  ```js
  UE.getEditor('editor').addListener('contentChange',function(editor){
    // do something ...
  });
  ```
注：其中 editor 代表你的富文本编辑器的id值
