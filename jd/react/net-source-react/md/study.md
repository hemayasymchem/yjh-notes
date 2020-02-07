### 项目中使用到的npm包

    1 react-hot-loader
    2 query-string (我觉得应该是和qs差不多吧)
    3 js-cookie (设置cookie的)
    4 connected-react-router
      是一个绑定react-router到redux的组件，来实现双向绑定router的数据到redux store中
      可以在action中实现对路由的操作
      项目中使用到的 connectRouter routerMiddleWare
    5 history 历史记录库 用到的 createBrowserHistory
    6 redux-thunk 用到的是thunk 因为redux store中仅仅支持同步数据流 使用thunk等中间件 可以实现异步
    7 react-lazy-load 图片懒加载

### 关于React的
    1 componentDidCatch React内置函数 如果render函数抛出异常 就会触发这个函数   
    2 { PureComponent} React中的



### rudux 
    1 applyMiddleWare
    2 compose
    3 window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ (这句代码还不知道什么意思 应该是调试工具之类的)

###  Router
    1 react-helmet 使用到了 Helmet React文档头管理器
    2 react-loadable 代码分割 并提供loader时候的组件

### 其他的npm包

    1 normalize 
    2 rmada
      rmada 中的 { omit path clone }
    3 @jdcfe/yep-react 京东自己的移动端包



### 其他

     1 webpack 热加载问题
       代码
       if (module.hot) {
           module.hot.accept('./reducer', () => {
               store.replaceReducer(connectRouter(history)(rootReducer))
           })
       }