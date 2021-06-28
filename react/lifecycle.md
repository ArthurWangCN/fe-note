## 旧版生命周期
旧版生命周期 指的是 React 16.3 及其之前的版本。

![old lifecycle](https://user-gold-cdn.xitu.io/2020/6/9/172968b84a735a6d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

初始化的时候不会把赋值算作更新，所以不会执行更新阶段

```js
import React, { Component } from 'react'

export default class LifeCycle extends Component {
    //// props = {age:10,name:'计数器'}
  static defaultProps = {
      name:'计数器'
  }
  constructor(props){
      //Must call super constructor in derived class before accessing 'this' or returning from derived constructor
    super();//this.props = props;
    this.state = {number:0,users:[]};//初始化默认的状态对象
    console.log('1. constructor 初始化 props and state');
  
  }  
  //componentWillMount在渲染过程中可能会执行多次
  componentWillMount(){
    console.log('2. componentWillMount 组件将要挂载');
    //localStorage.get('userss');
  }
  //componentDidMount在渲染过程中永远只有执行一次
  //一般是在componentDidMount执行副作用，进行异步操作
  componentDidMount(){
    console.log('4. componentDidMount 组件挂载完成');
    fetch('https://api.github.com/users').then(res=>res.json()).then(users=>{
        console.log(users);
        this.setState({users});
    });
  }
  shouldComponentUpdate(nextProps,nextState){
    console.log('Counter',nextProps,nextState);
    console.log('5. shouldComponentUpdate 询问组件是否需要更新');
    return true;
  }
  componentWillUpdate(nextProps, nextState){
    console.log('6. componentWillUpdate 组件将要更新');
  }
  componentDidUpdate(prevProps, prevState)){
    console.log('7. componentDidUpdate 组件更新完毕');
  }
  add = ()=>{
      this.setState({number:this.state.number});
  };
  render() {
    console.log('3.render渲染，也就是挂载')
    return (
      <div style={{border:'5px solid red',padding:'5px'}}>
        <p>{this.props.name}:{this.state.number}</p>
        <button onClick={this.add}>+</button>
        <ul>
            {
                this.state.users.map(user=>(<li>{user.login}</li>))
            }
        </ul>
        {this.state.number%2==0&&<SubCounter number={this.state.number}/>}
      </div>
    )
  }
}
class SubCounter extends Component{
    constructor(props){
        super(props);
        this.state = {number:0};
    }
    componentWillUnmount(){
        console.log('SubCounter componentWillUnmount');
    }
    //调用此方法的时候会把新的属性对象和新的状态对象传过来
    shouldComponentUpdate(nextProps,nextState){
        console.log('SubCounter',nextProps,nextState);
        if(nextProps.number%3==0){
            return true;
        }else{
            return false;
        }
    }
    //componentWillReceiveProp 组件收到新的属性对象
    componentWillReceiveProps(){
      console.log('SubCounter 1.componentWillReceiveProps')
    }
    render(){
        console.log('SubCounter  2.render')
        return(
            <div style={{border:'5px solid green'}}>
                <p>{this.props.number}</p>
            </div>
        )
    }
}
```

**洋葱模型**
![洋葱模型](https://user-gold-cdn.xitu.io/2019/12/15/16f0a0b3e1f4f59f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


## 新版生命周期

![新版生命周期](https://user-gold-cdn.xitu.io/2019/12/15/16f0a0b3e20fa9aa?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### static getDerivedStateFromProps

+ static getDerivedStateFromProps(nextProps,prevState)：接收父组件传递过来的 props 和组件之前的状态，返回一个对象来更新 state 或者返回 null 来表示接收到的 props 没有变化，不需要更新 state
+ 该生命周期钩子的作用： 将父组件传递过来的 props 映射 到子组件的 state 上面，这样组件内部就不用再通过 this.props.xxx 获取属性值了，统一通过 this.state.xxx 获取。映射就相当于拷贝了一份父组件传过来的 props ，作为子组件自己的状态。注意：子组件通过 setState 更新自身状态时，不会改变父组件的 props
+ 配合 componentDidUpdate，可以覆盖 componentWillReceiveProps 的所有用法
+ 该生命周期钩子触发的时机：
    + 在 React 16.3.0 版本中：在组件实例化、接收到新的 props 时会被调用
    + 在 React 16.4.0 版本中：在组件实例化、接收到新的 props 、组件状态更新时会被调用
    + 注意：派生状态时，不需要把组件自身的状态也设置进去


### getSnapshotBeforeUpdate

+ getSnapshotBeforeUpdate(prevProps, prevState)：接收父组件传递过来的 props 和组件之前的状态，此生命周期钩子必须有返回值，返回值将作为第三个参数传递给 componentDidUpdate。必须和 componentDidUpdate 一起使用，否则会报错
+ 该生命周期钩子触发的时机 ：被调用于 render 之后、更新 DOM 和 refs 之前
+ 该生命周期钩子的作用： 它能让你在组件更新 DOM 和 refs 之前，从 DOM 中捕获一些信息（例如滚动位置）
+ 配合 componentDidUpdate, 可以覆盖 componentWillUpdate 的所有用法
+ 使用场景：每次组件更新时，都去获取之前的滚动位置，让组件保持在之前的滚动位置


## 版本迁移

+ componentWillMount，componentWillReceiveProps，componentWillUpdate 这三个生命周期因为经常会被误解和滥用，所以被称为 不安全（不是指安全性，而是表示使用这些生命周期的代码，有可能在未来的 React 版本中存在缺陷，可能会影响未来的异步渲染） 的生命周期。
+ React 16.3 版本：为不安全的生命周期引入别名 UNSAFE_componentWillMount，UNSAFE_componentWillReceiveProps 和 UNSAFE_componentWillUpdate。（旧的生命周期名称和新的别名都可以在此版本中使用）
+ React 16.3 之后的版本：为 componentWillMount，componentWillReceiveProps 和 componentWillUpdate 启用弃用警告。（旧的生命周期名称和新的别名都可以在此版本中使用，但旧名称会记录DEV模式警告）
+ React 17.0 版本： 推出新的渲染方式——异步渲染（ Async Rendering），提出一种可被打断的生命周期，而可以被打断的阶段正是实际 dom 挂载之前的虚拟 dom 构建阶段，也就是要被去掉的三个生命周期 componentWillMount，componentWillReceiveProps 和 componentWillUpdate。（从这个版本开始，只有新的“UNSAFE_”生命周期名称将起作用）


