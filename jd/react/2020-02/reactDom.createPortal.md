[地址链接](http://www.ptbird.cn/react-portal-createPortal.html)
[简述参考地址](https://www.jianshu.com/p/810e8104f72f)

```
简而概之 具体的看文档

一般的React Modal组件 都是挂到父组件的this.props.children上 被最近的父组件捕获
而 Protals 则提供了一种将组件直接挂载到直接父组件 DOM 层次之外的一类方式

react-dom 提供的具体方法是 ReactDOM.createPortals(child, container)


一 之前的Modal
   const styles = {
      modal: {
          postion: '', left: '', ...
      }
   }
   class Modal extends Component {
       constructor(props) {
           super(props)
       }
       render() {
           return (
               <div style={styles.modal}>
                 { this.props.children}
               </div>
           )
       }
   }

   使用的时候
   <div className="app">
     <Modal>
       <h1>Modal</h1>
     </Modal>
   </div>
```

```
项目中的使用

class HomePop extends Component {
    render() {
        return reactDOM.createProtals(
            <div classnName="pop-box">
              <div className="pop-mask" />
              <div className="pop-body">
                ... 省略代码
                <p href="#" onClick={this.props.vipFun} className="clickBuy">
              </div>
            </div>, document.body
        )
    }
}
意思是 组件挂载在了document.body上 但是点击购买还是执行的 父组件传递过来的 vipFun方法
```