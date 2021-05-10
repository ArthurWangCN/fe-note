# Vue 多系统切换实现方案

分好几个后台模块，统一使用vue+elementUi框架开发，每一个后台模块都是单独团队开发的。并且**几个系统整体的风格、布局一样的，包括左侧边栏，上方的面包屑**等
用户在使用的时候，可能要切换别的系统就要在浏览器里，新打开窗口，再输入网址，回车。
总结来说，低效，所以现在想将几个系统融合到一个里边，并且**每次切换系统的时候保留用户的操作**。



## 思路

1.结合**iframe**开发上方**系统切换**的组件
2.各个子系统有自己的域名



## 实现

因为每个页面都需要这个切换功能，所以我们直接在app.vue里开发。我是用vue+element开发的，所以切换的地方直接用了ele的tab切换组件。(写法有很多种，主要是思路)
读完这些话再看代码，方便理解：
1.如果用v-if控制每个iframe的显示隐藏，那么切换后系统后，再切换回来，iframe的属性会使页面会刷新，用户的操作不能保留
2.如果纯粹用elementUi的tab组件来做，页面一进来，就会把每个系统的资源加载进来，卡的要命，所以需要做到按需加载
3.实现：结合1、2问题，用v-if控制页面一进来，只加载一个默认的系统；tab切换的时候再利用v-if去加载该系统的资源；v-if只控制一次，不会随着tab的切换变化，这样就能保证切换系统时保留了用户的操作。



```vue
<el-tabs v-model="activeName" @tab-click="handleClick">
    <el-tab-pane class="temp" label="VUE" name="first">
      <iframe v-if="ifArr.first" class="ifa" scrolling=auto src="http://panjiachen.github.io/vue-element-admin/#/dashboard" frameborder="0"></iframe>
    </el-tab-pane>
    <el-tab-pane class="temp"  label="SF" name="second">
       <iframe v-if="ifArr.second"   class="ifa" scrolling=auto src="https://segmentfault.com/" frameborder="0"></iframe>
    </el-tab-pane>
    <el-tab-pane class="temp"  label="百度" name="third">
       <iframe v-if="ifArr.third"  class="ifa" scrolling=auto src="https://www.baidu.com/" frameborder="0"></iframe>
    </el-tab-pane>
  </el-tabs>
```



```js
data(){
    return {
        activeName: 'first',
        ifArr: {
            first: true,
            second: false,
            third: false
        }
    }
},
methods: {
    handleClick(tab, event) {
        let flagName = tab.name
        this.ifArr[flagName] = true
    }
}
```





[原文](https://segmentfault.com/a/1190000015167850)
