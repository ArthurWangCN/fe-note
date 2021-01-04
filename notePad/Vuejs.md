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

### 4、scoped以及引入css文件
vue-cli中 scoped 的编译原理: 

所谓的局部css，就是通过 `vue-loader` 这个插件，在编译打包的时候将带有 scoped 属性的 css 打上一个 tag，同时将 template 内的所有 html 都打上一个相同的 tag，最后通过 css 的属性选择器定位，造就了所谓的局部css。

`css-loader` 对 import 的文件会当做外部资源，那么我能理解为它是把 import 的 css 文件单独提取出来，并没有把其中的样式放在 `<style scoped>` 中解析，而是最后把 `<style>` 中计算结果和 import 的css文件混合后，交由 `style-loader` 最后解析为 style 标签加载在页面里。

具体：

```html
/* 这样写的话import的css文件会被编译为全局样式，但是引入less等预编译文件，就>会局部生效 */
<style scoped>
    @import '../../assets/css/home.css';  
</style>
```

```html
/* 这样写的css文件中的样式只能在本组件中使用，而不会影响其他组件 */
<style src="../../assets/css/home.css" scoped>
</style>
```

