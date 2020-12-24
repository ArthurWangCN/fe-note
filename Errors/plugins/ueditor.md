## ueditor.all.js
### 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode...
> Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them

原因是因为webpack打包后出现的，是因为webpack使用的是严格模式打包。有人用一些外挂插件来解决这个问题，但是我会尽量不使用插件，采用的是修改webpack打包配置，略过某个文件，又不影响其他文件的严格模式。

> callee 是 arguments 对象的一个属性。它可以用于引用该函数的函数体内当前正在执行的函数。这在函数的名称是未知时很有用，例如在没有名称的函数表达式 (也称为“匿名函数”)内。

> 警告：在严格模式下，第5版 ECMAScript (ES5) 禁止使用 arguments.callee()。当一个函数必须调用自身的时候, 避免使用 arguments.callee(), 通过要么给函数表达式一个名字,要么使用一个函数声明.

需要修改两个地方:

```js
editor.addListener('ready', function() {
// ==> 改为 editor.addListener('ready', function ED() {
    //提供getDialog方法
    editor.getDialog = function(name) {
        return editor.ui._dialogs[name + "Dialog"];
    };
    ...
    if (editor.options.wordCount) {
      function countFn() {
          setCount(editor, me);
          domUtils.un(editor.document, "click", arguments.callee);
          // ==> 改为 domUtils.un(editor.document, "click", ED);
      }
      domUtils.on(editor.document, "click", countFn);
      editor.ui.getDom('wordcount').innerHTML = editor.getLang("wordCountTip");
    }
    ...
```
