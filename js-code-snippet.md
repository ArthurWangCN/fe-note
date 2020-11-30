### 1、findAndReplace
在字符串中查找给定的单词，并替换为另一个单词

```js
const findAndReplace = (string, wordToFind, wordToReplace) => {
    string.split(wordToFind).join(wordToReplace);
}
let result = findAndReplace('banana I like banana', 'banana', 'apple');// "apple I like apple"
```

### 2、RGBToHex
将RGB模式下的颜色转换为十六进制

```js
const RGBToHex = (r, g, b) => ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0');
let hex = RGBToHex(255, 255, 255);  // ffffff
```

### 3、deepFind
深度查找匹配的对象

```js
var result;
function deepFind(arr, targetId) {
  var result;
  for (var i = 0; i < arr.length; i++) {
    if (arr[i].id === targetId) {
      result = arr[i];
    } else {
      if (arr[i].children && arr[i].children.length > 0) {
        deepFind(arr[i].children);
      }
    }
  }
}
```
