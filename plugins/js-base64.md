# js-base64 —— Base64 的 JavaScript 实现
## 用法
安装：

```bash
$ npm install --save js-base64
```

使用：

```js
import { Base64 } from 'js-base64';
Base64.encode('dankogai'); // ZGFua29nYWk=
Base64.decode('ZGFua29nYWk=');  // dankogai
```

## 关于ie
从版本3开始，不再支持IE，可以尝试用poly-fill转一下，或者安装版本2：

卸载：
```bash
npm uninstall js-base64
```

安装2.x.x版本：
```js
npm install js-base64@2.6.2 --save
```
