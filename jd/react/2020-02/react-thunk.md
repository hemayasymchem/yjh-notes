[掘金参考资料一 react-thunk 和 react-sage ](https://juejin.im/post/5dc38b776fb9a04a6b204dbf)
```
什么事中间件
let next = store.dispatch
store.dispatch = function dispatchAndLog(action) {
    console.log('dispatching', action)
    next(action)
    console.log('next state', store.getState())
}
对store.dispatch进行了重定义 在发送action前后添加了打印功能 这就是中间件的雏形
中间件就是一个函数 对store.dispatch 方法进行了改造  在发出Aciton 和 执行Reducer这两步间添加了其他功能

为什么要使用中间件
redux的数据流大致是：
 UI -> action(plain) -> reducer -> state -> UI
 redux遵循函数式编程的规则 。。。
 中间件的作用就是；转换异步操作 生成原始的action 这样reducer就能处理相应的action 从而改变state 更新UI

```

[redux入门教程(二) 中间件和异步操作](https://www.cnblogs.com/chaoyuehedy/p/9713167.html)
```
随便写写  看不下去了
  对store.dispatch重写
  let next = store.dispatch
  store.dispatch = function dispatchAndLog(action) {
      console.log('dispatching', acton)
      next(action)
      console.log('next state', store.getState())
  }

  中间件的使用
  const store = createStore(
      reducer,
      initial_state,
      applyMiddleware(...middleware) // 中间件的次序是有讲究的
  )


  applyMiddleware() 源码

  export default function applyMiddleware(...middlewares) {
      return (craeteStore) => (reducer, preloadState, enhancer) => {
          var store = createStore(reducer, preloadedState, enhancer)
          var dispatch = store.dispatch
          var chain = []
          var middlewareAPI = {
              getState: store.getState,
              dispatch: (action) => dispatch(action)
          }
          chain=middlewares.map(middleware => middleware(middlewareAPI))
          dispatch = compose(...chain)(store.dispatch)
          return {...store, dispatch}
      }
  }


  redux-thunk中间件
    异步操作至少要送出两个action 用户触发第一个action 如何触发第二个action 奥妙就在 Action Creator中
    class AsyncApp extends Component {
        componentDidMount() {
            const { dispatch, selectedPost } = this.props
            dispatch(fetchPosts(selectedPost))
        }
    }
```

[cosChina redux中间件 redux-thunk的使用](https://my.oschina.net/u/4018697/blog/2961033)
```
  我的理解 普通的action是个对象 异步的action 是个 函数
  redux-thunk源码分析
  function createThunkMiddleware(extraArgument) {
    return ({dispatch, getState}) => next => action => {
        if (typeof action === 'function') {
            return action(dispatch, getState, extraArgument)
        }
        return next(action)
    }
  }
  const thunk = createThunkMiddleware();
    thunk.withExtraArgument = createThunkMiddleware; 
    export default thunk
```