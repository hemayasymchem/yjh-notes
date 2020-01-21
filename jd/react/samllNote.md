// 随记React学习

### React 渲染组件的过程
[官网](https://react.docschina.org/docs/components-and-props.html)
```
  function Welcome(props) {
      return <div>hello, {props.name}</div>
  }
  const Element = <Welcome name="sara" />
  ReactDOM.render(
      Element,
      document.getElementById('root')
  )

  步骤
   1 调用ReactDOM.render函数 并传入 <Welcome name="sara" />作为参数
   2  React调用Welcome组件， 并将{name: 'sara'}作为props传入
   3 Welcome组件 将<div>hello, sara</div>元素作为返回值
   4 ReactDOM 高效地更新为 <div>hello, sara</div>

```
### React组件的一个严格规则
    所有React组件都必须像纯函数一样保护他们的props不被更改

    state类似于props 但是 state是私有的 完全受控于当前组件

    props 和 state是 React本身设置的，且有特殊的含义，但其实你可以向class中随意添加不参数数据流的额外数据

### 正确的使用state
[官网](https://react.docschina.org/docs/state-and-lifecycle.html)
```
  1 不要直接修改state 而是 this.setState({}) 构造函数是唯一可以给state赋值的地方
  2 State的更新可能是异步的 出于性能考虑React可能会把多个setState()调用合并成一个

    错误代码
    this.setState({
        counter: this.state.counter + this.props.increase
    })
    解决这个问题 setState() 可以接受一个函数啊 参数分别是上一个state 和 更新被应用时的 props
    this.setState((state, props) => {
        counter: state.counter + props.increase
    })

   3 state的更新会并合并
     constructor() {
         super()
         this.state = {
             post: [],
             comment: []
         }
     }
     componentDidMount() {
         fetchPost().then(res => {
             this.setState({
                 post: res.data
             })
         })
         fetchComment().then(res => {
             this.setState({
                 comment: res.comment
             })
         })
     }
     这里的合并是浅合并
```

### React时间的绑定
[官网地址](https://react.docschina.org/docs/handling-events.html)
```
一般是这么写
constructor() {
    this.handleClick = this.handleClick.bind(this)
}
handleClick() {
    this.setState({})
}
render() {
    return (
        <div onClick={this.handleClick}>点我</div>
    )
}
在constructor里面 使用bind 重新绑定一次

不想这么麻烦的其他写法 使用箭头函数
 1 
  render() {
      return (
          <div onClick={this.handleClick}>点击</div>
      )
  }
  handleClick = () => { // 直接将handleClick写成箭头函数
      this.setState({})
  }

 2 render() {
     return (
         <div onClick={(e) => this.handleClick(e)}>点我</div> // 点击事件写成箭头函数 但是这里需要加参数e
     )
   } 
   handleClick() {
       this.setState({})
   }
 倾向于使用第一种 第二种在该回调函数作为props传入子组件时 可能会进行额外的渲染
```

### React事件传递参数
```
  两种方式等价
  <Button onClick={(e) => this.delete(id, e)}></Button> 参数放在前面
  <Button onClick={this.delete.bind(this, id)}></Button> 参数写在第二位  
```

### React循环渲染组件的两种方式 第二种方式可能造成滥用

例子1 
function List(props) {
    const names = props.name
    const ListItems = names.map(name =>
        <ListItem name={name} key={name}></ListItem>
    )
    return (
        <ul>{ListItems}</ul>
    )
}
例子2 
function List(props) {
    const names = props.names
    return (
        <ul>
          {names.map(name =>
              <ListItem name={name} key={name}>
          )}
        </ul>
    )
}