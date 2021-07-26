借助 babel-plugin-component，我们可以只引入需要的组件，以达到减小项目体积的目的。

### 1. 安装babel-plugin-component

```shell
npm install babel-plugin-component -D
```

### 2. 修改babel.config.js

新增
```js
"plugins": [
  [
    "component",
    {
      "libraryName": "element-ui",
      "styleLibraryName": "theme-chalk"
    }
  ]
]
```

### 3. 按需引入

```js
import { Button, Select } from 'element-ui';

Vue.use(Button)
Vue.use(Select)
```
