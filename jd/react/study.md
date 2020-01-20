/**
 * 学习react一些自己认为需要记录的东西
 */


### React.createClass 和 extends Component的区别
[掘金地址](https://juejin.im/post/5b508a545188251b186bbe16)

`
首先需要注意的是 React.createClass 以前引入React的就可以使用
    高版本的React 需要引入 create-react-class 包
`
```
区别在于 语法区别  propsType 和 getDefaultProps 状态区别 this的区别 Mixins 五处区别

语法 const demoCom = React.createClass({ render() { return (<div></div>)}})

     class demoCom extends Component {
         constructor(props) { super(props) }
         render() {
             return ( <div></div>)
         }
     }

```

### react-router-dom 5.x.x 使用简介
[csdn地址](https://lengyuexin.blog.csdn.net/article/details/103251514)

```
基础用法中对于Route组件，支持的渲染方式不唯一
  具体的代码是
  <Switch>
    <Route exact path="/"> <Home /> <Route>
    <Route path="/about/> component={About}</Route>
    <Route path="/dashBoard" children={<Dashboard/>}></Route>
    <Route path="/news" render={ () => <News />>}></Route>
    <Route path="/game" component={ () => <Game/>}</Route>
  </Switch>


```
