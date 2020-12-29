### 1、style属性绑定background-image
在css外设置background-image时，不能直接使用url，应该使用require：
```js
arr.forEach((item, index) => {
  item.bgImg = require(`../../assets/images/kmap/orgchart${index%6+1}.png`);
})
```

```html
<div :style="{backgroundImage: 'url(' + bgImg + ')'}"></div>
```

### 2、如何把el-dialog封装到一个单独的组件里
业务上有时候需要把 el-dialog 放到一个单独的 .vue 文件里，而控制弹窗显示隐藏的变量需要通过父子组件通信来改变：

父组件：
```js
<el-button type="primary" @click="modifyVisible=true;">show dialog</el-button>
<child :visible="modifyVisible" @updateVisible="updateVisible"></child>

data () {
  return {
    modifyVisible: false
  }
},
methods: {
  updateVisible(val) {
    this.modifyVisible = val;
  }
}
```

子组件： 
```js
<el-dialog title="外层 Dialog" :visible.sync="modifyVisible"></el-dialog>

props: {
  visible: {
    type: Boolean,
    default: false
  }
},
computed: {
  // 通过计算属性设置getter和setter来通知父组件
  modifyVisible: {
    get() {
      return this.visible;
    },
    set(val) {
      this.$emit('updateVisible', val);
    }
  }
},
```

### 3、路由发生变化的时候修改页面title
结合路由的 meta 和 导航守卫来实现。

router.js：
```js
{
  path: '/',
  name: 'home',
  component: Home,
  meta: {
    title: "首页"
  }
},
{
  path: '/user',
  name: 'user',
  component: User,
  meta: {
    title: "用户页"
  }
},
```

main.js：
```js
router.beforeEach((to, from, next) => {
  /* 路由发生变化的时候修改页面title */
  if (to.meta.title) {
    document.title = to.meta.title
  }
  next()
})
```
